#Introducción al lenguaje Elm 

Es un lenguaje funcional  que compila para JavaScript. Compite con proyectos como React (React es una librería de JavaScript para crear interfaces de usuario de Facebook e Instagram. Muchas personas piensan que React es la V en MVC)  como una herramienta para la creación de sitios y aplicaciones web. Elm tiene  un fuerte énfasis en la simplicidad, facilidad de uso y herramientas de calidad.

Con esta guía se podrá: 

 - Aprender los fundamentos de la programación en Elm. 
 - Mostrar que se pueden hacer aplicaciones interactivas  con la arquitectura de Elm.
 - Enfatizar los principios y los patrones que generalizan a la
   programación en cualquier lenguaje.

Se garantiza  que si se le da la oportunidad a Elm  y realmente se desarrolla un proyecto en el, el resultado final será escribir mejor JavaScript y código React.  ¡Realizar las ideas es muy fácil!

##Una muestra rápida 
Por supuesto pensar en Elm es bueno, mira por ti mismo: 

Este es un simple contador. Si se observa el código, solo va a incrementar y a disminuir el contador:
    
    import Html exposing (Html, button, div, text)
    import Html.App as App
    import Html.Events exposing (onClick)
    
    main =
      App.beginnerProgram { model = 0, view = view, update = update }
    
    type Msg = Increment | Decrement
    
    update msg model =
      case msg of
        Increment ->
          model + 1
    
        Decrement ->
          model - 1
    
    view model =
      div []
        [ button [ onClick Decrement ] [ text "-" ]
        , div [] [ text (toString model) ]
        , button [ onClick Increment ] [ text "+" ]
        ]
Se tiene en cuenta que `update` y `view` se desacoplan totalmente. Se describe el código HTML de una manera declarativa y Elm se encarga de jugar  con el DOM.

##Porque un lenguaje funcional 

Olvida lo que has oído hablar de la programación funcional. Palabras de lujo, ideas extrañas, malas herramientas, etc. En Elm:

 - No hay errores de ejecución en la práctica. No `null`, no `undefined` no es una función. 
 - Mensajes de error que le ayudaran a agregar funciones con mayor
   rapidez.
 - Código que se mantiene con buena arquitectura como su aplicación y
   buena arquitectura cuando la aplicación crece.
 - La aplicación automática de versiones semántica para todos los
   paquetes de elm.

Ninguna combinación de librerías JS podría darte esto, sin embargo, todo es libre y fácil en Elm. Ahora bien, estas cosas agradables solamente son posibles porque Elm se basa en más de 40 años de trabajo en los lenguajes funcionales mecanografiados. Así Elm es un lenguaje funcional debido a que los beneficios prácticos valen el par de horas que invertirá en la lectura de esta guía.




