A.1 Consejos para Sonic Pi

# Los Cinco Mejores Consejos

## 1. No hay errores

La lección más importante que debes aprender de Sonic Pi es que realmente no hay errores. La mejor forma de aprender es sencillamente probar, probar y probar. Prueba un montón de cosas, deja de preocuparte de si tu código suena bien o no y empieza a experimentar con tantos sintetizadores, notas, FX y opciones como puedas. Quita las cosas que no te gusten y deja las cosas que te gusten, sencilamente. Cuantos más 'errores' te permitas hacer, más rápido aprenderás y descubrirás tu propio sonido al hacer código.


## 2. Usa los FX

¿Ya has dominado lo esencial de Sonic Pi haciendo sonidos con `sample` y `play`? ¿Y ahora, qué? ¿Sabías que Sonic Pi ofrece hasta 27 efectos FX de estudio para cambiar el sonido de tu código? Los efectos son como filtros imagen chulos para los programas de dibujo, sólo que en vez de difuminar o hacer algo blanco y negro, puedes añadir cosas como reverberación, distorsión y eco a tu sonido. Piensa en lo que haces cuando conectas el cable de tu guitarra con un pedal de efectos que te guste, que a su vez va a un amplificador. Por suerte, usar efectos en Sonic Pi es muy fácil, ¡y no necesitas cables! Sólo necesitas escoger la sección de tu código a la que le quieras añadir el efecto, y envolverla con el código del mismo. Vamos a ver un ejemplo. Imagina que tienes el siguiente código:

```
sample :loop_garzul
16.times do
  sample :bd_haus
  sleep 0.5
end
```

Si quieres añadir el efecto al sample `:loop_garzul`, sólo necesitas meter el bucle en un bloque `with_fx`, así:

```
with_fx :flanger do
  sample :loop_garzul
end
16.times do
  sample :bd_haus
  sleep 0.5
end
```

Vale, y ahora si quieres añadir el efecto al pedal de la batería, vas y lo envuelves con `with_fx`, exactamente igual:

```
with_fx :flanger do
  sample :loop_garzul
end
with_fx :echo do
  16.times do
    sample :bd_haus
    sleep 0.5
  end
end
```

Recuerda, puedes envolver *cualquier* código en `with_fx` y cualquier sonido que haya creado el código pasará por ese efecto.


## 3. Parametriza tus sintetizadores

Si estás buscando tu verdadero sonido al hacer código, pronto querrás saber cómo modificar y controlar sintetizadores y efectos. Por ejemplo, puede que quieras cambiar la duración de una nota, añadir más reverberación, o cambiar el tiempo entre ecos. Por suerte, Sonic Pi te da un nivel de control increíble para hacer esto, con unas cosas especiales llamadas parámetros u opciones (abreviado como opts). Copia este código en tu área de trabajo y dale a Ejecutar:

```
sample :guit_em9
```

¡Hey, un sonido de guitarra! Vamos a jugar un poco con ese sonido. ¿Qué te parece si cambiamos su ritmo?

```
sample :guit_em9, rate: 0.5
```

Ey, ¿qué es ese `rate: 0.5` que acabo de añadir al final? Eso se llama opción (opt). Todos los sintetizadores y efectos de Sonic Pi soportan los opt's, y hay montones con los que puedes trastear. También están disponibles para efectos. Prueba esto:

```
with_fx :flanger, feedback: 0.6 do
  sample :guit_em9
end
```

Ahora, ¡prueba a subir la retroalimentación (feedback) a 1, para escuchar sonidos muuy locos! Lee la documentación para más detalles sobre los muchos opts que tienes disponibles.


## 5. Live Code

La mejor forma de experimentar rápido y explorar Sonic Pi es hacer código en vivo (live coding). Esto te permite empezar con algo de código e ir cambiándolo continuamente, retocándolo mientras suena. Por ejemplo, si no sabes qué le hace el parámetro cutoff a un sample, empieza a trastear con ello. ¡Vamos a probar! Copia este código en una de tus áreas de trabajo de Sonic Pi:

```
live_loop :experiment do
  sample :loop_amen, cutoff: 70
  sleep 1.75
end
```

Ahora, dale a Ejecutar y escucharás un break de batería ligeramente amortiguado. Vale, ahora cambia el valor de `cutoff:` a `80` y dale a Ejecutar de nuevo. ¿Escuchas la diferencia? Prueba con `90`, `100`, `110`...

Una vez le has cogido el tranquillo a usar `live_loop`s, verás no puedes dejar de usarlo. Cada vez que toco en un concierto de live coding, confío en mi `live_loop` igual que un/a baterista confía en sus baquetas. Para más información sobre live coding, mira la Sección 9 de el tutorial incluido en el programa.

## 5. Navega por flujos aleatorios

Por último, algo que me encanta es hacer trampa dejando a Sonic Pi que componga cosas por mí. Una forma muy chula de hacer esto es usar randomización. Puede sonar complicado, pero en realidad no lo es. Vamos a echarle un vistazo. Copia esto en un área de trabajo vacía:

```
live_loop :rand_surfer do
  use_synth :dsaw
  notes = (scale :e2, :minor_pentatonic, num_octaves: 2)
  16.times do
    play notes.choose, release: 0.1, cutoff: rrand(70, 120)
    sleep 0.125
  end
end
```

Ahora, cuando toques esto, escucharás un flujo constante de notas aleatorias de la escala `:e2 :minor_pentatonic`, tocada con el sintetizador `:dsaw`. "¡Espera, espera! Eso no es una melodía", te oigo decir entre gritos. Pues bien, aquí viene la primera parte del truco de magia. Cada vez que damos una vuelta en el `live_loop`, le podemos decir a Sonic Pi que resetee el flujo aleatorio a un punto que conozcamos. Vamos a probarlo - añade la línea `use_random_seed_1` al `live_loop`:

```
live_loop :rand_surfer do
  use_random_seed 1
  use_synth :dsaw
  notes = (scale :e2, :minor_pentatonic, num_octaves: 2)
  16.times do
    play notes.choose, release: 0.1, cutoff: rrand(70, 120)
    sleep 0.125
  end
end
```

Ahora, cada vez que el `live_loop` da una vuelta, el flujo de notas aleatorias se resetea. Esto significa que elige las mismas 16 notas cada vez. ¡Abracadabra! Una melodía en el momento. Aquí viene la parte realmente interesante. Cambia el valor de la semilla de `1` a cualquier otro número. Por ejemplo, `4923`. ¡Guau! ¡Otra melodía! O sea, con cambiar un sólo número (la semilla), ¡puedes explorar tantas combinaciones melódicas que te puedas imaginar! He aquí la magia del código.
