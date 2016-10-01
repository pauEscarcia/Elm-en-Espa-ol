# Web Sockets 

Vamos a hacer una sencilla aplicación de chat. Habrá un campo de texto para que pueda escribir cosas en una región y que muestra todos los mensajes que hemos recibido hasta ahora. Web sockets  son excelentes para este escenario porque nos permiten configurar una conexión persistente con el servidor. Esto significa:

Puede enviar mensajes de forma barata siempre que lo desee.
El servidor puede enviar mensajes cada vez que se siente como él.

En otras palabras, `WebSocket` es una de las pocas bibliotecas que hace uso de ambos comandos y suscripciones.

Este programa pasa a ser bastante corto, por lo que aquí es lo más completo:

```
import Html exposing (..)
import Html.App as App
import Html.Attributes exposing (..)
import Html.Events exposing (..)
import WebSocket


main =
  App.program
    { init = init
    , view = view
    , update = update
    , subscriptions = subscriptions
    }


-- MODEL

type alias Model =
  { input : String
  , messages : List String
  }


init : (Model, Cmd Msg)
init =
  (Model "" [], Cmd.none)


-- UPDATE

type Msg
  = Input String
  | Send
  | NewMessage String


update : Msg -> Model -> (Model, Cmd Msg)
update msg {input, messages} =
  case msg of
    Input newInput ->
      (Model newInput messages, Cmd.none)

    Send ->
      (Model "" messages, WebSocket.send "ws://echo.websocket.org" input)

    NewMessage str ->
      (Model input (str :: messages), Cmd.none)


-- SUBSCRIPTIONS

subscriptions : Model -> Sub Msg
subscriptions model =
  WebSocket.listen "ws://echo.websocket.org" NewMessage


-- VIEW

view : Model -> Html Msg
view model =
  div []
    [ div [] (List.map viewMessage model.messages)
    , input [onInput Input] []
    , button [onClick Send] [text "Send"]
    ]


viewMessage : String -> Html msg
viewMessage msg =
  div [] [ text msg ]
```

Las partes interesantes son, probablemente, los usos de `WebSocket.send` y `WebSocket.listen`.

Por simplicidad vamos a apuntar a un simple servidor que apenas hace eco de nuevo lo que se ingrese. Por lo que no será capaz de tener las conversaciones más interesantes en la versión básica, pero es por eso que tenemos ejercicios en estos ejemplos!
