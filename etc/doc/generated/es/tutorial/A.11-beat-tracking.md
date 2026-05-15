A.11. Tic tac

# Siguiendo el Beat

El último mes hicimos una inmersión técnica en el sistema de aleatoriedad de Sonic Pi. Exploramos cómo podemos usarlo para añadir nuevos niveles de control dinámico en nuestro código, de forma determinista. Este mes vamos a continuar con nuestra inmersión técnica, para enseñarte el sistema de tick único en Sonic Pi. Al final del artículo, podrás recorrer tus ritmos y riffs con total facilidad, avanzando en tu camino para ser un/a DJ de live coding.

## Contando Beats

Normalmente, cuando hacemos música, queremos hacer distintas cosas dependiendo de cómo sea nuestro beat. Sonic Pi tiene un sistema especial para contar beats llamado `tick` que te proporciona un control muy preciso sobre cuándo ocurre un beat, e incluso permite usar distintos beats con sus propios tempos.

Vamos a tocar un poco. Para hacer avanzar el beat sólo tenemos que llamar a `tick`. Abre un búfer nuevo, escribe el siguiente código y dale a Ejecutar:

```
puts tick #=> 0
```

Esto devuelve el beat actual: `0`. Fíjate en que aunque presiones el botón de Ejecutar varias veces, siempre va a devolver `0`. Esto pasa porque cada ejecución empieza un beat nuevo, empezando a contar desde 0. Sin embargo, mientras la ejecución siga activa, podemos hacer avanzar el beat tantas veces como queramos:

```
puts tick #=> 0
puts tick #=> 1
puts tick #=> 2
```

Cuando veas el símbolo `#=>` al final de una línea de código, significa que esa línea va a escribir el texto que queda al lado derecho. Por ejemplo, `puts foo #=> 0` significa que el código `puts foo` imprime`0` en el log en ese momento de la ejecución.

## Comprobando el Beat

Hemos visto que `tick` hace dos cosas. Tick incrementa (añade uno) y devuelve el beat actual. A veces sólo queremos ver el beat actual sin tener que incrementarlo, lo que podemos hacer con `look`:

```
puts tick #=> 0
puts tick #=> 1
puts look #=> 1
puts look #=> 1
```

En este código incrementamos el beat dos veces haciendo tick y entonces llamamos `look` dos veces. Veremos los siguientes valores en el log: `0`, `1`, `1`, `1`. Los dos primeros `tick`s han devuelto `0`, después `1` como esperábamos, finalmente los dos `look`s simplemente devolvieron el ultimo valor del beat dos veces, lo cual ha sido `1`.


## Anillos

Así que ahora podemos avanzar el beat con `tick`, y comprobar el beat con `look`. ¿Y ahora qué? Necesitamos algo sobre lo que hacer tick. Sonic Pi utiliza anillos para representar riffs, melodías y ritmos, y el sistema de ticking ha sido diseñado específicamente para trabajar con anillos. De hecho, los anillos tienen su propia versión de `tick` (`.tick`, con punto) que hace dos cosas. Primero, actúa como un tick normal e incrementa el beat. Segundo, obtiene el valor del anillo usando el beat como índice (posición dentro del anillo). Veamos:

```
puts (ring :a, :b, :c).tick #=> :a
```

`.tick` es una versión especial (con un punto delante) de `tick`, que devuelve el primer valor del anillo `:a`. Podemos coger cada uno de los valores del anillo llamando `.tick` varias veces:

```
puts (ring :a, :b, :c).tick #=> :a
puts (ring :a, :b, :c).tick #=> :b
puts (ring :a, :b, :c).tick #=> :c
puts (ring :a, :b, :c).tick #=> :a
puts look                   #=> 3
```

Mira el log y allí verás `:a`, `:b`, `:c` y de nuevo `:a`. Fíjate que `look` devuelve `3`. Las llamadas a `.tick` funcionan como si fueran llamadas normales a `tick`- incrementan el beat local.


## Un Arpegiador de Live Loop

El poder read viene cuando mezclas `tick` con rings y `live_loop`s. Cuando están combinados tenemos todas las herramientas necesarias para construir y también entender un simple arpegiador. Solo necesitamos cuatro cosas:

1. Un ring que contiene las notas que queremos repetir.
2. Una forma de incrementar y obtener el beat.
3. La capacidad de tocar una nota en función del beat actual.
4. Una bucle para mantener al arpegiador repitiéndose.

Estos conceptos pueden ser comprobados en el siguiente código:

```
notes = (ring 57, 62, 55, 59, 64)
live_loop :arp do
  use_synth :dpulse
  play notes.tick, release: 0.2
  sleep 0.125
end
```

Veamos cada una de estas líneas. Primero definimos nuestro ring de notas que reproduciremos continuamente. Entonces creamos un `live_loop` llamado `:arp` que hace loop a nuestro alrededor. Cada vuelta en el `live_loop` ponemos nuestro synth en `:dpulse` y entonces tocamos la siguiente nota usando`.tick`. Recuerda que esto incrementará nuestro contador de beats y usará el último valor del beat como el índice para nuestro ring de notas. Finalmente, esperamos un octavo de beat antes de volver a empezar el loop.

## Múltiples Ritmos Simultáneos

Una cosa que hay que saber es que los `tick`s son locales al `live_loop`. Esto quiere decir que cada `live_loop` tiene su propio contador de beats independiente. Esto es mucho más potente que tener un metrónomo y ritmo global . Veamos como funciona esto:

```
notes = (ring 57, 62, 55, 59, 64)
with_fx :reverb do
  live_loop :arp do
    use_synth :dpulse
    play notes.tick + 12, release: 0.1
    sleep 0.125
  end
end
live_loop :arp2 do
  use_synth :dsaw
  play notes.tick - 12, release: 0.2
  sleep 0.75
end
```

## Ritmos que chocan

Una causa de confusiones habituales con el sistema de tick de Sonic Pi es cuando la gente quiere hacer tick en varios rings dentro del mismo `live_loop`:

```
use_bpm 300
use_synth :blade
live_loop :foo do
  play (ring :e1, :e2, :e3).tick
  play (scale :e3, :minor_pentatonic).tick
  sleep 1
end
```

Aunque cada `live_loop` tiene su propio contador de beat (independiente del resto), estamos llamando a `.tick` dos veces dentro del mismo `live_loop`. Esto significa que el beat se incrementa dos veces en cada vuelta. Esto puede generar polirritmos interesantes, pero normalmente no es lo que queremos. Hay dos soluciones para este problema. Una opción es llamar a `tick` manualmente al principio del `live_loop`, y usar `.look` para mirar el beat actual en cada `live_loop`. Otra opción es pasarle un nombre único a cada llamada a `.tick`, como `.tick(:foo)`. De esta forma, Sonic Pi crea un contador de beat separado para cada tick que has nombrado, y realiza su seguimiento. De esta forma, ¡puedes trabajar con tantos beats como necesites! Mira la sección 9.4 (ticks con nombre) del manual incluido en Sonic Pi para más información.

## Juntándolo todo

Juntemos todo este conocimiento de los `tick`s, `ring`s y `live_loop`s en un ejemplo final. Como de costumbre, no consideres esto como una pieza acabada. Empieza cambiando cosas y jugando con ellos a ver en que puedes transformarlo. Nos vemos...

```
use_bpm 240
notes = (scale :e3, :minor_pentatonic).shuffle
live_loop :foo do
  use_synth :blade
  with_fx :reverb, reps: 8, room: 1 do
    tick
    co = (line 70, 130, steps: 32).tick(:cutoff)
    play (octs :e3, 3).look, cutoff: co, amp: 2
    play notes.look, amp: 4
    sleep 1
  end
end
live_loop :bar do
  tick
  sample :bd_ada if (spread 1, 4).look
  use_synth :tb303
  co = (line 70, 130, steps: 16).look
  r = (line 0.1, 0.5, steps: 64).mirror.look
  play notes.look, release: r, cutoff: co
  sleep 0.5
end
```
