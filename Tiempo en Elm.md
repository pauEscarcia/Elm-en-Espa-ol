
# Tiempo

Vamos a hacer un simple reloj.

Hasta ahora nos hemos centrado en los comandos. Con el ejemplo de la aleatoriedad, nos preguntamos por un valor aleatorio. Con el ejemplo HTTP, nos preguntamos por información de un servidor. Ese patrón no funciona muy bien para un reloj. En este caso, queremos sentarnos y escuchar acerca de ciclos de reloj cada vez que se produzcan. Aquí es donde entran en suscripciones.

El código no es demasiado loco aquí, así que voy a incluirlo en su totalidad. Después de leer a través, vamos a volver a las palabras normales que lo explican con mayor profundidad.

```
import Html exposing (Html)
import Html.App as App
import Svg exposing (..)
import Svg.Attributes exposing (..)
import Time exposing (Time, second)



main =
  App.program
    { init = init
    , view = view
    , update = update
    , subscriptions = subscriptions
    }


-- MODEL

type alias Model = Time


init : (Model, Cmd Msg)
init =
  (0, Cmd.none)


-- UPDATE

type Msg
  = Tick Time


update : Msg -> Model -> (Model, Cmd Msg)
update msg model =
  case msg of
    Tick newTime ->
      (newTime, Cmd.none)


-- SUBSCRIPTIONS

subscriptions : Model -> Sub Msg
subscriptions model =
  Time.every second Tick


-- VIEW

view : Model -> Html Msg
view model =
  let
    angle =
      turns (Time.inMinutes model)

    handX =
      toString (50 + 40 * cos angle)

    handY =
      toString (50 + 40 * sin angle)
  in
    svg [ viewBox "0 0 100 100", width "300px" ]
      [ circle [ cx "50", cy "50", r "45", fill "#0B79CE" ] []
      , line [ x1 "50", y1 "50", x2 handX, y2 handY, stroke "#023963" ] []
      ]
```

No hay nada nuevo en las secciones de `MODEL` o `UPDATE`. Las mismas cosas viejas. La función de vista es bastante interesante. En lugar de utilizar HTML, se utiliza la biblioteca `SVG`a dibujar algunas formas. Funciona igual que el HTML sin embargo. Tiene que poner una lista de atributos y una lista de hijos de cada nodo.

Lo importante viene en la sección de `SUBSCRIPTIONS`. La función de `subscriptions` toma en el modelo, y en vez de volver `Sub.none` como en los ejemplos que hemos visto hasta ahora, devuelve una suscripción de la vida real! En este caso `Time.every`:
```
Time.every : Time -> (Time -> msg) -> Sub msg
```

El primer argumento es un intervalo de tiempo. Elegimos para conseguir los ticks  cada segundo. El segundo argumento es una función que convierte la hora actual en un mensaje para la función de `update`. Estamos veces con Tick Tagging lo que el tiempo se convertiría en `Tick 1458863979862`.

Eso es todo lo que hay que establecer una suscripción! Estos mensajes serán alimentados a su función de actualización cada vez que estén disponibles.