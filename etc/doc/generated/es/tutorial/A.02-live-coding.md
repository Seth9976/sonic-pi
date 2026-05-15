A.2. Live Coding

# Live Coding (Programación en vivo)

El rayo láser atravesaba las nubes de humo; el subwoofer bombeaba las notas graves muy profundamente, en los cuerpos del público. El ambiente estaba cargado con una embriagadora mezcla de sintetizadores y baile. Sin embargo, algo no llegaba a encajar del todo en esta discoteca. Proyectado en colores brillantes, encima de la cabina de DJ, había un texto moviéndose, bailando, relampagueante. No eran efectos visuales, sino una simple proyección de Sonic Pi ejecutándose en una Raspberri Pi. La persona que estaba en la cabina de DJ no estaba tocando discos; estaba escribiendo, editando y evaluando código. En vivo. Esto es Live Coding.

![Sam Aaron Live Coding](../../../etc/doc/images/tutorial/articles/A.02-live-coding/sam-aaron-live-coding.png)

Esto puede sonar como la disparatada historia de una discoteca futurista, pero escribie música en código como en esta historia es una tendencia cada vez más extendida, y se suele llamar por el nombre de Live Coding (http://toplap.org). Una de las direcciones más recientes que este estilo ha tomado es el Algorave (http://algorave.com) - eventos en los que l@s artistas como yo programamos música para que la gente baile. No obstante, no tienes por qué estar en una discoteca para hacer Live Coding - con Sonic Pi versión 2.6 (y posteriores), puedes hacerlo siempre que tengas a mano tu Raspberry Pi y un par de auriculares o unos altavoces. Una vez llegues al final de este artículo, programarás tus propios beats y sabrás modificarlos en vivo y en directo. A dónde llegues después de esto está en el poder de tu imaginación.

## Bucle en Vivo

La clave para hacer live coding con Sonic Pi está en manejar el `live_loop` (bucle en vivo). Vamos a ver uno:

```
live_loop :beats do
  sample :bd_haus
  sleep 0.5
end
```

Un `live_loop` tiene 4 ingredientes fundamentales. El primero es su nombre. Nuestro `live_loop` de arriba se llama `:beats`. Eres libre de llamar a tu `live_loop` como quieras. Haz una locura. Sé creativ@. Yo suelo usar nombres que comuniquen algo sobre la música que va a sonar, pensando en el público. El segundo ingrediente es la palabra `do` que marca dónde empieza el `live_loop`. El tercer ingrediente es la palabra `end` que marca dónde el `live_loop` acaba, y finalmente está el cuerpo del `live_loop` que describe qué es lo que va a repetir el bucle (ésta es la parte entre el `do` y el `end`). En este caso estamos tocando de forma repetida un bombo de batería y luego esperamos durante medio pulso. Esto produce un pulso grave regular perfecto. Adelante, cópialo en un búfer vacío de Sonic Pi y dale a Ejecutar. ¡Boom, Boom, Boom!.

## Redefiniendo al vuelo

Vale, entonces, ¿qué hace al `live_loop` tan especial? ¡Hasta ahora parecía un `loop` glorificado! Pues bien, la belleza de los `live_loop`s está en que puedes redefinirlos sobre la marcha, al vuelo. Esto significa que mientras que éstos se ejecutan, puedes cambiar lo que hacen. Éste es el secreto de hacer live coding con Sonic Pi. Vamos a tocar un poco:

```
live_loop :choral_drone do
  sample :ambi_choir, rate: 0.4
  sleep 1
end
```

Ahora pulsa el botón de Ejecutar o presiona las teclas `alt-r`. Ahora mismo estás escuchando unos coros preciosos. Ahora, mientras suenan, cambia el rate de `0.4` a `0.38`. Dale a Ejecutar de nuevo. ¡Guau! ¿Notas cómo ha cambiado la nota del coro? Cámbialo de nuevo a `0.4` para volver a cómo era antes. Ahora bájalo a `0.2`, luego a `0.19` y después vuelve a `0.4`. ¿Ves cómo el cambiar un sólo parámetro sobre la marcha, te puede dar control de verdad sobre la música? Ahora trastea con el rate tú mism@ - escoge tus propios valores. Prueba con números neativos, números muy pequeños y números muy grandes. ¡Que te diviertas!

## Dormir es importante

Una de las lecciones más importantes sobre el `live_loop` es que necesitan descansar. Mira el siguiente `live_loop`:

```
live_loop :infinite_impossibilities do
  sample :ambi_choir
end
```

Si intentas ejecutar este código, verás de inmediato a Sonic Pi quejándose de que el `live_loop` no durmió. ¡Esto es el sistema de seguridad haciendo efecto! Tómate un segundo para pensar en lo que este código le está pidiendo al ordenador que haga. Efectivamente, le está pidiendo al ordenador que toque un número infinito de samples de coro en cero segundos. De no ser por el sistema de seguridad, el pobre ordenador intentará hacerlo y se colgará y arderá en el proceso. Así que recuerda, tu `live_loop` debe contener un `sleep`.


## Combinando Sonidos

La música está llena de cosas pasando a la vez. La batería a la vez que el bajo a la vez que las voces a la vez que las guitarras... En informática llamamos a esto concurrencia, y Sonic Pi nos ofrece una forma increíblemente fácil de tocar varias cosas a la vez. Sencillamente, ¡usa más de un `live_loop`!

```
live_loop :beats do
  sample :bd_tek
  with_fx :echo, phase: 0.125, mix: 0.4 do
    sample  :drum_cymbal_soft, sustain: 0, release: 0.1
    sleep 0.5
  end
end
live_loop :bass do
  use_synth :tb303
  synth :tb303, note: :e1, release: 4, cutoff: 120, cutoff_attack: 1
  sleep 4
end
```

Aquí tenemos dos `live_loop`s, uno dando la vuelta rápido marcando el pulso, y otro dando la vuelta lentamente y tocando un sonido de bajo muy loco.

Una de las cosas interesantes que tiene el usar varios `live_loop`s es que cada uno gestiona su propio tiempo. Esto significa que es muy fácil crear estructuras polirrítimicas interesantes, e incluso tocar con fases al estilo de Steve Reich. Échale un ojo a esto:

```
# Steve Reich's Piano Phase
notes = (ring :E4, :Fs4, :B4, :Cs5, :D5, :Fs4, :E4, :Cs5, :B4, :Fs4, :D5, :Cs5)
live_loop :slow do
  play notes.tick, release: 0.1
  sleep 0.3
end
live_loop :faster do
  play notes.tick, release: 0.1
  sleep 0.295
end
```

## Juntándolo todo

En cada uno de estos tutoriales, vamos a acabar con un ejemplo final en forma de una pieza de música, que coge un poco de todas las ideas que hemos ido introduciendo. Lee este código y mira si puedes imaginarte lo que está haciendo. Después, cópialo en un búfer vacío en Sonic Pi, dale a Ejecutar y escucha cómo suena. Finalmente, cambia uno de los números o comenta y descomenta cosas. Mira si puedes utilizar esto como un punto de partida para una nueva actuación, y sobre todo, ¡para pasártelo bien! Hasta la próxima...

```
with_fx :reverb, room: 1 do
  live_loop :time do
    synth :prophet, release: 8, note: :e1, cutoff: 90, amp: 3
    sleep 8
  end
end
live_loop :machine do
  sample :loop_garzul, rate: 0.5, finish: 0.25
  sample :loop_industrial, beat_stretch: 4, amp: 1
  sleep 4
end
live_loop :kik do
  sample :bd_haus, amp: 2
  sleep 0.5
end
with_fx :echo do
  live_loop :vortex do
    # use_random_seed 800
    notes = (scale :e3, :minor_pentatonic, num_octaves: 3)
    16.times do
      play notes.choose, release: 0.1, amp: 1.5
      sleep 0.125
    end
  end
end
```
