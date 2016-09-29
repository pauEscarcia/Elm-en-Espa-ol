
# Random

Estamos a punto de hacer una aplicación de "las tiradas de dados", se genera un número aleatorio entre 1 y 6.

Al escribir código con efectos, por lo general lo rompe en dos fases. La primera fase se trata de conseguir algo en la pantalla, simplemente haciendo lo mínimo para tener algo para trabajar. La segunda fase se está llenando de detalles, acercándose gradualmente a la meta real. Vamos a utilizar este proceso aquí también.

## Fase Uno - El mínimo necesario

Como siempre, se empieza a cabo por adivinar lo que debe ser su modelo:
```
type alias Model =
  { dieFace : Int
  }
```

Por ahora nos limitaremos a realizar un seguimiento de desfigurar como un entero entre 1 y 6. A continuación, me gustaría dibujar rápidamente la función de vista, ya que parece ser el siguiente paso más fácil.

```
view : Model -> Html Msg
view model =
  div []
    [ h1 [] [ text (toString model.dieFace) ]
    , button [ onClick Roll ] [ text "Roll" ]
    ]


```
Así que esto es típico. Mismas cosas que hemos estado haciendo con los ejemplos de entrada del usuario de la arquitectura Elm. Al hacer clic en nuestro `<botón >` que se va a producir un mensaje `roll`, así que supongo que es hora de dar un primer paso en la función de `update` también.
```
type Msg = Roll

update : Msg -> Model -> (Model, Cmd Msg)
update msg model =
  case msg of
    Roll ->
      (model, Cmd.none)
```

Ahora la función de actualización tiene la misma forma general que antes, pero el tipo de retorno es un poco diferente. En lugar de simplemente devolver un `Model`, que produce tanto un `Model` y un comando. La idea es: todavía queremos dar un paso adelante el modelo, pero también queremos hacer algunas cosas. En nuestro caso, queremos pedir Elm para darnos un valor aleatorio. Por ahora, sólo rellenarlo con `Cmd.none` que significa "no tengo comandos, no hacer nada." Vamos a llenar esto con las cosas buenas en la fase dos.

Por último, me gustaría crear un valor de inicialización como esta:

```
init : (Model, Cmd Msg)
init =
  (Model 1, Cmd.none)
```

Aquí se especifica tanto el modelo inicial y algunos comandos que nos gustaría ejecutar inmediatamente cuando se inicia la aplicación. Este es exactamente el tipo de cosas que la actualización está produciendo ahora también.

En este punto, es posible cablear todo y echar un vistazo. Puede hacer clic en el ` <button >` , pero no pasa nada. Vamos a arreglar eso!

## Fase dos - Agregando la Cool Stuff

Lo obvio que falta en este momento es la aleatoriedad! Cuando el usuario hace clic en un botón que queremos mandar Elm para llegar a su generador de números aleatorios internos y darnos un número entre 1 y 6. El primer paso me llevaría hacia ese objetivo sería la adición de un nuevo tipo de mensaje:

```
type Msg
  = Roll
  | NewFace Int


```

Todavía tenemos `roll` de antes, pero ahora le sumamos la nueva cara para cuando Elm nos entrega nuestro nuevo número aleatorio. Eso es suficiente para empezar a rellenar en `update` :
```
update : Msg -> Model -> (Model, Cmd Msg)
update msg model =
  case msg of
    Roll ->
      (model, Random.generate NewFace (Random.int 1 6))

    NewFace newFace ->
      (Model newFace, Cmd.none)
```

Hay dos cosas nuevas aquí. En primer lugar, existe ahora una rama de mensajes `Newface`. Cuando un `Newface` entra, sólo un paso adelante el modelo y no hacer nada. En segundo lugar, hemos añadido un verdadero comando para la rama `Roll`. Este utiliza un par de funciones de la biblioteca aleatoria. Lo más importante es `Random.generate`:

```
Random.generate : (a -> msg) -> Random.Generator a -> Cmd msg


```

Esta función toma dos argumentos. La primera es una función para etiquetar valores aleatorios. En nuestro caso queremos usar `Newface: Int -> Msg`  a su vez el número aleatorio en un mensaje para nuestra función de `update`. El segundo argumento es un "generador", que es como una receta para la producción de ciertos tipos de valores aleatorios. Usted puede tener generadores de tipos simples como `Int` o `Float` o `Bool`, sino también para los tipos de lujo como registros personalizados grandes con una gran cantidad de campos. En este caso, se utiliza uno de los generadores más simples:

```
Random.int : Int -> Int -> Random.Generator Int


```

Proporcionarle un límite inferior y superior en el entero, y ahora tiene un generador que produce enteros en ese rango!

Eso es. Ahora podemos hacer clic y ver el flip número a un nuevo valor!

Así que las grandes lecciones aquí son:

 - Escribir programas poco a poco. Comience con un simple esqueleto, y
   añadir poco a poco el material más duro.
 - La función de update produce ahora un nuevo modelo y un comando.
 - No se puede simplemente obtener valores aleatorios de cualquier
   manera. Se crea una orden, y Elm va a ir a hacer algún trabajo entre
   bastidores para proporcionarle ese servicio.
 - De hecho, cada vez que el programa necesita para obtener valores
   fiables (aleatoriedad, HTTP / S de archivos, bases de datos lee,
   etc.) tiene que ir a través de Elm.

En este punto, la mejor manera de mejorar su comprensión de los comandos es sólo para ver más de ellos! Ellos van a aparecer prominentemente con las bibliotecas HTTP y WebSocket, por lo que si usted se siente inestable, el único camino a seguir es practicar con el azar y jugando con otros ejemplos de comandos!