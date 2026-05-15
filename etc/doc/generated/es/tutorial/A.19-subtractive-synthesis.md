A.19 Diseño Sonoro - Síntesis Sustractiva

# Síntesis Sustractiva

Este ese el segundo artículo de la serie, en la que veremos cómo usar Sonic Pi para hacer diseño sonoro. El último mes vimos la síntesis aditiva, y descubrimos que no es más que juntar muchos sonidos a la vez para crear un nuevo sonido combinado. Por ejemplo, podíamos combinar distintos sintetizadores, o incluso el mismo sintetizador con distinta altura, para crear un sonido muy complejo a partir de ingredientes sencillos. Este mes vamos a ver una nueva técnica conocida como *síntesis substractiva*, que es sencillamente coger un sonido complejo y quitarle partes para crear algo nuevo. Esta técnica está asociada al sonido de los sintetizadores analógicos de los 60 y los 70; también tuvo que ver con el renacimiento de los sintetizadores analógicos modulares, con estándares muy populares como el Eurorack.

Aunque parezca una técnica complicada o avanzada, Sonic Pi lo hace increíblemente fácil y sencillo... así que vamos a ello.

## Señal Fuente Compleja

Para que un sonido quede bien con síntesis sustractiva, debe ser bastante rico e interesante. Esto no significa que necesitemos algo enormemente complejo; de hecho, nos vale con una onda cuadrada (`:square`) o de sierra (`:saw`):

```
synth :saw, note: :e2, release: 4
```

Note que este sonido ya es más interesante y contiene muchas frecuencias diferentes `:e2`(el segundo Mi del piano) que se añade para crear el timbre. Si esto no tiene mucho sentido para usted, intente compararlo con `:beep`:

```
synth :beep, note: :e2, release: 4
```

Como el sintetizador `:beep` es sólo una onda de seno, escucharás un tono mucho más puro, con la nota `:e2` muy clara, y ningún otro ruido que la emborrone (lo que sí escucharías con una onda `:saw`). El zumbido y la variación de la onda pura de seno es lo que nos permite jugar cuando usamos síntesis sustractiva.

## Filtros

Ahora que tenemos nuestra señal fuente en bruto, el siguiente paso es pasarla por un filtro de algún tipo, que modifica el sonido quitando (o reduciendo) partes del mismo. Uno de los filtros de síntesis sustractiva más conocidos es el filtro de paso bajo (low pass filter). Este filtro deja pasar las partes graves del sonido, pero reduce o borra las partes agudas. Sonic Pi tiene un sistema de efectos sencillo y potente, que incluye un filtro de paso bajo llamado `:lpf` (de sus siglas en inglés, low pass filter). Vamos a probarlo:

```
with_fx :lpf, cutoff: 100 do 
  synth :saw, note: :e2, release: 4 
end
```

Si te fijas escucharás cómo se ha quitado parte del zumbido del sonido. De hecho, todas las frecuencias del sonido por encima de la nota `100` han sido reducidas o borradas; sólo se han dejado las frecuencias agudas. Prueba a cambiar el valor del `cutoff:` a notas más bajas, por ejemplo `70` y luego `50`; compara los sonidos que producen.

Por supuesto, `:lpf` no es el único filtro que tienes para manipular la señal fuente. Otro efecto importante es el filtro de paso alto, llamado `:hpf` (por sus siglas en inglés, high pass filter) en Sonic Pi. Este hace lo contrario a `:lpf`, ya que deja pasar los agudos y corta los graves.

```
with_fx :hpf, cutoff: 90 do
  synth :saw, note: :e2, release: 4
end
```

Fíjate en cómo suena mucho más brillante y áspero ahora que has borrado las frecuencias bajas. Experimenta con los valores de cutoff: escucha cómo se dejan pasar cada vez más bajos cuantos más bajos sean los valores del filtro, y cómo los agudos van sonando cada vez más pequeños y callados.

## Low Pass Filter

![Cantidades variables de filtrado de paso bajo](../../../etc/doc/images/tutorial/articles/A.19-subtractive-synthesis/subtractive-synthesis-waveforms.png)

El filtro de paso bajo es una parte tan importante de todo kit de herramientas de síntesis sustractiva que merece la pena profundizar en su funcionamiento. Este diagrama muestra la misma onda de sonido (el sintetizador `:prophet`) con distintas cantidades de filtrado. En la parte superior, la sección A muestra la onda de audio sin filtro. Fíjate en que la forma de la onda es muy puntiaguda y contiene muchos bordes afilados. Son estos ángulos duros y afilados los que producen las partes altas y crujientes del sonido. La sección B muestra el filtro de paso bajo en acción: observe que es menos puntiagudo y más redondeado que la forma de onda anterior. Esto significa que el sonido tendrá menos frecuencias altas, dándole una sensación más suave y redondeada. La sección C muestra el filtro de paso bajo con un valor de corte bastante bajo, lo que significa que se han eliminado aún más frecuencias altas de la señal, lo que da lugar a una forma de onda aún más suave y redondeada. Por último, observe cómo el tamaño de la forma de onda, que representa la amplitud, disminuye a medida que pasamos de A a C. La síntesis sustractiva funciona eliminando partes de la señal, lo que significa que la amplitud general se reduce a medida que aumenta la cantidad de filtrado que se realiza.


## Modulación del filtro

Hasta ahora sólo hemos producido sonidos estáticos. En otras palabras, el sonido no cambia a lo largo de su duración. Normalmente querrás algo de movimiento en tu sonido para darle más vida a su timbre. Una forma de conseguir esto es con la modulación de filtros (cambiar las opciones del filtro a lo largo del tiempo). Por suerte, Sonic Pi te ofrece herramientas muy potentes para modificar las opciones de un efecto a lo largo del tiempo. Por ejemplo, puedes ponerle un tiempo de deslizamiento a la opción del sintetizador que quieras, siempre y cuando ésta permita modulación; esto le dice a la opción cuánto tarda su valor actual en desplazarse hasta un nuevo valor:

```
with_fx :lpf, cutoff: 50 do |fx|
  control fx, cutoff_slide: 3, cutoff: 130
  synth :prophet, note: :e2, sustain: 3.5
end
```

Vamos a echarle un vistazo a lo que está pasando. Primero, empezamos un bloque de efecto `:lpf` normal y corriente, con un valor de `cutoff:` de `20` (muy bajo). Sin embargo, la primera línea acaba con un `|fx|` un poco raro al final. Eso es una parte opcional de la sintaxis `with_fx`, que te permite nombrar y controlar el sintetizador del efecto manualmente. La línea 2 hace justo esto, y controla el efecto para cambiar el opt `cutoff_slide:` a 4, y el nuevo `cutoff:` a `130`. Con esto, el efecto empezará a deslizar el valor del `cutoff:` de `50` a `130` a lo largo de 3 beats. Por último, lanzamos un sintetizador con la señal fuente, para que podamos escuchar el efecto del filtro de paso bajo modulado.


## Juntándolo todo

Esto es sólo una pequeña muestra de lo que puedes hacer con filtros. Juega con los distintos efectos de Sonic Pi para ver qué clase de sonidos puedes diseñar. Si tu sonido te parece demasiado estático, recuerda que puedes empezar modulando las opciones para crear más movimiento.

Terminemos diseñando una función que ejecutará un nuevo sonido creado con síntesis sustractiva. Intente descubrir qué es lo que sucede - y para aquellos usuarios avanzados de Sonic Pi- vean si pueden descubrir por qué empaqueté todo en la llamada a `at` (por favor envíen sus respuestas a @samaaron en Twiter).

```
define :subt_synth do |note, sus|
  at do
    with_fx :lpf, cutoff: 40, amp: 2 do |fx|
      control fx, cutoff_slide: 6, cutoff: 100
      synth :prophet, note: note, sustain: sus
    end
    with_fx :hpf, cutoff_slide: 0.01 do |fx|
      synth :dsaw, note: note + 12, sustain: sus
      (sus * 8).times do
        control fx, cutoff: rrand(70, 110)
        sleep 0.125
      end
    end
  end
end
subt_synth :e1, 8
sleep 8
subt_synth :e1 - 4, 8
```
