A.13 Programando un Secuenciador Probabilístico

# Programando un Secuenciador Probabilístico

En un episodio anterior de esta serie Sonic Pi exploramos el poder de la aleatoriedad para introducir variedad, sorpresa y cambio en nuestras pistas y actuaciones codificadas en directo. Por ejemplo, elegimos al azar notas de una escala para crear melodías interminables. Hoy vamos a aprender una nueva técnica que utiliza la aleatoriedad para el ritmo: ¡los ritmos probabilísticos!

## Portabilidad

Antes de empezar a crear nuevos ritmos y sintetizadores, tenemos que hacer una rápida inmersión en los fundamentos de la probabilidad. Esto puede sonar desalentador y complicado, pero en realidad es tan sencillo como tirar un dado, ¡de verdad! Cuando coges un dado de tablero normal de 6 caras y lo tiras, ¿qué ocurre en realidad? Bueno, en primer lugar, sacas un 1, 2, 3, 4, 5 o 6 con exactamente la misma probabilidad de obtener cualquiera de los números. De hecho, dado que es un dado de 6 caras, por término medio (si tiras muchas veces) sacarás un 1 cada 6 tiradas. Esto significa que tienes una probabilidad de 1 entre 6 de sacar un 1. Podemos emular las tiradas de dados en Sonic Pi con el fn `dice`. Tiremos uno 8 veces:

```
8.times do
  puts dice
  sleep 1
end
```

Fíjate en que el registro imprime los valores entre 1 y 6 como si hubiéramos tirado un dado real nosotros mismos.

## Semillas aleatorias

Escuchemos como suena:

```
live_loop :random_beat do
  sample :drum_snare_hard if dice == 1
  sleep 0.125
end
```


Vamos a repasar rápidamente cada línea para asegurarnos de que todo está muy claro. Primero creamos un nuevo `live_loop` llamado `:random_beat` que repetirá continuamente las dos líneas entre `do` y `end`. La primera de estas líneas es una llamada a `sample` que reproducirá un sonido pregrabado (el sonido `:drum_snare_hard` en este caso). Sin embargo, esta línea tiene un final condicional especial `if`. Esto significa que la línea sólo se ejecutará si la declaración a la derecha del `if` es `true` (verdadero). La sentencia en este caso es `dice == 1`. Esto llama a nuestra función `dice` que, como hemos visto, devuelve un valor entre 1 y 6. A continuación, utilizamos el operador de igualdad `==` para comprobar si este valor es `1`. Si es `1`, la sentencia se resuelve en `true` y nuestro tambor suena, si no es `1` la sentencia se resuelve en `false` y el tambor se salta. La segunda línea simplemente espera durante `0.125` segundos antes de volver a tirar los dados.

## Cambiando las Probabilidades

Los que hayan jugado a juegos de rol estarán familiarizados con un montón de dados de formas extrañas con diferentes rangos. Por ejemplo, está el dado con forma de tetraedro que tiene 4 caras e incluso un dado de 20 caras con forma de icosaedro. El número de caras de los dados cambia la posibilidad, o probabilidad, de sacar un 1. Cuantas menos caras, más probabilidades hay de sacar un 1 y cuantas más caras, menos probabilidades. Por ejemplo, con un dado de 4 caras, hay una posibilidad entre 4 de sacar un 1 y con un dado de 20 caras hay una posibilidad entre 20. Por suerte, Sonic Pi tiene el práctico fn `one_in` para describir exactamente esto. Vamos a jugar:

```
live_loop :different_probabilities do
  sample :drum_snare_hard if one_in(6)
  sleep 0.125
end
```

Inicia el bucle en vivo de arriba y escucharás el conocido ritmo aleatorio. Sin embargo, no detengas la ejecución del código. En su lugar, cambia el `6` a un valor diferente como `2` o `20` y pulsa el botón `Run` de nuevo. Observa que los números más bajos significan que el redoblante suena con más frecuencia y los números más altos significan que el redoblante se dispara menos veces. ¡Estás haciendo música con probabilidades!

## Combinando las Probabilidades

Las cosas se ponen interesantes cuando combinas varios samples que se disparan con distintas probabilidades. Por ejemplo:

```
live_loop :multi_beat do
  sample :elec_hi_snare if one_in(6)
  sample :drum_cymbal_closed if one_in(2)
  sample :drum_cymbal_pedal if one_in(3)
  sample :bd_haus if one_in(4)
  sleep 0.125
end
```

De nuevo, ejecuta el código de arriba y empieza a cambiar las probabilidades para modificar el ritmo. También puedes cambiar los samples para crear una sensación completamente nueva. Por ejemplo, ¡prueba a cambiar `:drum_cymbal_closed` por `:bass_hit_c` si quieres una sobredosis de bajo!


## Ritmos Repetibles

A continuación, podemos utilizar nuestro viejo amigo `use_random_seed` para reiniciar el flujo aleatorio después de 8 iteraciones para crear un ritmo regular. Escribe el siguiente código para escuchar un ritmo mucho más regular y repetitivo. Una vez que escuches el ritmo, intenta cambiar el valor de la semilla de `1000` a otro número. Observa cómo diferentes números generan diferentes ritmos.

```
live_loop :multi_beat do
  use_random_seed 1000
  8.times do
    sample :elec_hi_snare if one_in(6)
    sample :drum_cymbal_closed if one_in(2)
    sample :drum_cymbal_pedal if one_in(3)
    sample :bd_haus if one_in(4)
    sleep 0.125
  end
end
```

Una cosa que suelo hacer con este tipo de estructura es recordar qué semillas suenan bien y apuntarlas. De esta forma, puedo recrear fácilmente mis ritmos cuando vuelva a practicar la canción, o cuando la toque en un concierto en el futuro.

## Juntándolo todo

Por último, podemos añadir unos bajos aleatorios para darle un buen contenido melódico. Fíjate en que también podemos utilizar nuestro recién descubierto método de secuenciación probabilística tanto en sintetizadores como en samples. Pero no te quedes ahí: ¡ajusta los números y crea tu propia pista con el poder de las probabilidades!

```
live_loop :multi_beat do
  use_random_seed 2000
  8.times do
    c = rrand(70, 130)
    n = (scale :e1, :minor_pentatonic).take(3).choose
    synth :tb303, note: n, release: 0.1, cutoff: c if rand < 0.9
    sample :elec_hi_snare if one_in(6)
    sample :drum_cymbal_closed if one_in(2)
    sample :drum_cymbal_pedal if one_in(3)
    sample :bd_haus, amp: 1.5 if one_in(4)
    sleep 0.125
  end
end
```
