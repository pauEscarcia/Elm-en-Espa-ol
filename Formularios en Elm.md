#Formularios

Aquí vamos a hacer una forma rudimentaria. Cuenta con un campo para su nombre, un campo para la contraseña, y un campo para verificar que la contraseña. También vamos a hacer un poco de validación muy simple (ver las dos contraseñas coinciden?) sólo porque es fácil de añadir.

El código es un poco más largo en este caso, pero sigo pensando que es valioso para mirar a través de él antes de entrar en la descripción de lo que está pasando.

```
import Html exposing (..)
import Html.App as App
import Html.Attributes exposing (..)
import Html.Events exposing (onInput)


main =
  App.beginnerProgram { model = model, view = view, update = update }


-- MODEL

type alias Model =
  { name : String
  , password : String
  , passwordAgain : String
  }


model : Model
model =
  Model "" "" ""


-- UPDATE

type Msg
    = Name String
    | Password String
    | PasswordAgain String


update : Msg -> Model -> Model
update msg model =
  case msg of
    Name name ->
      { model | name = name }

    Password password ->
      { model | password = password }

    PasswordAgain password ->
      { model | passwordAgain = password }


-- VIEW

view : Model -> Html Msg
view model =
  div []
    [ input [ type' "text", placeholder "Name", onInput Name ] []
    , input [ type' "password", placeholder "Password", onInput Password ] []
    , input [ type' "password", placeholder "Re-enter Password", onInput PasswordAgain ] []
    , viewValidation model
    ]

viewValidation : Model -> Html msg
viewValidation model =
  let
    (color, message) =
      if model.password == model.passwordAgain then
        ("green", "OK")
      else
        ("red", "Passwords do not match!")
  in
    div [ style [("color", color)] ] [ text message ]

```
Esto es más o menos exactamente cómo se veía nuestro ejemplo campo de texto, sólo que con más campos. Vamos a caminar a través de cómo llegó a ser!

Como siempre, se empieza adivinando en el modelo. Sabemos que va a haber tres campos de texto, así que vamos a ir con eso:
```
type alias Model =
  { name : String
  , password : String
  , passwordAgain : String
  }


```
Genial, parece razonable. Esperamos que cada uno de estos campos se pueden cambiar por separado, por lo que sus mensajes deben dar cuenta de cada uno de esos escenarios.
```
type Msg
    = Name String
    | Password String
    | PasswordAgain String


```

Esto significa que su actualización `update` es bastante mecánico. Sólo actualizar el campo correspondiente:

```
update : Msg -> Model -> Model
update msg model =
  case msg of
    Name name ->
      { model | name = name }

    Password password ->
      { model | password = password }

    PasswordAgain password ->
      { model | passwordAgain = password }
```

Tenemos un poco más elegante de lo normal en nuestra `view`, sin embargo.

```
view : Model -> Html Msg
view model =
  div []
    [ input [ type' "text", placeholder "Name", onInput Name ] []
    , input [ type' "password", placeholder "Password", onInput Password ] []
    , input [ type' "password", placeholder "Re-enter Password", onInput PasswordAgain ] []
    , viewValidation model
    ]


```

Comienza la normalidad: Creamos un `<div>` y poner un par `<input>`. Cada uno tiene un atributo  `onInput` que puede etiquetar cualquier cambio apropiada para nuestra función de actualización `update`. 

Pero para el último hijo que no utilizamos directamente una función de HTML. En lugar de ello llamamos a la función `viewValidation`, que pasa en el modelo actual.

```
viewValidation : Model -> Html msg
viewValidation model =
  let
    (color, message) =
      if model.password == model.passwordAgain then
        ("green", "OK")
      else
        ("red", "Passwords do not match!")
  in
    div [ style [("color", color)] ] [ text message ]
```


Esta función compara en primer lugar las dos contraseñas. Si coinciden, hace  el texto verde y un mensaje positivo. Si no coinciden, hace  el texto rojo y un mensaje de ayuda. Con esa información, producimos un `<div>` colorido un mensaje explicando la situación.

Esto empieza a mostrar los beneficios de tener nuestra biblioteca sea HTML código normal Elm. Parecería muy raro para tocar todo el código en nuestra `view`. En Elm, acaba de refactoriza como lo haría con cualquier otro código!

En esta misma línea, se puede observar que los nodos `<input>` todos se crean con código bastante similares. Decimos que hicimos cada entrada más elegante: hay un exterior `<div>` que contiene un `<span>` y un `<input>` con ciertas clases. No tendría sentido total de romper ese patrón a cabo en una función `viewInput`para que nunca tenga que repetirse. Esto también significa que la cambie en un lugar y todo el mundo recibe el código HTML actualizada.
