#La arquitectura de Elm 

La arquitectura Elm es un simple patrón  para la arquitectura de aplicaciones web. Es ideal para la modularidad, la reutilización de código y pruebas. Ultimamente, hace que sea fácil crear aplicaciones web complejas que se mantienen sanos a medida que refactorizar y agregar funciones.


Esta arquitectura parece surgir de forma natural en Elm. En primer lugar, observamos los juegos de la comunidad Elm. Luego, en aplicaciones web como TodoMVC y dreamwriter también. Ahora vemos que se ejecuta en la producción en empresas como NoRedInk y CircuitHub. La arquitectura parece ser una consecuencia del diseño del propio Elm, por lo que te va a pasar si usted sabe sobre él o no. Esto ha demostrado ser muy agradable para la incorporación de nuevos desarrolladores. Su código sólo resulta buena arquitectura. Es un poco espeluznante.

Así que la arquitectura en Elm es fácil en Elm, pero es útil en cualquier proyecto de front-end. De hecho, proyectos como Redux se han inspirado en la arquitectura Elm, por lo que es posible que ya han visto los derivados de este patrón. El punto es, en última instancia, incluso si usted no puede utilizar Elm en el trabajo, sin embargo, va a obtener una gran cantidad de uso de Elm y la internalización de este patrón.


----------


##El patrón básico 

La lógica de todo programa Elm puede verse claramente en partes separadas:

 - **Model** – El estado de la aplicación
 - **Update** -  Una manera de actualizar tu estado
 - **View** – Una manera de ver tu estado como HTML

Este patrón es tan fiable que siempre empiezo con el siguiente esqueleto y completar los detalles de mi caso en particular.
```import Html exposing (..)


-- MODEL

type alias Model = { ... }


-- UPDATE

type Msg = Reset | ...

update : Msg -> Model -> Model
update msg model =
  case msg of
    Reset -> ...
    ...


-- VIEW

view : Model -> Html Msg
view model =
  ...
``` 

¡Eso es realmente la esencia de la arquitectura Elm! Vamos a continuar llenando este esqueleto con una lógica cada vez más interesante.
