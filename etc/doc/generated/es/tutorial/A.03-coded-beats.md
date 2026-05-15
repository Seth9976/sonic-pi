A.3. Beats Programados

# Beats Programados

Uno de los desarrollos tecnológicos más emocionantes y disruptivos en la música moderna fue la invención de los samplers. Eran cajas que te permitían grabar cualquier sonido y manipularlo, reproduciendo estos sonidos grabados de distintas formas muy interesantes. Por ejemplo, podías coger una grabación antigua, encontrar un solo de batería (o un break), grabarlo en tu sampler y después reproducirlo en repetición a la mitad de la velocidad, para tener una base para tus beats. Así es como la música hip-hop se creó en sus inicios; hoy en día es casi imposible encontrar música electrónica que no incorpore samples de algún tipo. Usar samples es una forma maravillosa de introducir fácilmente elementos nuevos e interesantes en tus conciertos de live coding.

¿Y dónde puedes conseguir un sampler? Bueno, ya tienes uno - ¡es tu Raspberry Pi! La aplicación incorporada Sonic Pi incluye un sampler extremadamente potente. ¡Vamos a tocar con él!

## El Amen Break

Uno de los samples de batería más clásicos y reconocibles es el Amen Break. Fue utilizado por primera vez en 1969 en la canción "Amen Brother", de los Winstons, como parte de un break de batería. Sin embargo, no fue conocido hasta que lo descubrieron los primeros músicos de hip-hop de los 80's, quienes lo usaron en samplers; a partir de ahí empezó a usarse muchísimo, y en una gran variedad de estilos como drum and bass, breakbeat, hardcore tecno y breakcore.

Te encantará saber que este sample también está incorporado en Sonic Pi. Limpia un búfer y pega este código:

```
sample :loop_amen
```

Dale a *Ejecutar* y ¡boom! Estás escuchando uno de los breaks de batería más importante de la historia de la música dance. No obstante, este sample no era famoso por ser tocado una vez; estaba pensado para ser tocado en bucle.


## Beat Stretching

Vamos a poner el Amen Break en bucle usando nuestro viejo amigo, el `live_loop` introducido en este tutorial el último mes:

```
live_loop :amen_break do
  sample :loop_amen
  sleep 2
end
```

Vale, está en bucle, pero a cada vuelta se escucha una molesta pausa. Esto pasa porque le dijimos que durmiese `2` pulsos; con el BPM por defecto de 60 que tiene el sample `:loop_amen`, éste sólo dura `1.753` pulsos. Por eso ponemos un silencio de `2 - 1.753 = 0.247` pulsos. Aunque es un silencio corto, se nota.

Para arreglar este problema, podemos usar el opt `beat_stretch:` para decirle a Sonic Pi que estire (stretch) o encoja (shrink) el sample, de modo que éste encaje con el número de beats que queremos.

Las funciones `sample` y `synth` en Sonic Pi te dan mucho control con parámetros opcionales, como `amp:`, `cutoff:` y `release:`. Lo de parámetro opcional suena un poco a trabalenguas, así que vamos a llamarlos *opts* (del inglés *option*, opción) para que sea más fácil.

```
live_loop :amen_break do
  sample :loop_amen, beat_stretch: 2
  sleep 2
end  
```

¡Ahora sí se puede bailar! Tal vez queramos hacerlo más rápido o más lento, para que encaje con el carácter del baile.

## Jugando con el Tiempo

Bueno, ¿y qué pasa si queremos cambiar el estilo a hip hop de la vieja escuela, o a breakcore? Una forma fácil de hacer esto es jugar con el tiempo - o en otras palabras, toquetear el tempo de la canción. Esto es súper fácil en Sonic Pi - sólo necesitas poner un `use_bpm` en tu live loop:

```
live_loop :amen_break do
  use_bpm 30
  sample :loop_amen, beat_stretch: 2
  sleep 2
end 
```

Mientras rapeas con esos beats lentos, fíjate en que seguimos durmiendo 2 y que nuestro BPM es 30, y aun así todo sigue a tiempo. El opt `beat_stretch` trabaja con el BPM actual para asegurarse de que todo suene bien.

Aquí viene la parte graciosa. Mientras el bucle sigue activo, cambia el `30` a `50` en la línea de `use_bpm 30`. ¡Guau, todo ha acelerado, pero sigue *a tiempo*! Intenta ir más rápido - a 80, a 120, ¡y ahora vuélvete loc@ y mete 200!


## Filtrando

Ahora que podemos poner samples en bucle en directo, vamos a ver uno de los opts más divertidos del sintetizador `sample`. El primero es `cutoff:`, que controla el filtro limitador (cutoff) del sampler. Está desactivado por defecto, pero puedes activarlo fácilmente:

```
live_loop :amen_break do
  use_bpm 50
  sample :loop_amen, beat_stretch: 2, cutoff: 70
  sleep 2
end  
```

Adelante, cambia el opt `cutoff`. Por ejemplo, súbelo a 100, dale a *Ejecutar* y espera al bucle que dé una vuelta para escuchar el cambio en el sonido. Fíjate en que valores bajos como 50 suenan suaves, como un bajo; valores altos como 100 o 120 son más completos y ásperos. Esto pasa porque la opción `cutoff:` corta las frecuencias altas del sonido, igual que un cortacésped corta la parte de arrriba de la hierba. La opción `cutoff:` es como la longitud del cortacésped: determina cuánta hierba deja sin cortar.


## Cortando

Otra herramienta muy chula que puedes probar es el efecto de corte (slicer). Éste corta (slice) el sonido. Envuelve la línea que dice `sample` con el código del efecto, tal que así:

```
live_loop :amen_break do
  use_bpm 50
  with_fx :slicer, phase: 0.25, wave: 0, mix: 1 do
    sample :loop_amen, beat_stretch: 2, cutoff: 100
  end
  sleep 2
end
```

Si te das cuenta, el sonido se balancea hacia arriba y hacia abajo un poco más. (Puedes escuchar el sonido original sin el efecto cambiando el opt `mix:` a `0`). Ahora, experimenta con la opción `phase:`. Este controla el ritmo (en pulsos) del efecto de slicing. Un valor más pequeño como `0.125` corta más rápido, y valores grandes como `0.5` cortan más lento. Fíjate en que si doblas o divides por la mitad la opción `phase:` sucesivamente, siempre suena bien. Por último, cambia la opción `wave:` a 0, 1 o 2, y escucha cómo cambia el sonido. Éstos números son distintas formas de onda. 0 es una onda de sierra (inicio duro, salida suave), 1 es una onda cuadrada (inicio duro, salida dura), y 2 es una onda de triángulo (inicio suave, salida suave).


## Juntándolo todo

Por último, vamos a volver atrás en el tiempo y a visitar de nuevo la escena drum and bass de Bristol en sus inicios, con el ejemplo de este mes. No te preocupes si no lo entiendes; tú escríbelo, dale a Ejecutar, y empieza a hacer live coding cambiando los números de los opts, a ver a dónde puedes llegar con ello. Por favor, ¡comparte tus creaciones! Hasta la próxima...

```
use_bpm 100
live_loop :amen_break do
  p = [0.125, 0.25, 0.5].choose
  with_fx :slicer, phase: p, wave: 0, mix: rrand(0.7, 1) do
    r = [1, 1, 1, -1].choose
    sample :loop_amen, beat_stretch: 2, rate: r, amp: 2
  end
  sleep 2
end
live_loop :bass_drum do
  sample :bd_haus, cutoff: 70, amp: 1.5
  sleep 0.5
end
live_loop :landing do
  bass_line = (knit :e1, 3, [:c1, :c2].choose, 1)
  with_fx :slicer, phase: [0.25, 0.5].choose, invert_wave: 1, wave: 0 do
    s = synth :square, note: bass_line.tick, sustain: 4, cutoff: 60
    control s, cutoff_slide: 4, cutoff: 120
  end
  sleep 4
end
```
