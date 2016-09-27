#Lo básico del lenguaje Elm (Core Language)

Esta sección nos hablará de la base del lenguaje Elm. Así que primero se debe de tener todo para luego familiarizarse con las herramientas de la línea de comandos. Una vez que esté todo listo, para empezar debemos de poner `elm-repl` en la  terminal. Se debería ver algo como esto:
 

     ---- elm repl 0.17.0 -----------------------------------------------------------
     :help for help, :exit to exit, more at <https://github.com/elm-lang/elm-repl>
    --------------------------------------------------------------------------------
    >
REPL imprime el tipo de cada resultado, se abarcaran los valores, funciones, listas, tuplas y registros . Estos bloques de construcción se parecen a las estructuras de lenguajes como JavaScript, Python y Java. 

##Valores 
Vamos a empezar con algunas cadenas:
```
> "hello"
"hello"

> "hello" ++ "world"
"helloworld"

> "hello" ++ " world"
"hello world" 
```
Elm usa el operador `(++)`  para poner las cadenas juntas. Nota que ambas cadenas se conservan tal y como son cuando se ponen juntas, asi que cuando combinamos  “`hello”` y `“Word”` el resultado no tiene espacios.
Las matemáticas se ven normales también:

```
> 2 + 3 * 4
14

> (2 + 3) * 4
20
``` 
A diferencia de JavaScript, Elm hace una distinción entre los números enteros y de coma flotante. Al igual que Python 3, no es tanto la división de punto flotante `(/)` y la división de enteros `(//)`.
```
> 9 / 2
4.5

> 9 // 2
4
```
##Funciones 
Vamos a empezar por escribir una función `isNegative` que toma algún número y comprueba si es menor que cero. El resultado será `True` o `Falso`.
```
> isNegative n = n < 0
<function>

> isNegative 4
False

> isNegative -7
True

> isNegative (-3 * -4)
False
```

Observe que la aplicación de funciones se ve diferente que en lenguajes como Javascript y Python y Java. En lugar de envolver todos los argumentos entre paréntesis y separándolos con comas, utilizamos espacios para aplicar la función. Así que `(add(3,4))` se convierte `(add 3 4 )` que termina evitando un montón de paréntesis  y comas cuando las cosas se hacen más grandes. En última instancia, esto se ve mucho más limpio, una vez que se acostumbre a ella! El paquete de elm-html es un buen ejemplo de cómo esto hace que se vea la luz.

##Expresiones if 
Cuando usted quiere tener un comportamiento condicional en Elm, se utiliza una expresión if.
```
> if True then "hello" else "world"
"hello"

> if False then "hello" else "world"
"world"
```
 La palabra reservada `if` `then` `else` son usadas para separar la condicional y las dos ramas no necesitan ningún paréntesis o llaves.

Elm no tiene una noción de "truthiness" por lo que los números, las cadenas y listas no se pueden utilizar como valores booleanos. Si lo intentamos, Elm nos dirá que tenemos que trabajar con un valor booleano verdadero.

Ahora vamos a hacer una función que nos dice si un número es más de 9000.
```
 > over9000 powerLevel = \
|   if powerLevel > 9000 then "It's over 9000!!!" else "meh"
<function>

> over9000 42
"meh"

> over9000 100000
"It's over 9000!!!"
```
 El uso de una barra invertida en el REPL nos deja que las cosas divididas en varias líneas. Utilizamos esta en la definición de over9000 anteriormente. Por otra parte, es la mejor práctica para llevar siempre el cuerpo de una función por una línea. Hace las cosas mucho más uniformes y fáciles de leer, por lo que desea hacer esto con todas las funciones y los valores se definen en el código normal.

##Listas 
Las listas son una de las estructuras de datos más comunes en Elm. Llevan a cabo una secuencia de cosas relacionadas, similar a las matrices de JavaScript.

Las listas pueden contener muchos valores. todos esos valores deben ser del mismo tipo. Aquí hay algunos ejemplos que utilizan funciones de la [biblioteca de listas](http://package.elm-lang.org/packages/elm-lang/core/latest/List):
```
> names = [ "Alice", "Bob", "Chuck" ]
["Alice","Bob","Chuck"]

> List.isEmpty names
False

> List.length names
3

> List.reverse names
["Chuck","Bob","Alice"]

> numbers = [1,4,3,2]
[1,4,3,2]

> List.sort numbers
[1,2,3,4]

> double n = n * 2
<function>

> List.map double numbers
[2,8,6,4]
```
 Una vez más, todos los elementos de la lista deben tener el mismo tipo.

##Tuplas 

Las tuplas son otra estructura de datos útiles. Una tupla puede contener un número fijo de valores, y cada valor puede tener cualquier tipo. Un uso común es si tiene que devolver más de un valor de una función. La siguiente función recibe un nombre y da un mensaje para el usuario:
```
> import String

> goodName name = \
|   if String.length name <= 20 then \
|     (True, "name accepted!") \
|   else \
|     (False, "name was too long; please limit it to 20 characters")

> goodName "Tom"
(True, "name accepted!")
```
 Esto puede ser muy útil, pero cuando las cosas comienzan a ser más complicadas, a menudo es mejor usar los registros en lugar de tuplas.


##Records (Registros).

Un registro es un conjunto de pares de valores clave, de forma similar a los objetos en JavaScript o Python. Usted encontrará que son extremadamente comunes y útiles en Elm! Veamos algunos ejemplos básicos.
```
> point = { x = 3, y = 4 }
{ x = 3, y = 4 }

> point.x
3

> bill = { name = "Gates", age = 57 }
{ age = 57, name = "Gates" }

> bill.name
"Gates"
```
 Así podemos crear registros utilizando llaves y campos de acceso que utilicen un punto. Elm también tiene una versión de acceso al registro que funciona como una función. Al comenzar la variable con un punto, que está diciendo por favor acceder al campo con el siguiente nombre. Esto significa que `.name` es una función que obtiene el campo de nombre del registro.

```
> .name bill
"Gates"

> List.map .name [bill,bill,bill]
["Gates","Gates","Gates"] 
```
Cuando se trata de hacer las funciones de registros, se puede hacer un poco de coincidencia de patrones para hacer las cosas un poco más ligero.

```
> under70 {age} = age < 70
<function> 

> under70 bill
True

> under70 { species = "Triceratops", age = 68000000 }
False
``` 
Así podemos pasar cualquier registro en la medida en que tiene un campo de  ´age ´(edad) que tiene un número.

A menudo es útil para actualizar los valores de un registro.

```
> { bill | name = "Nye" }
{ age = 57, name = "Nye" }

> { bill | age = 22 }
{ age = 22, name = "Gates" }
``` 
Es importante notar que no hacemos cambios destructivos. Cuando actualizamos algunos campos de ´bill´ creamos un nuevo registro en lugar de sobrescribir el existente. Elm acelera esta operación, compartiendo tanto contenido como sea posible. Si actualiza uno de los diez campos, el nuevo disco compartirá los nueve valores sin cambios.

##Comparando registros y objetos 

Registros en Elm son similares a los objetos en JavaScript, pero tienen unas diferencias cruciales. Las principales  diferencias son que con los registros: 

* No se puede pedir para un campo que no existe.
* Ningún campo volverá a ser indefinido o nulo.
* No se pueden crear registros recursivas con `this` o `self` `keyword`.

Elm fomenta una separación estricta de los datos y la lógica, y la capacidad de decir esto se utiliza principalmente para romper esta separación. Este es un problema sistémico en lenguajes orientados a objetos que Elm está evitando deliberadamente.

Los registros también apoyan la tipificación estructural que significa registros en Elm se pueden utilizar en cualquier situación, siempre y cuando existan los campos necesarios. Esto nos da una flexibilidad sin comprometer la fiabilidad.
