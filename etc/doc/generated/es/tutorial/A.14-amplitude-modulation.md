Amplitud

# Amplitud

Este mes vamos a sumergirnos en uno de los efectos más potentes y flexibles de Sonic Pi: el `:slicer`. Al final de este artículo, habrás aprendido cómo manipular el volumen de nuevas formas muy interesantes. Esto te permitirá crear nuevos ritmos y estructuras rítmicas, ampliando tus posibilidades sonoras.

## Rebana ese Amp

Entonces, ¿qué hace realmente el FX `:slicer`? Una forma de pensar en ello es como si alguien jugara con el control de volumen de tu televisor o equipo de música. Echemos un vistazo, pero primero, escucha el profundo gruñido del siguiente código que activa el sintetizador `:prophet`:

```
synth :prophet, note: :e1, release: 8, cutoff: 70
synth :prophet, note: :e1 + 4, release: 8, cutoff: 80
```

Ahora, vamos a pasarlo por el efecto `:slicer`:

```

with_fx :slicer do
  synth :prophet, note: :e1, release: 8, cutoff: 70
  synth :prophet, note: :e1 + 4, release: 8, cutoff: 80
end
```

Escucha cómo el slicer le pone y le quita volumen al audio, con un beat regular. Fíjate también en cómo el `:slicer` afecta a todo el audio generado entre los bloques `do`/`end`. Puedes controlar la velocidad a la que se enciende y apaga el audio con la opción `phase:`, que es una abreviatura de duración de fase (phase duration en inglés). Su valor por defecto es `0.25`, que significa 4 veces por segundo usando el BPM por defecto de 60. Hagámoslo más rápido:

```
with_fx :slicer, phase: 0.125 do
  synth :prophet, note: :e1, release: 8, cutoff: 70
  synth :prophet, note: :e1 + 4, release: 8, cutoff: 80
end
```

Ahora, juega tú mismo con diferentes duraciones de `phase:`. Prueba con valores más largos y más cortos. Mira lo que ocurre cuando eliges un valor realmente corto. Prueba también con diferentes sintetizadores como `:beep` o `:dsaw` y diferentes notas. Echa un vistazo al siguiente diagrama para ver cómo los diferentes valores de `phase:` cambian el número de cambios de amplitud por compás.

Duración

La duración de fase (phase duration) es la longitud de tiempo para un ciclo de encendido/apagado del volumen. Por tanto, valores más bajos harán al efecto cambiar entre encendido y apagado mucho más rápido que valores más altos. Puedes empezar jugando con valores como `0.125`, `0.25`, `0.5` y `1`.


## Ondas de Control

Por defecto, el efecto `:slicer` utiliza una onda cuadrada para manipular la amplitud a lo largo del tiempo. Por eso escuchamos la amplitud encendida durante un periodo de tiempo, luego inmediatamente apagada por otro periodo, y encendida de nuevo. Podemos ver que la onda cuadrada sólo es una de las 4 ondas de control distintas que permite usar `:slicer`. Las otras son la onda de sierra, la onda triangular la onda de (co)seno. Échale un vistazo al diagrama de arriba para ver qué forma tienen estas ondas. También podemos escuchar cómo suenan. Por ejemplo, el siguiente código utiliza el (co)seno como onda de control. Escucha cómo el sonido no se enciende y apaga de forma abrupta, sino que aparece y desaparece gradualmente:

```
with_fx :slicer, phase: 0.5, wave: 3 do
  synth :dsaw, note: :e3, release: 8, cutoff: 120
  synth :dsaw, note: :e2, release: 8, cutoff: 100
end
```

Juega con las siguientes formas de onda cambiando la opción `wave:` a `0` para onda de sierra, `1` para onda cuadrada, `2` para onda triangular, y `3` para onda de seno. Prueba también cómo suenan ondas distintas con valores de `phase:` distintos.

Cada una de estas ondas se pueden invertir con la opción `invert_wave:`, que les da la vuelta en torno al eje y. Por ejemplo, en una misma fase, una onda cuadrada suele empezar alto y va bajando gradualmente antes de volver a subir. Con `invert_wave:1`, la onda empieza bajo y va subiendo gradualmente antes de volver a bajar. Además, la onda de control puede empezar en puntos distintos con la opción `phase_offset:`, que debería tener un valor entre `0` y `1`. Jugando con las opciones `phase:`, `wave:`, `invert_wave:` y `phase_offset` puedes cambiar drásticamente cómo se modifica la amplitud a lo largo del tiempo.

Duración


## Configurando tus niveles

Por defecto, `:slicer` cambia entre los valores de amplitud `1` (totalmente alto) y `0` (silenciado). Esto se puede cambiar con las opciones `amp_min:` y `amp_max`. Puedes usarlas con la onda de seno, creando un efecto de tremolo muy sencillo:

```
with_fx :slicer, amp_min: 0.25, amp_max: 0.75, wave: 3, phase: 0.25 do
  synth :saw, release: 8
end
```

Es como si cogieses la rueda de volumen en tu altavoz y lo movieses arriba y abajo ligeramente, con lo que el sonido 'tiembla'.


## Probabilidades

Una de las potentes características de `:slicer` es su capacidad de utilizar la probabilidad para elegir si se activa o desactiva el slicer. Antes de que el `:slicer` FX comience una nueva fase, lanza un dado y, en función del resultado, utiliza la onda de control seleccionada o mantiene la amplitud desactivada. Vamos a escucharlo:

```
with_fx :slicer, phase: 0.125, probability: 0.6  do
  synth :tb303, note: :e1, cutoff_attack: 8, release: 8
  synth :tb303, note: :e2, cutoff_attack: 4, release: 8
  synth :tb303, note: :e3, cutoff_attack: 2, release: 8
end
```

Escucha el interesante ritmo de pulsos que hemos conseguido. Prueba a cambiar la opción `probability:` (probabilidad) a un valor distinto entre `0` y `1`. Valores más cercanos a `0` generarán más espacio entre cada sonido, porque la probabilidad de que el sonido se dispare es mucho menor.

Otra cosa que hay que notar es que el sistema de probabilidad en el FX es igual que el sistema de aleatoriedad accesible a través de fns como `rand` y `shuffle`. Ambos son completamente deterministas. Esto significa que cada vez que pulses Run escucharás exactamente el mismo ritmo de pulsos para una probabilidad determinada. Si quieres cambiar las cosas, puedes usar la opción `seed:` para seleccionar una semilla diferente. Esto funciona exactamente igual que `use_random_seed` pero sólo afecta a ese FX en particular.

Finalmente, puedes probar a cambiar la posición de 'reposo' de la onda de control cuando los tests de probabilidad fallen de `0` a cualquier otra posición, con la opción `prob_pos:`:

```
with_fx :slicer, phase: 0.125, probability: 0.6, prob_pos: 1  do
  synth :tb303, note: :e1, cutoff_attack: 8, release: 8
  synth :tb303, note: :e2, cutoff_attack: 4, release: 8
  synth :tb303, note: :e3, cutoff_attack: 2, release: 8
end
```

## Ritmos que chocan

Algo muy divertido que puedes probar es usar el `:slicer` para cortar un beat de batería:

```
with_fx :slicer, phase: 0.125 do
  sample :loop_mika
end
```

Esto nos permite coger cualquier sample y crear nuevas posibilidades rítmicas, lo cual es súper divertido. Sin embargo, debes tener cuidado y asegurarte de que el tempo del sample coincida con el BPM actual de Sonic Pi; de lo contrario, el slicing sonará fatal. Por ejemplo, prueba a cambiar el `:loop_mika` con el sample `loop_amen` y escucha cómo de mal puede llegar a sonar cuando los tempos no están alineados.

## ¡Cambiandolo al vuelo!

Como ya hemos visto, cambiar el BPM por defecto con `use_bpm` alarga o comprime los tiempos de silencio y duraciones de envolventes de sintetizadores para que el tempo coincida. El efecto `:slicer` también necesita esto, ya que la opción `phase:` se mide en beats, en vez de segundos. Por tanto, podemos arreglar el problema que tiene `loop_amen` arriba, cambiando el BPM para que coincida con el del sample:

```
use_sample_bpm :loop_amen
with_fx :slicer, phase: 0.125 do
  sample :loop_amen
end
```

## Juntándolo todo

Vamos a aplicar todas estas ideas en un ejemplo final que, usando el efecto `:slicer`, crea una combinación muy interesante. Adelante, empieza a cambiarlo y ¡conviértelo en tu propia pieza!

```
live_loop :dark_mist do
  co = (line 70, 130, steps: 8).tick
  with_fx :slicer, probability: 0.7, prob_pos: 1 do
    synth :prophet, note: :e1, release: 8, cutoff: co
  end
  
  with_fx :slicer, phase: [0.125, 0.25].choose do
    sample :guit_em9, rate: 0.5
  end
  sleep 8
end
live_loop :crashing_waves do
  with_fx :slicer, wave: 0, phase: 0.25 do
    sample :loop_mika, rate: 0.5
  end
  sleep 16
end
```




