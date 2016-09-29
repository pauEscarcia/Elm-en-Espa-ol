#Botones en Elm

Nuestro primer ejemplo es un simple contador que puede ser ascendente o descendente. Me parece que puede ser útil ver el programa completo de una vez, por lo que aquí está! 
```
import Html exposing (Html, button, div, text)
import Html.App as App
import Html.Events exposing (onClick)


main =
  App.beginnerProgram { model = model, view = view, update = update }


-- MODEL

type alias Model = Int

model : Model
model =
  0


-- UPDATE

type Msg = Increment | Decrement

update : Msg -> Model -> Model
update msg model =
  case msg of
    Increment ->
      model + 1

    Decrement ->
      model - 1


-- VIEW

view : Model -> Html Msg
view model =
  div []
    [ button [ onClick Decrement ] [ text "-" ]
    , div [] [ text (toString model) ]
    , button [ onClick Increment ] [ text "+" ]
    ]
```
¡Eso es todo!

> Nota: En esta sección tiene las declaraciones `type` y `type alias`. No es necesario comprender profundamente  las cosas ahora.

Al escribir este programa desde cero, siempre comienzo mediante la adopción de una pista sobre el modelo. Vamos a empezar de arriba a abajo. Así que vamos a empezar con eso!

```
type alias Model = Int
```

Ahora que tenemos un modelo, tenemos que definir cómo cambia con el tiempo. Siempre comienzo mi sección de la actualización `UPDATE` mediante la definición de un conjunto de mensajes que vamos a obtener de la interfaz de usuario:
```
type Msg = Increment | Decrement


```

Definitivamente sé el usuario será capaz de incrementar y decrementar del contador. El tipo de error se describen estas capacidades como de datos. ¡Importante! A partir de ahí, la función de actualización simplemente describe lo que hay que hacer cuando recibe uno de estos mensajes.

```
update : Msg -> Model -> Model
update msg model =
  case msg of
    Increment ->
      model + 1

    Decrement ->
      model - 1
```

Si recibe un mensaje de `Increment`, se incrementará el modelo. Si obtiene un mensaje de `Decrement`, el decremento del modelo. Algo bastante sencillo.

Está bien, pero ¿cómo podemos hacer un poco de HTML y mostrarlo en la pantalla? Elm tiene una biblioteca se llama èlm-lang / html` que le da acceso completo a HTML5 como funciones normales de ELM:

```
view : Model -> Html Msg
view model =
  div []
    [ button [ onClick Decrement ] [ text "-" ]
    , div [] [ text (toString model) ]
    , button [ onClick Increment ] [ text "+" ]
    ]


```

Una cosa a notar es que nuestra función de `view` está produciendo un valor `Html Msg`. Esto significa que se trata de un trozo de HTML que puede producir `Msg` de valores . Y cuando nos fijamos en la definición, se ven los atributos `onClick` se establecen para dar a conocer los valores de `Increment` y `Decrement`. Estos se alimentan  directamente en nuestra función de `update`, conduciendo toda nuestra aplicación hacia adelante.

Otra cosa a notar es que  `div` y `button` son funciones normales de Elm. Estas funciones toman (1) una lista de atributos y (2) una lista de nodos secundarios. Es sólo HTML con sintaxis ligeramente diferente. En lugar de tener `<` y `>` en todas partes, tenemos `[` y `]`. Hemos encontrado que la gente que puede leer HTML tienen un tiempo bastante fácil de aprender a leer esta variación. De acuerdo, pero ¿por qué no tiene que ser exactamente igual que el HTML? ** Ya que estamos usando las funciones normales de Elm, tenemos todo el poder del lenguaje de programación Elm para ayudarnos a construir nuestros puntos de vista! ** Podemos refactorizar código repetitivo hacia funciones. Podemos poner ayudantes en módulos e importarlos al igual que cualquier otro tipo de código. Podemos utilizar las mismas infraestructuras de prueba y las bibliotecas como cualquier otro código Elm. Todo lo que es bueno acerca de la programación en Elm es 100% disponible para ayudarle con su punto de vista. No hay necesidad de un lenguaje de plantillas hackeado!

También hay algo un poco más profundo pasando aquí. ** El código de la vista es totalmente declarativo ** . Tomamos  un `Model` y producimos algo de `Html`.  Eso es. No hay necesidad de mutar el DOM de forma manual, Elm se encarga de que detrás de escenas. Esto da Elm mucha más libertad para realizar optimizaciones inteligentes y termina haciendo una representación más rápida en general. Así se escribe menos código y el código se ejecuta más rápido. El mejor tipo de abstracción!

Este patrón es la esencia de la arquitectura Elm. Todos los ejemplos que vamos a ver  a partir de ahora van a tener una ligera variación de este patrón básico: `Model`, `update` y  `view`.

> Ejercicio: Una cosa cool  sobre la arquitectura Elm es que es muy fácil de ampliar a medida que cambian las necesidades de productos. Digamos que su jefe de producto ha llegado con esta increíble característica de "reset". Un nuevo botón que reiniciar el contador a cero.

> Para agregar la característica de que devuelva  con el tipo de error `Msg` y añadir otra posibilidad: `Reset`. A continuación, pasa a la función de `update`y describir lo que sucede cuando usted consigue ese mensaje. Finalmente se agrega un botón su view.

>¡Veremos  si puede poner en práctica la característica de  "reset"!
