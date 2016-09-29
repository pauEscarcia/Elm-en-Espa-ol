
# La arquitectura de Elm + Efectos

La última sección muestra cómo manejar todo tipo de entrada del usuario. Se puede pensar en esos programas como este:
![Programa principal](https://github.com/pauEscarcia/Posts_Elm_Spanish/blob/master/Imagenes/beginnerProgram.svg)

Desde nuestra perspectiva, acabamos de recibir mensajes y producir nuevos `Html` para conseguir prestados en la pantalla. El "tiempo de ejecución Elm" está sentado detrás de las escenas. Cuando se pone en `Html` que se da cuenta de la forma en que lo convierta en pantalla es muy rápido. Cuando un usuario hace clic en algo, se da cuenta de cómo tubería que en nuestro programa como un `Msj`. Por lo que el tiempo de ejecución de Elm es el encargado de hacer las cosas. Acabamos transformamos datos.

Esta sección se basa en ese patrón, que le da la capacidad de hacer peticiones HTTP o suscribirse a los mensajes de sockets web. Piensa en esto, de esta manera:
![Programa](https://github.com/pauEscarcia/Posts_Elm_Spanish/blob/master/Imagenes/program.svg)

En lugar de simplemente producir HTML, ahora vamos a estar produciendo comandos y suscripciones:

*  **Comandos** - Un `Cmd` le permite hacer cosas: generar un número aleatorio, envía una solicitud HTTP, etc.
*  **Suscripciones** - Una `Sub` le permite registrar en lo que usted está interesado en algo: háblame de cambios de ubicación, para escuchar mensajes de conexión web, etc.

Si entrecierra los ojos, comandos y las suscripciones son bastante similares a los valores HTML. Con HTML, nosotros nunca tocamos DOM con la mano. En lugar representamos el código HTML deseado en forma de datos y dejar que el tiempo de ejecución de Elm hacer algunas cosas inteligentes para hacer rinda muy rápido. En lo mismo con los comandos y las suscripciones. Creamos datos que describe lo que queremos hacer, y el tiempo de ejecución de Elm hace el trabajo sucio.

No se preocupe si parece un poco confuso, por ahora, los ejemplos ayudarán! Así que primero vamos a ver cómo encajar estos conceptos en el código que hemos visto antes.

## Extendiendo el esqueleto de la arquitectura 

Hasta ahora nuestro esqueleto de la arquitectura se ha centrado en la creación de `Model` y tipos de funciones de `update` y `view`. Para manejar los comandos y las suscripciones, necesitamos extender la arquitectura básica esqueleto un poco:

```
-- MODEL

type alias Model =
  { ...
  }


-- UPDATE

type Msg = Submit | ...

update : Msg -> Model -> (Model, Cmd Msg)
update msg model =
  ...


-- VIEW

view : Model -> Html Msg
view model =
  ...


-- SUBSCRIPTIONS

subscriptions : Model -> Sub Msg
subscriptions model =
  ...


-- INIT

init : (Model, Cmd Msg)
init =
  ...
```

Las tres primeras secciones son casi exactamente lo mismo, pero hay algunas cosas nuevas en general:

La función de `update` ahora vuelve algo más que un nuevo modelo. Devuelve un nuevo modelo y algunos comandos que desea ejecutar. Estos comandos son todos van a producir valores `Msg` que se alimentan de nuevo en nuestra función de `update`.
Hay una función de `subscriptions`. Esta función le permite declarar las fuentes de eventos que necesita para suscribirse a dado el modelo actual. Al igual que con el `Html Msg`y `Cmd Msg`, estas suscripciones producirán valores `Msg` que se alimentan de nuevo en nuestra función de `update`.
Hasta ahora `init` simplemente ha sido el modelo inicial. Ahora se produce un modelo y algunos comandos, al igual que la nueva `update`. Esto nos permite ofrecer un valor de partida y comenzar cualquier solicitud HTTP o lo que sea que sea necesario para la inicialización.

Ahora bien,esto no tiene mucho sentido todavía! En realidad sólo sucede cuando se empiezan a ver en acción, así que vamos a realizar 
los ejemplos!

> 
>  Aparte: Un detalle crucial aquí es que los comandos y las suscripciones son los datos. Cuando se crea un comando, que en
> realidad no lo hace. Lo mismo pasa con los comandos en la vida real.
> Vamos a intentarlo. Comer una sandía entera de un solo bocado! ¿Lo has
> hecho? ¡No! Usted mantiene la lectura antes de que incluso pensó en
> comprar una pequeña sandía.
> 
>    El punto es, los comandos y las suscripciones son los datos. Se la da a Elm para realmente ejecutarlos, dándole a Elm la chance de
> ingresar toda esta información. Al final, los efectos-como-los datos
> significa Elm puede:
> 
 *  Tener un depurador de viajes en el tiempo de uso general.
 *  Mantenga la "misma entrada, misma salida" garaniza todas las funciones de Elm.
 * Evitar las fases de instalación / desmontaje cuando se prueba lógica de actualización.
 *  Caché y por lotes, lo que minimiza los efectos conexiones HTTP u otros recursos.
> 
>   Así que, sin ir demasiado loco en detalles, prácticamente todas las garantías y herramientas que tiene en Elm bonitas provienen de la
> elección para el tratamiento de efectos como los datos! Creo que esto
> tendrá más sentido a medida que profundicemos en Elm.

