#Empezando con Elm
 
##Pruébalo de manera online

No todos quieren tener instalado todo una vez, así que  puede  seguir esta guía con el [editor online](http://elm-lang.org/try). No se tendrá el REPL para la selección “Lenguaje fundamental”, pero todo lo demás va a funcionar muy bien. 

Se pude ver el editor online en acción con estos [ejemplos](http://elm-lang.org/examples) de pequeños programas de Elm.

##Instalación 

Se puede instalar la “Plataforma de Elm” que incluye todas las herramientas de la línea de comandos que se necesitan para trabajar con Elm.

* Mac - [Instalar](http://install.elm-lang.org/Elm-Platform-0.17.1.pkg)
* Windows - [Instalar](http://install.elm-lang.org/Elm-Platform-0.17.1.exe)
* Cualquiera — [instalador npm](https://www.npmjs.com/package/elm)  o [construir de la fuente](https://github.com/elm-lang/elm-platform)

### Configura tu editor 

Se sabe que la sintaxis de Elm destaca módulos durante al menos los siguientes editores de texto:

 - [Atom](https://atom.io/packages/language-elm) 
 - [Brackets](https://github.com/lepinay/elm-brackets) 
 - [Emacs](https://github.com/jcollard/elm-mode)   
 - [IntelliJ](https://github.com/durkiewicz/elm-plugin)
 - [LightTable](https://github.com/rundis/elm-light)
 - [Sublime Text](https://packagecontrol.io/packages/Elm%20Language%20Support)
 - [Vim](https://github.com/lambdatoast/elm.vim)
 - [VS Code](https://github.com/sbrink/vscode-elm)

Si no se tiene un editor como tal, [Sublime Text](https://packagecontrol.io/packages/Elm%20Language%20Support) esta bien para empezar. 

## Solución de Problemas 

La manera rápida para aprender cualquier cosa es hablar con otras personas en la comunidad de Elm. Son amables y están felices de ayudar. Asi que si se atasca durante la instalación o se encuentra algo raro visita [Elm Slack](http://elmlang.herokuapp.com/) y pregunta por ello. De hecho, si se llega a estar confuso en cualquier punto mientras está aprendiendo o usando Elm, pregunte. Puede ahorrarse horas.

## ¿Qué es la plataforma Elm?

Después de instalar Elm exitosamente,se tendrán los siguientes comandos disponibles en la computadora: 

* elm-repl
* elm-reactor
* elm-make
* elm-package

Cada uno tiene una bandera `–-help` que mostrará más información. Pero vamos a verlos a continuación.
 

### Elm-repl 

`Elm-repl` vamos a jugar con simples expresiones de Elm.
 ```
$ elm-repl
---- elm-repl 0.17.1 -----------------------------------------------------------
 :help for help, :exit to exit, more at <https://github.com/elm-lang/elm-repl>
--------------------------------------------------------------------------------
> 1 / 2
0.5 : Float
> List.length [1,2,3,4]
4 : Int
> List.reverse ["Alice","Bob"]
["Bob","Alice"] : List String
```

> Nota: `elm-repl` trabaja compilando cdigo de JavaScript, asegúrate que tienes instalado Node.js. 

### Elm-reactor 
`Elm-reactor` nos ayuda a construir proyectos de Elm sin jugar con la línea de comandos demasiado. Se acaba de ejectuar la raíz del proyecto, de esta forma:
```
git clone https://github.com/evancz/elm-architecture-tutorial.git
cd elm-architecture-tutorial
elm-reactor
```

Esto empieza un servidor `http://localhost:8000`. Puede desplazarse a un archivo Elm y ver como se ve. 

** Banderas notables: **
* `--port` permite elegir algo más que el puerto 8000. Por lo tanto, se puede decir que `elm-reactor --port=8123` para hacer correr el código en la dirección `http://localhost:8123`.
* `--address` esto remplaza `localhost` con otra dirección. Por ejemplo, si quiere usar `elm-reactor –address=0.0.0.0`  quiere probar el programa Elm en un dispositivo móvil en la red local.


### Elm-make 
`Elm-make` construye proyectos Elm. Puede compilar código Elm a HTML o JavaScript. Es la manera más general de compilar código Elm, así que si el proyecto es demasiado avanzado para `elm-reactor`, tendrá que utilizar `elm-make` directamente.

Si quiere compilar `Main.elm` a un archivo HTML llamado `main.html`. Debe de correr este comando: 
`elm-make Main.elm --output=main.html`

** Banderas notables: ** 
* `--warn` imprime los warnings para mejorar la calidad del código.

### Elm-package

`Elm-package` descarga y publica paquetes de la página de catalogo. Como miembros de la comunidad resuelven problemas de una manera agradable, comparten su código en el catalogo de paquetes para que cualquiera lo use.

Digamos que quiere utilizar `evancz/elm-http` y `NoRedInk/elm-decode-pipeline` para hacer solicitudes HTTP a un servidor y mostrar el JSON resultante en los valores de Elm: 

```
elm-package install evancz/elm-http
elm-package install NoRedInk/elm-decode-pipeline
```

Esto añade las dependencias a el archivo `elm-package.json` que describe el proyecto. (¡O crealo si no tiene uno todavía!).

** Comandos notables: **
* `Install:` instala las dependencias en elm-package.json.
* `Publish:` publica tu librería para el catalogo de paquetes de Elm.
* `Bump:` números de versión en base a cambios en la API.
* `Diff:` obtiene la diferencia entre dos APIs.

