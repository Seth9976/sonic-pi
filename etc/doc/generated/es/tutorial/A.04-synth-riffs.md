A.4 Riffs con sintetizadores

# Riffs con Sintetizadores

No importa si es el giro hechizante de los estrepitosos osciladores, o el golpe desafinado de las ondas de sierra perforando el remix; el sintetizador que lleva la melodía (lead synth) tiene un papel muy importante en cualquier canción de música electrónica. En la edición del último mes de esta serie de tutoriales, vimos cómo programar nuestros beats. En este tutorial, vamos a ver cómo programar los tres componentes fundamentales de un buen riff de sintetizador - timbre, melodía y ritmo.

Vale, pues enciende tu Raspberry Pi, abre Sonic Pi v2.6+ y, ¡vamos a hacer ruido!


## Posibilidades Tímbricas

Una parte esencial de cualquier riff de sintetizador es cambiar y jugar con el timbre de los sonidos. Podemos controlar el timbre en Sonic Pi de dos formas - eligiendo diferentes sintetizadores para un cambio drástico, y cambiando los opts de un mismo sintetizador para un cambio más suave. También podemos usar efectos, pero eso lo veremos en otro tutorial...

Vamos a crear un live loop sencillo, donde podamos cambiar constantemente el sintetizador actual:

```
live_loop :timbre do
  use_synth (ring :tb303, :blade, :prophet, :saw, :beep, :tri).tick
  play :e2, attack: 0, release: 0.5, cutoff: 100
  sleep 0.5
end
```

Échale un vistazo al código. Sólo estamos haciendo ticking sobre un anillo de nombres de sintetizador (esto recorre cada uno de ellos y repite la lista una y otra vez). Le pasamos este nombre de sintetizador a la función (abreviada como fn) `use_synth`, que cambia el sintetizador actual del `live_loop`. También estamos tocando la nota `:e2` (Mi en la segunda octava), con una separación de 0.5 pulsos (medio segundo con el BPM de 60 que está por defecto), y con la opción `cutoff:` puesta a 100.

Escucha cómo los distintos sintetizadores tienen sonidos muy diferentes, aunque estén tocando la misma nota. Ahora, experimenta y juega con ello. Cambia el tiempo de decaimiento (release) a valores más altos y más bajos. Por ejemplo, cambia las opciones `attack:` y `release:` para ver cómo distintos tiempos de entrada y salida pueden cambiar considerablemente el sonido. Finalmente, cambia el opt `cutoff:` para ver cómo distintos valores de cutoff pueden cambiar notablemente el timbre (con valores entre 60 y 130 está bien). Mira cuántos sonidos distintos puedes crear con sólo cambiar unos pocos valores. Una vez le cojas el tranquillo, ve a la pestaña de Sintetizadores en el sistema de Ayuda si quieres una lista completa de todos los sintetizadores y de todas las opciones disponibles para cada sintetizador, para que veas cuánto poder tienes en tus manos de programador/a.

## Timbre

Timbre es una palabra guay para describir cómo suena un sonido. Si tocas la misma nota con distintos instrumentos como un violín, una guitarra o un piano, la altura (cómo de agudo o grave suena) es la misma en todos ellos, pero la cualidad del sonido cambia. Esa cualidad del sonido - aquello que te permite diferenciar un piano de una guitarra, es el timbre.


## Composición Melódica

Otro aspecto importante para nuestro sintetizador lead son las notas que queremos que toque. Si ya tienes una idea, sólo tienes que crear un anillo con las notas que has pensado, y hacer ticking sobre ellas:

```
live_loop :riff do
  use_synth :prophet
  riff = (ring :e3, :e3, :r, :g3, :r, :r, :r, :a3)
  play riff.tick, release: 0.5, cutoff: 80
  sleep 0.25
end
```
    
Aquí hemos definido nuestra melodía con un anillo, que incluye notas como `:e3` y silencios representados como un ':r'. Después usamos `.tick` para recorrer cada nota, lo que nos da un riff repetitivo.

## Melodía Automática

No siempre es fácil inventarse una melodía desde cero. Es más fácil pedirle a Sonic Pi una selección de riffs aleatorios y esocoger aquel que te guste más. Para ello tenemos que combinar tres cosas: anillos, aleatoriedad y semillas aleatorias. Veamos un ejemplo:

```
live_loop :random_riff do
  use_synth :dsaw
  use_random_seed 3
  notes = (scale :e3, :minor_pentatonic).shuffle
  play notes.tick, release: 0.25, cutoff: 80
  sleep 0.25
end
```

Hay varias cosas que tenemos que revisar - vamos a verlas una por una. Primero, estamos usando una semilla aleatoria de 3. ¿Qué significa esto? Bien, hacer esto es muy útil porque, cuando especificamos la semilla, podemos predecir cuál va a ser el siguiente valor aleatorio - ¡igual que la última vez que pusimos la semilla a 3! Otra cosa útil que te viene bien saber, es que desordenar un anillo de notas funciona de la misma forma. En el ejemplo de arriba estamos preguntando por el 'tercer desorden' en la lista estándar de formas de desordenar notas - que también es igual cada vez porque siempre estamos usando la misma semilla para el mismo valor, justo antes de que empecemos a desordenar las notas. Finalmente, estamos haciendo ticking, recorriendo nuestras notas desordenadas para tocar el riff.

Ahora es cuando empieza la diversión. Si cambiamos el valor de la semilla aleatoria a otro número, como 3000, obtenemos una mezcla de notas totalmente distinta. Así es muy fácil explorar nuevas melodías. Sencillamente, escoge la lista de notas que queremos mezclar (una escala está bien para empezar), y selecciona la semilla con la que queremos mezclarlas. Si no te gusta la melodía, sólo tienes que cambiar las notas o la semilla, y probar de nuevo. ¡Repite hasta que te guste lo que escuchas!


## Pseudoaleatoriedad

La aleatoriedad en Sonic Pi no es aleatoria en realidad; es lo que se conoce como pseudoaleatoria. Imagina que tienes que tirar un dado 100 veces y luego escribes los resultados en una hoja de papel. Sonic Pi tiene algo parecido esta lista de resultados, que utiliza cuando le pides un valor aleatorio. Ajustar la semilla aleatoria es como saltar a un punto específico en dicha lista.
 
## Encontrando tu Ritmo

Otro aspecto importante de nuestro riff es el ritmo - cuándo tocar una nota y cuándo no. Como vimos arriba, podemos usar `:r` en nuestros anillos para insertar silencios. Otra forma muy potente de hacer esto es usar extensiones (spreads), que veremos en otro tutorial. Hoy vamos a usar la aleatoriedad para que nos ayude a encontrar nuestro ritmo. En vez de tocar cada nota, vamos a usar una condición para tocar una nota con una determinada probabilidad. Vamos a echarle un vistazo:

```
live_loop :random_riff do
  use_synth :dsaw
  use_random_seed 30
  notes = (scale :e3, :minor_pentatonic).shuffle
  16.times do
    play notes.tick, release: 0.2, cutoff: 90 if one_in(2)
    sleep 0.125
  end
end
```

Una función muy útil que deberías conocer es `one_in`, que nos devuelve un valor `true` (verdadero) o `false` (falso) con una determinada probabilidad. Vamos a usar un valor de 2, así que, de media, una de cada dos llamadas a `one_in` va a devolver `true`. Dicho de otra forma, el 50% del tiempo vamos a recibir `true`. Usar valores más altos devolverá `false` más a menudo, introduciendo más espacio en el riff.

Fíjate en que hemos añadido algo de iteración con el `16.times`. Esto es porque sólo queremos resetear el valor de nuestra semilla aleatoria cada 16 notas, de modo que nuestro ritmo se repita cada 16 veces. Esto no afecta a la mezcla, ya que dicha operación se realiza justo después de que se ajuste la semilla. Podemos usar el tamaño de la iteración para cambiar la duración del riff. Prueba a cambiar el 16 a 8, o incluso a 4 o 3, y mira cómo afecta al ritmo del riff.

## Juntándolo todo

Vale, ahora vamos a combinar todo lo que hemos aprendido en un ejemplo final. ¡Hasta la próxima!

```
live_loop :random_riff do
  #  uncomment to bring in:
  #  synth :blade, note: :e4, release: 4, cutoff: 100, amp: 1.5
  use_synth :dsaw
  use_random_seed 43
  notes = (scale :e3, :minor_pentatonic, num_octaves: 2).shuffle.take(8)
  8.times do
    play notes.tick, release: rand(0.5), cutoff: rrand(60, 130) if one_in(2)
    sleep 0.125
  end
end
 
live_loop :drums do
  use_random_seed 500
  16.times do
    sample :bd_haus, rate: 2, cutoff: 110 if rand < 0.35
    sleep 0.125
  end
end
 
live_loop :bd do
  sample :bd_haus, cutoff: 100, amp: 3
  sleep 0.5
end
```
