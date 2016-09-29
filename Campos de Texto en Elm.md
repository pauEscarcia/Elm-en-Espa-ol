#Campos de Texto 

Estamos a punto de crear una aplicación sencilla que invierte el contenido de un campo de texto. Este ejemplo también introduce algunas cosas nuevas que nos ayudará en nuestro siguiente ejemplo.

De nuevo, este es un programa muy corto, por lo que he incluido todo aquí.  Puedes hacer una lectura rápida para tener una idea de cómo es  todo.

```
import Html exposing (Html, Attribute, div, input, text)
import Html.App as App
import Html.Attributes exposing (..)
import Html.Events exposing (onInput)
import String


main =
  App.beginnerProgram { model = model, view = view, update = update }


-- MODEL

type alias Model =
  { content : String
  }

model : Model
model =
  { content = "" }


-- UPDATE

type Msg
  = Change String

update : Msg -> Model -> Model
update msg model =
  case msg of
    Change newContent ->
      { model | content = newContent }


-- VIEW

view : Model -> Html Msg
view model =
  div []
    [ input [ placeholder "Text to reverse", onInput Change ] []
    , div [] [ text (String.reverse model.content) ]
    ]

```

Se configura un modelo. Se definen algunos mensajes. Usted dice que la forma de actualizar `update`. Usted hace su vista `view`. La diferencia es sólo en la forma en que nos llena este esqueleto. 

Como siempre, se empieza por adivinar lo que debe ser su `Model`. En nuestro caso, sabemos que vamos a tener que hacer un seguimiento de lo que el usuario ha introducido en el campo de texto. Necesitamos que la información así que sabemos cómo representar el texto invertido.

```
type alias Model =
  { content : String
  }


```
Esta vez he elegido para representar el modelo como un registro. 

> Nota: Es posible que se esté preguntando, ¿por qué molestarse en tener un registro aunque sólo tenga una entrada? ¿No podrías utilizar la cadena directa? ¡Si por supuesto! Pero a partir de un registro hace que sea fácil agregar más campos como nuestra aplicación se vuelva más complicada. Cuando llegue el momento en que queremos dos entradas de texto, vamos a tener que hacer mucho menos.

Está bien, así que tenemos nuestro modelo. Ahora en esta aplicación sólo hay un tipo de mensaje de verdad. El usuario puede cambiar el contenido del campo de texto.

```
type Msg
  = Change String


```

Esto quiere decir o función de actualización sólo tiene que manejar esto uno de los casos:

```
update : Msg -> Model -> Model
update msg model =
  case msg of
    Change newContent ->
      { model | content = newContent }


```

Cuando recibimos el nuevo contenido, nosotros utilizamos  el registro para actualizar la  sintaxis del contenido.

Por último hay que decir cómo ver nuestra aplicación:

```
view : Model -> Html Msg
view model =
  div []
    [ input [ placeholder "Text to reverse", onInput Change ] []
    , div [] [ text (String.reverse model.content) ]
    ]


```

Creamos un `<div>` con dos hijos.

El hijo nteresante es el  nodo `<input>`. Además del atributo marcador de posición `placeholder` , que utiliza `onInput` para declarar qué mensajes deben ser enviados cuando el usuario escribe en esta entrada.

Esta función `onInput` es bastante interesante. Se toma un argumento, en este caso el cambio `Change` de función que se creó cuando declaramos el `Msg` de error:

```
Change : String -> Msg
```

Esta función se utiliza para etiquetar todo lo que es actualmente en el campo de texto. Así que digamos que el campo de texto tiene actualmente `yol` y el usuario escribe `o`. Esto desencadena un `input` evento de entrada, por lo que recibirá el mensaje de `Change"Yolo"` en nuestra función de actualización `update`.
