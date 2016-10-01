## HTTP

Estamos a punto de hacer una aplicación que recupera un archivo GIF aleatorio cuando el usuario pida otra imagen.

Ahora, voy a asumir que usted acaba de leer el ejemplo aleatoriedad. (1) presenta un proceso de dos pasos para escribir aplicaciones como esta y (2) muestra el tipo más simple de comandos posibles. Aquí vamos a estar utilizando el mismo proceso de dos pasos para construir hasta tipos de comandos más elegantes, así que muy altamente recomiendo ir a una página atrás. Juro que alcanzará sus objetivos con mayor rapidez si se comienza con una base sólida!

##Fase Uno - El mínimo necesario

En este punto, en esta guía, tiene que estar muy cómoda golpeando abajo de la estructura básica de una aplicación Elm. Adivinar el modelo, rellene algunos mensajes, etc., etc.

```
-- MODEL

type alias Model =
  { topic : String
  , gifUrl : String
  }

init : (Model, Cmd Msg)
init =
  (Model "cats" "waiting.gif", Cmd.none)


-- UPDATE

type Msg = MorePlease

update : Msg -> Model -> (Model, Cmd Msg)
update msg model =
  case msg of
    MorePlease ->
      (model, Cmd.none)


-- VIEW

view : Model -> Html Msg
view model =
  div []
    [ h2 [] [text model.topic]
    , img [src model.gifUrl] []
    , button [ onClick MorePlease ] [ text "More Please!" ]
    ]
```

Para el modelo, decidí realizar un seguimiento de un tema así que sé qué tipo de gifs para ir a buscar. No quiero a codificar `"cats"`, y tal vez más adelante nos va a querer dejar que el usuario decida el tema también. También he localizado el `gifUrl` que es un URL que apunta en algún gif azar.

Al igual que en el ejemplo de la aleatoriedad, acabo de hacer ficticias funciones de inicialización y actualización. Ninguno de ellos realmente puede producir ningún comando por ahora. El punto es sólo conseguir algo en la pantalla!

## Fase dos - Adición de las cosas interesantes

Muy bien, lo que  falta en este momento es la petición HTTP. Creo que es más fácil de iniciar este proceso mediante la adición de nuevos tipos de mensajes. Ahora recuerde, cuando se le da una orden, usted tiene que esperar a que suceda. Así que cuando mandamos Elm hacer una petición HTTP, que finalmente se va a decir "bueno, aquí es lo que quería", o que va a decir "¡Uy, algo salió mal con la petición HTTP". Necesitamos que esto se refleja en nuestros mensajes:

```
type Msg
  = MorePlease
  | FetchSucceed String
  | FetchFail Http.Error
```

Todavía tenemos `MorePlease` de antes, pero para los resultados HTTP, añadimos `FetchSucceed` que contiene la nueva dirección URL gif y FetchFail indica que hubo algún problema HTTP (servidor está caído, mala dirección URL, etc.)

Eso es suficiente para empezar a rellenar `update` :
```
update : Msg -> Model -> (Model, Cmd Msg)
update msg model =
  case msg of
    MorePlease ->
      (model, getRandomGif model.topic)

    FetchSucceed newUrl ->
      (Model model.topic newUrl, Cmd.none)

    FetchFail _ ->
      (model, Cmd.none)
```

Por lo que añade ramas para nuestros nuevos mensajes. En el caso de  `FetchSucceed ` actualizamos el campo `gifUrl` para  tener la nueva URL. En el caso de `FetchFail` que prácticamente ignoramos, devolviendo el mismo modelo y no haciendo nada.

También ha cambiado la rama `MorePlease` un poco. Necesitamos un comando HTTP, así que llamamos a la función `getRandomGif`. El truco es que hice esa función para arriba. No existe todavía. Ese es el siguiente paso!

Definición de `getRandomGif` podría ser algo como esto:
```
getRandomGif : String -> Cmd Msg
getRandomGif topic =
  let
    url =
      "https://api.giphy.com/v1/gifs/random?api_key=dc6zaTOxFJmzC&tag=" ++ topic
  in
    Task.perform FetchFail FetchSucceed (Http.get decodeGifUrl url)

decodeGifUrl : Json.Decoder String
decodeGifUrl =
  Json.at ["data", "image_url"] Json.string
```

De acuerdo, entonces la función `getRandomGif` no es excepcionalmente loco. Primero vamos a definir la url que necesitamos para para obtener gifs azar. A continuación tenemos esta función `Http.get` que se va a conseguir un poco de JSON desde la url que le damos. La parte interesante está el argumento `decodeGifUrl` que describe cómo convertir JSON en los valores de Elm. En nuestro caso, estamos diciendo "tratar de obtener el valor en `json.data.image_url` y debe ser una cadena."

La parte `Task.perform` está clarificando qué hacer con el resultado de esta GET:

 * El primer argumento `FetchFail` es para cuando la operación de obtener no. Si el servidor no funciona o la dirección URL es un 404, marcamos el error resultante con `FetchFail` y se alimentan en nuestra función de actualización.
 * El segundo argumento es `FetchSucceed` para cuando el GET tiene éxito. Cuando tengamos alguna URL espalda como `http://example.com/json`, lo convertimos en FetchSucceed `"http://example.com/json"` para que pueda ser alimentado en nuestra función de actualización.



Y ahora cuando se hace clic en el botón "Más", en realidad se va y se va a buscar un gif azar!
