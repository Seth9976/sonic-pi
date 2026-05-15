A.5 Acid Bass

# Acid Bass

Es imposible mirar en la historia de la música electrónica dance sin ver el enorme impacto del pequeño sintetizdor Roland TB-303. Es la salsa secreta del sonido de bajo ácido (acid bass). Esos clásicos riffs de bajo chillones del TB-303 se pueden escuchar desde los inicios de la música House en Chicago, hasta en artistas electrónic@s más recientes como Plastikman, Squarepusher y Aphex Twin.

Curiosamente, Roland no diseñó el TB-303 para música de baile. Se creó inicialmente como una ayuda para practicar para guitarristas. Pensaron que la gente podría programarlos para tocar líneas de bajo eléctrico que sirviesen de acompañamiento. Por desgracia había varios problemas: eran un poco difíciles de programar, no sonaban muy bien como substitutos para un bajo eléctrico, y eran bastante caros. Para contrarrestar sus pérdidas, Roland dejó de fabricarlos después de vender 10.000 unidades, y después de unos años cogiendo polvo en armarios de guitarristas, era fácil verlos en los escaparates de tiendas de segunda mano. Estos TB-303 solitarios estaban esperando a ser descubiertos por una nueva generación de experimentador@s que empezaron a usarlos de formas en las que Roland no se habría imaginado, creando sonidos muy guays. Así nació el Acid House.

Aunque no es fácil tocar un TB-303 original, te alegrará saber que puedes convertir tu Raspberri Pi en este sintetizador usando el poder de Sonic Pi. Abre Sonic Pi, pon este código en un búfer vacío, dale a Ejecutar y contempla...:

```
use_synth :tb303
play :e1
```
    
¡Acid bass al instante! Vamos a cambiar cosillas...

## Que Chille ese Bajo

Primero, vamos a construir un arpegiador en directo para hacer las cosas más divertidas. En el último tutorial vimos cómo un riff puede ser sencillamente un anillo de notas sobre las que hacemos tick una tras otra, repitiéndolas cuando llegamos al final. Vamos a crear un live loop que haga justo eso:

```
use_synth :tb303
live_loop :squelch do
  n = (ring :e1, :e2, :e3).tick
  play n, release: 0.125, cutoff: 100, res: 0.8, wave: 0
  sleep 0.125
end
```

Échale un vistazo a cada línea.

1. En la primera línea ponemos el sintetizador por defecto como `tb303`, con la función `use_synth`.
2. En la línea dos creamos un live loop llamado `:squelch` (chillar) que sencillamente da vueltas, vueltas y más vueltas.
3. En la línea tres es donde creamos nuestro riff - un anillo de notas (Mi en las octavas 1, 2 y 3), que recorremos con `.tick`. Definimos `n` para representar la nota actual dentro del riff. El símbolo de igual asigna el valor de la derecha al nombre de la izquierda. Este valor será diferente en cada vuelta del bucle. La primera vez, `n` valdrá `:e1`. La segunda vez valdrá `:e2`, luego `:e3`, y así hasta volver a `:e1`, dando vueltas para siempre.
4. En la línea cuatro es donde realmente usamos nuestro sintetizador `:tb303`. Le estamos pasando varios opts interesantes: `release:`, `cutoff:`, `res:` y `wave:`, que vamos a ver a continuación.
5. La línea cinco es nuestro `sleep` - le estamos diciendo al live loop que dé una vuelta cada `0.125`, o 8 veces cada segundo si utilizamos el BPM por defecto (60).
6. La línea séis es el `end` (fin) de nuestro live loop. Esto le dice a Sonic Pi dónde acaba el live loop, sencillamente.

Mientras te preguntas qué está pasando, escribe el código de arriba en Sonic Pi y dale a Ejecutar. Deberías escuchar el `:tb303` en acción. Ahora viene lo bueno: vamos a hacer live coding.

Mientras el bucle sigue funcionando, cambia el opt `cutoff:` a `110`. Ahora dale al botón de Ejecutar de nuevo. Deberías escuchar que el sonido se hace un poco más duro y chillón. Cambia de nuevo el `cutoff:` a `120` y dale a Ejecutar. Ahora con `130`. Escucha cómo frecuencias de cutoff más bajas hacen que suene más penetrante e intenso. Finalmente, bájalo a `80`, con lo que sentirás una sensación de respiro. Repite tantas veces como quieras. No te preocupes, yo sigo por aquí...

Otra opción que merece la pena probar es `res:`. Ésta controla el nivel de resonancia del filtro. Una resonancia alta es típica de sonidos de acid bass. Ahora mismo tenemos nuestro `res:` a `0.8`. Prueba a cambiarlo a `0.85`, luego a `0.9`, y finalmente a `0.95`. Tal vez notes que un cutoff como `110` o más alto hace las diferencias más fáciles de escuchar. Ahora haz una locura y cámbialo a `0.999` para escuchar sonidos muy chulos. ¡Con una `res` tan alta, escuchas el filtro cutoff resonando tanto que empieza a crear sonidos él mismo!

Por último, para un cambio grande en el timbre, modifica el valor de la opción `wave` a `1`. Esta opción selecciona un oscilador inicial. El valor por defecto es `0`, que es una onda de sierra. `1` es una onda de pulso y `2` es una onda triangular.

Por supuesto, prueba con distintos riffs cambiando las notas en el anillo o incluso cogiendo notas de escalas o acordes. Que te diviertas con tu primer sintetizador de acid bass.

## Desmontando el TB-303

El diseño del TB-303 original es muy sencillo en realidad. Como puedes ver en el siguiente diagrama, sólo tiene 4 partes básicas.

![TB-303 Design](../../../etc/doc/images/tutorial/articles/A.05-acid-bass/tb303-design.png)

En primer lugar tenemos la onda de oscilador - el ingrediente crudo del sonido. En este caso tenemos una onda cuadrada. Después tenemos el envolvente (envelope) de amplitud del oscilador, que controla la amplificación (amp) de la onda cuadrada con el paso del tiempo. Estas opciones se acceden en Sonic Pi con los opts `attack:`, `decay:`, `sustain:` y `release:`, y lo mismo con sus versiones para el volumen. Para más información, lee la sección 2.4, 'Duración con Envolventes' en el tutorial incluido en Sonic Pi. Después del envolvente pasamos nuestra onda cuadrada por un filtro de paso bajo. Aquí es donde empieza la diversión. ¡El valor de cutoff de este filtro también se controla con su propio envolvente! Esto significa que tenemos un nivel de control alucinante sobre el tumbre del sonido con sólo jugar con estos dos envolventes. Echémosle un vistazo:

```
use_synth :tb303
with_fx :reverb, room: 1 do
  live_loop :space_scanner do
    play :e1, cutoff: 100, release: 7, attack: 1, cutoff_attack: 4, cutoff_release: 4
    sleep 8
  end
end
```
    
Para cada opción estándar de un envolvente, hay una opción `cutoff_` equivalente en el sintetizador `:tb303`. Por tanto, podemos usar la opción `cutoff_attack:` para cambiar el valor de ataque del cutoff. Copia el código de abajo en un búfer vacío y dale a Ejecutar. Escucharás un sonido de lo más curioso, haciendo trinos arriba y abajo. Ahora empieza a tocar cosillas. Prueba a cambiar el tiempo de `cutoff_attack` a `1`, y luego a `0.5`. Ahora prueba con `8`.

Fíjate en que he pasado todo por un efecto `:reverb` para darle un poco más de ambiente - ¡prueba con otros efectos para ver cuál funciona mejor!

## Juntándolo todo

Finalmente, aquí tienes una pieza que he compuesto usando ideas de este tutorial. Cópiala en un búfer vacío, escúchala un rato y empieza a a hacer live coding con tus propios cambios. ¡A ver qué sonidos locos le puedes sacar! Hasta la próxima...

```
use_synth :tb303
use_debug false
 
with_fx :reverb, room: 0.8 do
  live_loop :space_scanner do
    with_fx :slicer, phase: 0.25, amp: 1.5 do
      co = (line 70, 130, steps: 8).tick
      play :e1, cutoff: co, release: 7, attack: 1, cutoff_attack: 4, cutoff_release: 4
      sleep 8
    end
  end
 
  live_loop :squelch do
    use_random_seed 3000
    16.times do
      n = (ring :e1, :e2, :e3).tick
      play n, release: 0.125, cutoff: rrand(70, 130), res: 0.9, wave: 1, amp: 0.8
      sleep 0.125
    end
  end
end
```
