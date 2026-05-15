Programación en directo (Live Coding)

# Programación en directo (Live Coding)

En el tutorial de Sonic Pi de este mes, vamos a ver cómo puedes empezar a usar Sonic Pi como un instrumento de verdad. Por eso necesitamos empezar a pensar en programar de una forma totalmente distinta. L@s livecoders piensan en código de forma parecida l@s violinistas cuando piensan en su arco. De hecho, igual que un/a violinista puede usar varias técnicas de arco para crear distintos sonidos (movimientos lentos, golpes rápidos...), vamos a explorar las cinco técnicas básicas de livecoding que Sonic Pi nos proporciona. Al final de este artículo, serás capaz de empezar a practicar tus propias actuaciones de livecoding.

## 1. Memoriza los Atajos de Teclado

El primer paso para hacer livecoding con Sonic Pi es empezar a usar los atajos de teclado. Por ejemplo, en vez de perder tiempo alcanzando el ratón, poniéndolo encima del botón de Ejecutar y haciendo click, puedes pulsar `alt` y `r` al mismo tiempo, que es mucho más rápido y mantiene tus dedos en el teclado, preparados para el siguiente cambio en el código. Puedes encontrar los atajos de teclado para los botones principales de la barra superior colocando el ratón encima de ellos. Mira la sección 10.2 del tutorial incluido en Sonic Pi para una lista completa de atajos de teclado.

Cuando estás tocando en un concierto, algo divertido que puedes probar es añadirle un poco de estilo al movimiento de tu brazo cuando hagas los atajos de teclado. Por ejemplo, normalmente es bueno comunicar al público cuándo vas a hacer un cambio - así que embellece tu movimiento cuando escribas `alt-r`, igual que haría un/a guitarrista al tocar un power chord.

## 2. Crea tus Sonidos por Capas a Mano

Ahora que puedes lanzar código directamente con el teclado, puedes aplicar esta habilidad de forma inmediata a nuestra segunda técnica, que consiste en crear tus sonidos por capas de forma manual. En vez de 'componer' usando un montón de llamadas a `play` y `sample` separadas por llamads a `sleep`, vamos a tener una sola llamada a `play`, que vamos a disparar de forma manual usando `alt-r`. Vamos a probarlo. Escribe el siguiente código en un búfer vacío:

```
synth :prophet, note: :e1, release: 8, cutoff: 100
```

Ahora, dale a `Ejecutar`, y mientras se reproduce el sonido, modifica el código para quitar las cuatro notas, cambiándolo al siguiente:


```
synth :prophet, note: :e1, release: 8, cutoff: 100
```

Ahora, dale a `Ejecutar` de nuevo, para escuchar ambos sonidos sonando al mismo tiempo. Esto pasa porque el botón de `Ejecutar` de Sonic Pi no espera a que el código anterior acabe, sino que empieza a ejecutar el código a la vez. Esto significa que puedes colocar fácilmente un montón de capas de sonidos de forma manual con modificaciones más o menos pequeñas entre cada ejecución. Por ejemplo, prueba a cambiar las opciones `note:` y `cutoff:` y vuelve a ejecutar el código.


También puedes probar esta técnica con ejemplos más largos y abstractos. Por ejemplo:

```
sample :ambi_lunar_land
```

Prueba a empezar el sample apagado, y divide a la mitad la opción `rate:` entre cada ejecución, de `1` a `0.5`, luego a `0.25`, luego a `0.125`, e incluso, prueba valores negativos como `-0.5`. Deja todos los sonidos juntos, uno encima de otro, a ver hasta dónde puedes llegar. Finalmente, prueba a añadir algún efecto.

Cuando tocas en un concierto usando líneas de código sencillas (como hemos visto anteriormente), le permites al público nuevo a Sonic Pi seguir lo que estás haciendo, y relacionar el código que ven con los sonidos que escuchan.


## Bucles en Vivo

Cuando trabajas con música más rítmica, suele ser complicado cambiar todo manualmente y mantener el tempo. Lo que se suele hacer en estos casos, es usar un `live_loop`. Esto le proporciona repetición a tu código, y te permite editar el código para la siguiente vuelta del bucle. Además, se ejecutan a la vez que otros `live_loops`, lo que significa que puedes hacer que suenen juntos entre sí y con código disparado manualmente. Mira la sección 9.2 del tutorial incluido en Sonic Pi para más información sobre cómo trabajar con live loops.

Cuando toques, recuerda usar la opción `sync:` de los `live_loop`'s, que te permite recuperarte de errores de ejecución accidentales cuando éstos paran la ejecución de tu live loop. Si tienes la opción `sync:` apuntando a algún otro `live_loop` válido, entonces puedes arreglar el error rápidamente y ejecutar de nuevo el código para empezar otra vez sin perder un solo beat.

## 4. Usa el Mezclador Maestro (Master Mixer)

Uno de los secretos mejor guardados de Sonic Pi es que éste tiene un mezclador maestro (Master Mixer) por el que fluye todo el sonido. Este mezclador incorpora un filtro de paso bajo y un filtro de paso alto, así que puedes modificar fácilmente el sonido a nivel global. Puedes acceder a funcionalidad del mezclador maestro con la función `set_mixer_control`. Por ejemplo, mientras tienes código en ejecución y sonando, escribe esto en un búfer distinto y dale a `Ejecutar`:

` set_mixer_control! lpf: 50 `

Cuando hayas ejecutado tu código, todos los sonidos existentes y nuevos tendrán un filtro de paso bajo aplicado, y por tanto sonarán más apagados. Fíjate en que esto significa que los nuevos valores del mezclador se quedan hasta que puedan ser cambiados de nuevo. Sin embargo, si quieres, siempre puedes resetear el mezclador a su estado original con `reset_mixer`. Algunas de las opciones permitidas actualmente son: `pre_amp:`, `lpf:`, `hpf:` y `amp:`. Para la lista completa, mira los documentos incluidos de Sonic Pi; busca `set_mixer_control`.

Usa las opciones `*_slide` del mezclador para deslizar el valor de una o varias opciones a lo largo del tiempo. Por ejemplo, si quieres deslizar el filtro de paso bajo del mezclador bajando desde el valor actual (30), usa esto:

```
set_mixer_control! lpf_slide: 16, lpf: 30
```

Puedes volver a deslizar el valor a un valor alto rápidamente con:

```
set_mixer_control! lpf_slide: 1, lpf: 130
```

Cuando tocas, suele ser útil el tener un búfer libre para que puedas trabajar con el mezclador de esta forma.

## FX en la práctica

La técnica más importante para hacer live coding es práctica. El atributo más común entre músic@s profesionales de todos los tipos es que practican tocando con sus instrumentos - normalmente durante varias horas al día. La práctica es tan importante para un/a live coder como para un/a guitarrista. La práctica le permite a tus dedos memorizar ciertos patrones y ediciones comunes; de esa forma puedes escribir y trabajar con ellos con mayor fluidez. La práctica también te da oportunidades para explorar nuevos sonidos y nuevas construcciones de código.

Con el tiempo, te darás cuenta de que cuanto más practiques, más podrás relajarte en directo. La práctica también te da experiencia. Esto te ayudará a entender mejor qué cambios puedes hacer en tu código que suenen interesantes y que encajen con el resto de sonidos de tu canción.

## Juntándolo todo

Este mes, en vez de darte un ejemplo final que combine todo lo que hemos visto, vamos a empezar con un reto. Mira si puedes dedicar una semana a practicar, practicando una de las ideas que hemos visto cada día. Por ejemplo, un día puedes practicar triggers manuales, otro día puedes trabajar `live_loop`s básicos, y al día siguiente puedes juguetear con el mezclador maestro. Repite esta dinámica otros tres días. No te preocupes si todo se te hace lento y torpe al principio; tú sigue practicando, y antes de que te des cuenta, estarás haciendo live coding para un público real.
