A.18 Diseño Sonoro - Síntesis Aditiva

# Síntesis Aditiva

Esta es la primera parte de una miniserie de artículos donde veremos cómo usar Sonic Pi para hacer diseño sonoro. Haremos un recorrido por las distintas técnicas que tienes para fabricar tu propio sonido. La primera técnica que vamos a ver se llama *síntesis aditiva*. Puede sonarte complicado, pero el significado se entiende muy fácil si vemos cada palabra por separado. Primero, aditivo significa combinar cosas; segundo, síntesis significa crear sonido. Por tanto, síntesis aditiva no es más que *combinar sonidos que tienes para crear sonidos nuevos*. Esta técnica de síntesis se usa desde hace mucho tiempo; por ejemplo, los órganos de la edad media tienen un montón de tubos con sonidos muy variados, que puedes controlar con unas palancas. Si tiras de la palanca de un tubo, lo estás 'añadiendo a la mezcla', haciendo el sonido mucho más rico y complejo. Vamos a ver ahora cómo podemos tirar de todas las palancas en Sonic Pi.


## Combinaciones Sencillas

Empecemos con el sonido más sencillo - la humilde y pura onda de seno:

```
synth :sine, note: :d3
```

Ahora, veamos cómo estos sonidos se combinan con una onda cuadrada:

```
synth :sine, note: :d3
synth :square, note: :d3
```

¿Has visto cómo se han juntado los dos sonidos para formar uno más rico? Y no hay por qué parar ahí; podemos añadir tantos sonidos como queramos. En cualquier caso, hay que tener cuidado con cuántos sonidos juntamos. Cuando mezclamos muchos pigmentos para crear nuevos colores, acabamos con un color marrón muy feo; del mismo modo, mezclar muchos sonidos genera un sonido muy sucio.


## Mezclando

Vamos a añadir algo al sonido que tenemos para hacerlo un poco más brillante. Podríamos usar una onda triangular una octava por encima (para obtener ese sonido brillante), y ponerle sólo `0.4` de amplitud (amp) para que añada al sonido en vez de taparlo:

```
synth :sine, note: :d3
synth :square, note: :d3
synth :tri, note: :d4, amp: 0.4
```

Ahora, prueba a crear tus propios sonidos combinando 2 o más sintetizadores, con distintas octavas y amplitudes. Fíjate también en que puedes jugar con las opciones de cada sintetizador para modificar cada fuente de sonido antes de mezclarla con el resto; así puedes conseguir más combinaciones de sonidos.


## Desafinación (detuning)

Hasta ahora, al combinar nuestros sintetizadores, sólo hemos usado la misma altura o hemos cambiado alguno de octava. ¿Cómo sonaría si en vez de restringirnos a octavas, usáramos notas un poco más agudas o un poco más graves? Vamos a probarlo:

```
detune = 0.7
synth :square, note: :e3
synth :square, note: :e3 + detune
```

Si desafinamos nuestras ondas cuadradas 0.7 notas, podemos escuchar que suenan 'mal'. Sin embargo, a medida que nos acercamos a 0, sonará cada vez mejor, ya que la altura de las dos ondas se parece cada vez más. ¡Pruébalo! Cambia el valor del opt `detune:` de `0.7` a `0.5` y escucha el nuevo sonido. Prueba con `0.2`, `0.1`, `0.05`, `0`. Cada vez que cambies el valor, dale a Ejecutar y comprueba cómo cambia el sonido. Fíjate en que los valores bajos de desafinación (detune) como `0.1` producen un sonido 'grueso' muy agradable; las alturas de las ondas interactúan de formas muy interesantes, y a menudo sorprendentes.

Algunos de los sintes incorporados ya incluyen una opción de desafinación que hace exactamente esto en un sinte. Prueba a jugar con la opción `detune:` de `:dsaw`, `:dpulse` y `:dtri`.


## Amplitud

También podemos pulir nuestro sonido usando distintas opciones y envolventes (envelope) para el disparador de cada sintetizador. Con esto puedes, por ejemplo, hacer que parte del sonido sea percusiva, y que otra parte resuene durante un rato.

```
detune = 0.1
synth :square, note: :e1, release: 2
synth :square, note: :e1 + detune, amp: 2, release: 2
synth :gnoise, release: 2, amp: 1, cutoff: 60
synth :gnoise, release: 0.5, amp: 1, cutoff: 100
synth :noise, release: 0.2, amp: 1, cutoff: 90
```

En el ejemplo de arriba, le he añadido un elemento percusivo y ruidoso al sonido; además, le he puesto un poco de ruido de fondo que dura un rato. Para ello, primero he usado dos sintetizadores de ruido con valores de corte (cutoff) normales (`90` y `100`) y con tiempos para soltar (release) cortos; además, he usado un sintetizador de ruido con un tiempo de release mayor pero con un valor de cutoff bajo (lo que hace al ruido menos brillante y más resonante.)

## Juntándolo todo

Vamos a combinar todas estas técnicas; vamos usar síntesis aditiva para recrear un sonido sencillo de campana. He dividido este ejemplo en cuatro secciones. En primer lugar, tenemos la sección 'hit' (golpe), que es la parte inicial del sonido de la campana, así que usa un envolvente corto (por ejemplo, un `release:` alrededor de `0.1`). Después tenemos la parte del timbre largo de la campana, en la que estoy usando el sonido puro de una onda de seno. Fíjate en que suelo aumentar la nota en `12` y `24`, que es el número de notas en una y dos octavas, respectivamente. También le he metido unos cuantos sintetizadores de onda de seno graves, para darle al sonido un poco de bajo y de profundidad. Por último, he usado `define` para guardar mi código en una función que puedo usar para tocar una melodía. ¡Prueba a tocar tu propia melodía, y juega con los contenidos de la función `:bell` hasta que tengas un sonido muy loco que te guste!

```
define :bell do |n|
  # Triangle waves for the 'hit'
  synth :tri, note: n - 12, release: 0.1
  synth :tri, note: n + 0.1, release: 0.1
  synth :tri, note: n - 0.1, release: 0.1
  synth :tri, note: n, release: 0.2
  # Sine waves for the 'ringing'
  synth :sine, note: n + 24, release: 2
  synth :sine, note: n + 24.1, release: 2
  synth :sine, note: n + 24.2, release: 0.5
  synth :sine, note: n + 11.8, release: 2
  synth :sine, note: n, release: 2
  # Low sine waves for the bass
  synth :sine, note: n - 11.8, release: 2
  synth :sine, note: n - 12, release: 2
end
# Play a melody with our new bell!
bell :e3
sleep 1
bell :c2
sleep 1
bell :d3
sleep 1
bell :g2
```
