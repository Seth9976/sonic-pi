A.12 Cortando Samples

# Cortando Samples

En el episodio 3 de esta serie sobre Sonic Pi vimos como hace loop, alargar y filtrar uno de los más famosos ritmos de batería de todos los tiempos - el ritmo Amen. En este tutorial vamos a llevar esto un paso más allá y aprenderemos como cortarlo en trozos, reordenar los trozos, y pegarlos juntos de nuevo en un orden completamente diferente. Si esto te suena un poco a locura, no te preocupes, pronto será más claro y tu controlarás una nueva y potente herramienta para manejar tus sesiones de live coding.

## El sonido como datos

Antes de que empecemos detengámonos un momento para entender como trabajar con samples. Hasta ahora, hemos jugado con el potente sampler de Sonic Pi. Y si no, probémoslo ahora! Enciende tu Raspberry Pi, inicia Sonic Pi desde el menú Programación, escribe lo que viene a continuación en un buffer nuevo y pulsa el boton Run para poder escuchar un sonido de bateria pre-grabado:

```
sample :loop_amen
```

La grabación de un sonido es simplemente representada como datos - mogollón de números entre -1 y 1 que representan los picos y valles de la onda de sonido. Si reproducimos esos números en orden, oiremos el sonido original. Sin embargo, ¿qué nos impide reproducirlo en un orden diferentes para crear un sonido nuevo?

¿Cómo se graban los samples? En realidad es bastante sencillo, una vez entiendes la física básica del sonido. Cuando haces un sonido (por ejemplo, tocando un tambor), el ruido viaja por el aire, como la superficie de un lago forma ondas cuando lanzas un guijarro al agua. Cuando esas ondas llegan a tus oídos, tu tímpano se mueve y convierte esos movimientos en el sonido que escuchas. Por tanto, si queremos grabar y reproducir un sonido, necesitamos una forma de capturar, guardar y reproducir dichas ondas. Una forma de hacer esto es usando un micrófono, que funciona como un tímpano y se mueve de un lado para otro a medida que las ondas lo golpean. El micrófono convierte su posición en una pequeña señal eléctrica, que se mide varias veces por segundo. Estas mediciones se representan como una serie de números entre -1 y 1.

Si fuéramos a visualizar en un gráfico el sonido sería un simple gráfico con información temporal en el eje x y con la posición del micrófono/altavoz como un valor entre -1 y 1 en el eje y. Puedes ver un ejemplo de un gráfico similar en la parte superior del diagrama.

## Reproducir una parte de un sample

Y, ¿cómo hacemos en Sonic Pi para tocar un sample en un orden diferente? Para responder a esta pregunta, debemos echarle un vistazo a las opciones `start:` y `finish:` de `sample`. Estas opciones nos permiten controlar las posiciones de inicio y fin de reproducción de las mediciones que mencionamos antes. Los valores para estas opciones se representan como un número entre `0` y `1`, donde `0` representa el inicio del sample, y `1` es el final del sample. Entones, para tocar la primera mitad del Amen Break, sólo tenemos que especificar un `finish:` de `0.5`:

```
sample :loop_amen, finish: 0.5
```

Podemos añadir en el valor `start:` para que toque incluso una sección más pequeña del sample:

```
sample :loop_amen, start: 0.25, finish: 0.5
```

Para probar, puedes incluso poner el valor de la opción `finish:` *antes* del `start:` y así tocará la sección macha atrás:

```
sample :loop_amen, start: 0.5, finish: 0.25
```

## Re-ordenando el playback de un sample

Ahora que sabemos que un sample es simplemente una lista de números que puede ser leída en cualquier orden y también cómo tocar una parte específica de un sample podemos empezar a pasarlo bien tocando un sample en el orden 'equivocado'.

![Amen Slices](../../../etc/doc/images/tutorial/articles/A.12-sample-slicing/amen_slice.png)

Vamos a coger el Amen Break y a cortarlo en 8 trozos iguales, y después vamos a cambiar el orden de las piezas. Mira el diagrama: en la parte superior, A) representa el gráfico con los datos del sample original. Al cortarlo en 8 trozos obtenemos B) (fíjate en que le hemos dado un color distinto a cada sample, para que sea fácil distinguirlos). Puedes ver los valores de inicio y fin de cada sample arriba. Finalmente, C) es un posible ordenamiento nuevo de los trozos que tenemos. Ahora podemos reproducir el sample resultante, para crear un beat nuevo. Mira el código para ver cómo hacerlo:

```
live_loop :beat_slicer do
  slice_idx = rand_i(8)
  slice_size = 0.125
  s = slice_idx * slice_size
  f = s + slice_size
  sample :loop_amen, start: s, finish: f
  sleep sample_duration :loop_amen, start: s, finish: f
end
```

1. Elegimos un trozo al azar para tocar, que debe ser un numero aleatorio entre 0 y 7 (recuerda que tenemos que empezar a contar desde 0). Sonic Pi tiene una función adecuada para esto: `rand_i(8)`. Entonces guardamos el índice aleatorio del trozo en la variable `slice_idx`.
2. Definimos nuestro `slice_size` que es 1/8 o 0.125. El `slice_size` es necesario apara nosotros para convertir `slice_idx` en un valor entre 0 y 1 para que podamos usarlo en nuestra opcion `start:`.
3. Calculamos la posición de inicio `s` multiplicando `slice_idx` por `slice_size`.
4. Calculamos la posición final `f` añadiendo el `slice_size` a la posición de inicio `s`.
5. Ahora podemos tocar el trozo de sample pasando los valores `s` y `f` en las opciones `start:` y `finish:` de `sample`.
6. Antes de tocar el siguiente trozo necesitamos saber cuánto necesitamos esperar ( `sleep`). Esto debería de ser la duración del trozo del sample. Por suerte Sonic Pi nos ayuda con `sample_duration` que acepta las mismas opciones que `sample` y simplemente nos devuelve la duración. Así pues, pasando a `sample_duration` nuestras opciones `start:` y `finish:`, podemos saber la duración de un solo trozo.
7. Ponemos todo este código en un 'live_loop' para continuar seleccionando aleatoriamente trozos nuevos que tocar.


## Juntándolo todo

Combinemos ahora todo lo que hemos visto hasta ahora en un ejemplo final donde demostraremos cómo podemos tomar una estrategia similar para combinar aleatoriamente ritmos cortados con un ritmo de bajo para crear el inicio de un tema interesante. Ahora es tu turno - usa el código que hay aquí abajo como punto de inicio e intenta ver si puedes llevarlo en tu propia dirección para crear algo nuevo...

```
live_loop :sliced_amen do
  n = 8
  s =  line(0, 1, steps: n).choose
  f = s + (1.0 / n)
  sample :loop_amen, beat_stretch: 2, start: s, finish: f
  sleep 2.0  / n
end
live_loop :acid_bass do
  with_fx :reverb, room: 1, reps: 32, amp: 0.6 do
    tick
    n = (octs :e0, 3).look - (knit 0, 3 * 8, -4, 3 * 8).look
    co = rrand(70, 110)
    synth :beep, note: n + 36, release: 0.1, wave: 0, cutoff: co
    synth :tb303, note: n, release: 0.2, wave: 0, cutoff: co
    sleep (ring 0.125, 0.25).look
  end
end
```
