#Empezando con Elm
 
##Pruébalo de manera online

No todos quieren tener instalado todo una vez, así que tu puedes seguir esta guía con el [editor online](http://elm-lang.org/try). Usted no tendrá el REPL para la selección “Lenguaje fundamental”, pero tolo lo demás va a funcionar muy bien. 

Tu puedes ver el editor online en acción con estos [ejemplos](http://elm-lang.org/examples) de pequeños programas de Elm.

##Instalación 

Tu puedes instalar la “Plataforma de Elm” que incluye todas las herramientas de la línea de comandos que necesitas para trabajar con Elm.

Mac - [Instalar](http://install.elm-lang.org/Elm-Platform-0.17.1.pkg)
Windows - [Instalar](http://install.elm-lang.org/Elm-Platform-0.17.1.exe)
Cualquiera — [instalador npm](https://www.npmjs.com/package/elm)  o [construir de la fuente](https://github.com/elm-lang/elm-platform)

Configura tu editor 

Sabemos de la sintasis de Elm destacando modulos durante al menos los siguientes editores de texto:

 - [Atom](https://atom.io/packages/language-elm) 
 - [Brackets](https://github.com/lepinay/elm-brackets) 
 - [Emacs](https://github.com/jcollard/elm-mode)   
 - [IntelliJ](https://github.com/durkiewicz/elm-plugin)
 - [LightTable](https://github.com/rundis/elm-light)
 - [Sublime Text](https://packagecontrol.io/packages/Elm%20Language%20Support)
 - [Vim](https://github.com/lambdatoast/elm.vim)
 - [VS Code](https://github.com/sbrink/vscode-elm)

Si tu no tienes un editor como tal, [Sublime Text](https://packagecontrol.io/packages/Elm%20Language%20Support) esta bien para empezar con el 

## Solución de Problemas 

La manera rápida para aprender cualquier cosa es hablar con otras personas en la comunidad de Elm. Son amables y están felices de ayudarte. Asi que si te quedas atascado durante la instalación o encuentras algo raro visita [Elm Slack](http://elmlang.herokuapp.com/) y pregunta por ello. De hecho, si llegas a estar confuso en cualquier punto mientras estas aprendiendo o usando Elm, pregunta. Puedes ahorrarte horas, ¡Solo hazlo!

¿Qué es la plataforma Elm?

Despues de instalar Elm exitosamente, tu  tienes los siguientes comandos disponibles en tu computadora: 

•	elm-repl
•	elm-reactor
•	elm-make
•	elm-package
Cada uno tiene una bandera –help que mostrará más información. Pero vamos a verlos a continuación.
 

Elm-repl 

Elm-repl vamos a jugar con simples expresiones de Elm.
 ‘’’’

Nota: ‘elm-repl’ trabaja compilando código de JavaScript, asegúrate que tienes instalado Node.js. 


Elm-reactor 
Elm-reactor nos ayuda a construir proyectos de Elm sin jugar con la línea de comandos demasiado. Usted acaba de ejectuar la raiza del proyecto, de esta forma:


Esto empieza un servidor http://localhost:8000.  Tu puedes desplazarte a un archivo Elm y ver como se ve. 

Banderas notables:
--port te premite elegir algo mas que el puerto 8000. Por lo tanto se puede decir de que elm-reactor --port=8123 para hacer correr el código en la dirección elm-reactor --port=8123.
--address esto remplaza localhost con otra dirección. Por ejemplo, tu quieres usar elm-reactor –adress=0.0.0.0 si tu quieres probar el programa Elm en un dispositivo móvil en tu red local.


Elm-make 
Elm-make construye proyectos Elm. Puedes compilar código Elm a HTML o JavaScript. Es la manera más general de compilar código Elm, así que si tu proyecto es demasiado avanzado para elm-reactor, tendrás que utilizar elm-make directamente.

Si tu quieres compilar Main.elm a un archivo HTML llamado main.html. Tu debes de correr este comando: 
elm-make Main.elm --output=main.html

Banderas notables: 
--warn imprime los warnings para mejorar la calidad del código.

Elm-package

Elm-package descarga y publica paquetes de nuestra pagina de catalogo. Como miembros de la comunidad resolvemos problemas de una manera agradable, comparten su código en el catalogo de paquetes para que cualquiera lo use.

Digamos que quieres utilizar evancz/elm-http y NoRedInk/elm-decode-pipeline para hacer solicitudes http a un servidor y mostrar el JSON resultante en los valores de Elm. Tú dirías: 

elm-package install evancz/elm-http
elm-package install NoRedInk/elm-decode-pipeline

Esto añade las dependencias a tu archivo elm-package.json que describe tu proyecto. (¡O crearlo si no tiens uno todavía!).

Comandos notables; 
Install; instala las dependencias en elm-package.json
Publish: publica tu libreri para el catalogo de paquetes de Elm
Bump: números de versión en base a cambios en la API.
Diff: obtiene la diferencia entre dos APIs.

