A.9 Randomización

# Navegando por Flujos Aleatorios

En el episodio 4 de esta serie de tutoriales, vimos brevemente la aleatoriedad al programar unos chispeantes riffs de sintertizador. Ya que la aleatoriedad es una parte tan importante de mis conciertos de live coding, pensé que sería útil enseñarte los fundamentos en mayor detalle. Así pues, vamos manos a la obra, ¡a navegar por los flujos aleatorios!

## No existe lo aleatorio

Lo primero que debes aprender y que probablemente te sorprenda cuando empieces a tocar con las funciones aleatorias de Sonic Pi, es que en realidad no son aleatorias. ¿Qué significa esto? Vamos a hacer unas cuantas pruebas. Primero, imagínate un número en tu cabeza de 0 a 1. Recuérdalo y no me lo digas. Ahora déjame adivinar... era `0.321567`? Bah, esto no se me da bien. Déjame intentarlo de nuevo, pero ahora le vamos a pedir a Sonic Pi que escoja el número. Abre Sonic Pi v2.7+ (versión 2.7 o posterior) y pídele un número aleatorio, pero de nuevo, no me digas cuál es:

```
print rand
```

Veamos... ¿es `0.75006103515625`? ¡Sí! Ja ja, veo que no te lo crees. Bueno, tal vez tuve suerte. Probemos de nuevo. Dale al botón de Ejecutar y mira qué te sale... ¿Cómo? ¿Otra vez `0.75006103515625`? ¡Esto no puede ser aleatorio! Tienes razón, no lo es.

¿Qué está pasando? En estos casos l@s informátic@s usamos el palabrejo determinismo. Sencillamente, esta palabra significa que nada ocurre al azar y que todo está predestinado. Tu versión de Sonic Pi está destinada a devolver siempre `0.75006103515625` en el programa de arriba. Esto puede parecerte inútil, pero voy a intentar convencerte de que es una de las partes más potentes de Sonic Pi. Si me sigues, aprenderás cómo confiar en la naturaleza determinista de la aleatoriedad de Sonic Pi, y la usarás como un bloque de construcción fundamental para tus composiciones y conciertos de live coding.

## Una melodía aleatoria

Cuando abres Sonic Pi, éste carga en memoria una secuencia de 441.000 valores aleatorios generados previamente. Cuando llamas a una función aleatoria como `rand` o `rrand`, se usa esta lista de valores aleatorios para generar tu resultado. Cada llamada a una función aleatoria consume un valor de esta lista. Por tanto, la décima llamada a una función aleatoria usará el décimo valor de la lista. Además, cada vez que pulsas el botón de Ejecutar, la lista se resetea para esa ejecución. Por eso pude adivinar el resultado de `rand`, y por eso la melodía 'aleatoria' era siempre la misma. La versión de Sonic Pi de todo el mundo usa exactamente la misma lista aleatoria, lo que es muy importante cuando empezamos a compartir nuestras canciones con otras personas.

Vamos a usar lo que hemos aprendido para generar una melodía aleatoria que se repita:

```
8.times do
 play rrand_i(50, 95)
 sleep 0.125
end
```

Escribe esto en un búfer vacío y dale a Ejecutar. Escucharás una melodía hecha con notas 'aleatorias' entre 50 y 95. Cuando haya acabado, dale a Ejecutar de nuevo y escucharás exactamente la misma melodía.

## Funciones Aleatorias Útiles

Sonic Pi contiene un montón de funciones para trabajar con la lista de valores aleatorios. Aquí tienes una lista de las más útiles:

* `rand` - Sencillamente devuelve el siguiente valor de la lista aleatoria
* `rrand` - Devuelve un valor aleatorio dentro de un rango
* `rrand_i` - Devuelve un número entero aleatorio dentro de un rango
* `one_in` - Devuelve verdadero o falso dependiendo con una probabilidad determinada
* `dice` - Imita el lanzar un dado, devolviendo un valor entre 1 y 6
* `choose` - Elige un valor aleatorio de una lista

Mira la documentación en el sistema de Ayuda para más información y ejemplos.

## Reseteando la Lista

Aunque poder repetir una secuencia de notas es esencial para poder reproducir tu riff en el concierto, puede que no sea exactamente lo que buscas. ¿No sería genial poder probar con distintos riffs y escoger el que mśa te guste? Aquí es donde empieza la magia.

Podemos ajustar la lista de valores aleatorios manualmente, con la función `use_random_seed`. En informática, una semilla aleatoria (random seed) es el punto de partida desde el cual una nueva lista de valores aleatorios puede brotar y florecer. Vamos a probarlo:

```
use_random_seed 0
3.times do
  play rrand_i(50, 95)
  sleep 0.125
end
```

¡Genial!, tenemos las tres primeras notas de nuestra melodía aleatoria de arriba: `84`, `83` y `71`. Ahora podemos cambiar la semilla a un valor distinto. A ver qué te parece así:

```
use_random_seed 1
3.times do
  play rrand_i(50, 95)
  sleep 0.125
end
```

Interesante, obtenemos `83`, `71` y `61`. Puede que te hayas fijado en que los dos primeros números son los mismos que los dos últimos números de antes... no es una coincidencia.

Recuerda que la lista de valores es sencillamente una lista gigante de valores previamente calculados. Usar una semilla aleatoria sólo nos hace saltar a un punto dentro de la lista. Otra forma de verlo: imagínate una baraja de cartas gigante que ha sido barajada previamente. Usar una semilla aleatoria es como cortar la baraja en un punto. Lo mejor de esto es precisamente que puedes saltar por la lista de valores, lo que nos da un gran poder cuando hacemos música.

Vamos a visitar de nuevo nuestra melodía aleatoria de 8 notas con nuestra nueva habilidad para resetear la lista aleatoria, pero ahora vamos a meter un live loop, para poder probar cosas mientras suena:

```
live_loop :random_riff do    
  use_random_seed 0
  8.times do
    play rrand_i(50, 95), release: 0.1
    sleep 0.125
  end
end
```
  
Ahora, mientras sigue sonando, cambia el valor de la semilla de `0` a algo distinto. Prueba `100`, o `999`. Prueba con tus propios valores, experimenta y juega con ello: a ver qué semilla genera el riff que más te guste.

## Juntándolo todo

El tutorial de este mes ha sido una inmersión un poco técnica en el sistema de aleatoriedad de Sonic Pi. Con suerte, te habrá servido para entender cómo funciona y cómo puedes usar la aleatoriedad con seguridad, creando patrones reproducibles en tu música. Es importante recalcar que puedes usar dicha aleatoriedad reproducible *donde quieras*. Por ejemplo, puedes hacer aleatorios la amplitud de las notas, el ritmo, la cantidad de reverberación, el sintetizador actual, la cantidad de mezcla de un efecto, etc. En el futuro veremos de cerca alguna de estas aplicaciones, pero de momento, déjame enseñarte un ejemplo corto.

Escribe el siguiente código en un búfer vacío, dale a Ejecutar, y empieza a cambiar las semillas, dale a Ejecutar de nuevo (mientras suena) y explora distintos sonidos, ritmos y melodías que puedes hacer. Cuando hayas encontrado algo que te guste, recuerda el valor de la semilla para que puedas usarla más tarde. Finalmente, cuando hayas encontrado unas cuantas semillas que te gusten, márcate un concierto de live coding para tus amig@s; sólo tienes que cambiar entre tus semillas favoritas, creando una pieza completa.

```
live_loop :random_riff do
  use_random_seed 10300
  use_synth :prophet
  s = [0.125, 0.25, 0.5].choose
  8.times do
    r = [0.125, 0.25, 1, 2].choose
    n = (scale :e3, :minor).choose
    co = rrand(30, 100)
    play n, release: r, cutoff: co
    sleep s
  end
end
live_loop :drums do
  use_random_seed 2001
  16.times do
    r = rrand(0.5, 10)
    sample :drum_bass_hard, rate: r, amp: rand
    sleep 0.125
  end
end
```
