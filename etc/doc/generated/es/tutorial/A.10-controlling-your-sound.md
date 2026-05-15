A.10 Control

# Controlando tu sonido

Hasta ahora en esta serie, nos hemos centrado en usar sonidos disparándolos. Hemos descubierto que podemos disparar los distintos sintetizadores de Sonic Pi con `play` o `synth`, y hemos aprendido cómo lanzar samples pregrabados con `sample`. También hemos visto cómo podemos pasar estos sonidos por efectos de estudio, como reverberación y distorsión, usando el comando `with_fx`. Combina esto con el sistema de tiempo increíblemente preciso de Sonic Pi, y puedes producir una cantidad enorme de sonidos, beats y riffs. Sin embargo, una vez has seleccionado las opciones de un sonido y lo has disparado, no hay forma de cambiar dichas opciones mientras suena, ¿verdad? ¡Pues no! Hoy vas a aprender algo muy potente: cómo controlar sintetizadores en ejecución.

## Un sonido básico

Hagamos un sonido simple. Arranca Sonic Pi y en un buffer libre escribe lo siguiente:

```
synth :prophet, note: :e1, release: 8, cutoff: 100
```

Ahora pulsa el botón de Ejecutar (arriba a la izquierda) para escuchar un adorable y estruendoso sintetizador. No pares, ejecútalo varias veces para que tengas una primera impresión. Vale, ¿lo tienes? Entonces, ¡vamos a empezar a controlarlo!

## Synth Nodes

Una funcionalidad poco conocida de Sonic Pi es que las funciones `play`, `synth` y `sample` devuelven algo llamado `SynthNode` (nodo de sintetizador), que representa un sonido en ejecución. Puedes capturar un `SynthNode` usando una variable normal y corriente, y luego **controlarlo** más tarde. Por ejemplo, vamos a cambiar la opción `cutoff:` después de 1 beat:

```
sn = synth :prophet, note: :e1, release: 8, cutoff: 100
sleep 1
control sn, cutoff: 130
```

Veamos cada línea una por una:

En primer lugar tenemos el sintetizador `:prophet` que usa la función `synth` como de costumbre. Sin embargo, hemos capturado el resultado en una variable llamada `sn`. Podríamos haber llamado a esta variable de cualquier forma, como `synth_node` o `jane` - el nombre no importa. Lo que sí es importante es escoger un nombre que tenga sentido para ti cuando des tu concierto, o para otras personas que quieran leer tu código. Yo he usado `sn` porque es una abreviatura de synth node.

En la línea 2 hemos usado un comando `sleep` normal y corriente. Esto no hace nada especial; sencillamente, le pide al ordenador que espere 1 beat antes de moverse a la siguiente línea.

En la línea 3 empieza la diversión con el tema del control. Aquí hemos usado la función `control` para decirle a nuestro `SynthNode` en ejecución que cambie su valor de cutoff a `130`. Si le das al botón de **Ejecutar**, escucharás el sintetizador que empieza a tocar como antes, pero después de 1 beat verás que cambia a un sonido mucho más brillante.

Opciones Modulables

La mayoría de las opciones para sintetizadores y efectos de Sonic Pi se pueden cambiar después de haber sido disparadas. Sin embargo, esto no pasa con todas. Por ejemplo, las opciones de envolvente en `attack:`, `decay:`, `sustain:` y `release:` sólo se pueden controlar cuando disparas el sintetizador. Es fácil saber qué opts puedes cambiar y cuáles no puedes cambiar; busca el sintetizador o efecto que quieras en la documentación, baja hasta la documentación de las opts, y busca las frases "Puede ser cambiado mientras se toca" o "No se puede cambiar una vez iniciado". Por ejemplo, la documentación para la opción `attack:` del sintetizador `:beep` nos deja claro que no se puede cambiar esa opción después de disparar el sintetizador:

* Default: 0
* Debe ser cero o mayor
* No se puede cambiar una vez disparado
* Escalado con el valor actual de BPM

## Cambios múltiples

Mientras un sintetizador está en ejecución, no estás limitado a cambiarlo sólo una vez: puedes cambiarlo tantas veces como quieras. Por ejemplo, podemos convertir nuestro `:prophet` en un mini arpegiador con el siguiente código:

```
notes = (scale :e3, :minor_pentatonic)
sn = synth :prophet, note: :e1, release: 8, cutoff: 100
sleep 1
16.times do
  control sn, note: notes.tick
  sleep 0.125
end
```

Hemos añadido unas cosillas extra en este fragmento de código. Primero, hemos definido una nueva variable llamada `notes` que contiene las notas por las que nos gustaría pasar (por cierto, arpegiador sólo es un palabrejo para algo que da vueltas sobre una lista de notas en orden). Segundo, hemos reemplazado nuestra llamada a `control` por una iteración que la llama 16 veces. En cada llamada a `control`, hacemos `.tick` sobre nuestro anillo de notas (`notes`), que se repite automáticamente cuando llegamos al final (gracias al fabuloso poder de los anillos de Sonic Pi). Para tener algo más de variedad, prueba a reemplazar `.tick` por `.choose`, a ver si puedes escuchar la diferencia.

Fíjate en que podemos cambiar varios opts a la vez. Prueba a cambiar la línea de control por la siguiente, y escucha la diferencia:

```
control sn, note: notes.tick, cutoff: rrand(70, 130)
```

## Sliding

Cuando controlamos un `SynthNode`, éste responde exactamente a tiempo y cambia al instante el valor del opt por uno nuevo, como si hubieras presionado un botón o hubieras accionado un interruptor. Esto puede sonar rítmico y percusivo - especialmente si el opt controla un aspecto del timbre, como `cutoff:`. Sin embargo, a veces no quieres que el cambio sea instantáneo. Tal vez quieras moverte suavemente del valor actual al nuevo, como si estuvieras moviendo un deslizador o un dial. Por supuesto, Sonic Pi también puede hacer esto, usando los opts `_slide:`.

Cada opción que puede ser modificada también tiene una opción `_slide:` correspondiente, que permite especificar el tiempo de deslizamiento (slide). Por ejemplo, `amp:` tiene `amp_slide:`, y `cutoff:` tiene `cutoff_slide:`. Estas slide opts son muy distintas al resto de opts, ya que le dicen a la nota del sintetizador cómo comportarse **la siguiente vez que sean controladas**. Vamos a echarle un vistazo:

```
sn = synth :prophet, note: :e1, release: 8, cutoff: 70, cutoff_slide: 2
sleep 1
control sn, cutoff: 130
```

Fíjate en que este ejemplo es exactamente igual que el de antes, excepto por el `cutoff_slide:`. Esto le dice al sintetizador que, para la próxima vez que controle su opción `cutoff:`, tiene que usar 2 beats para deslizarse desde el valor actual hasta el nuevo valor. Por eso puedes escuchar el cutoff moverse de 70 a 130 cuando usas `control`. Esto le da al sonido un toque dinámico muy interesante. Ahoa prueba a cambiar el tiempo de `cutoff_slide:` a un valor más corto, como 0.5, o un valor más largo, como 4, para ver cómo cambia el sonido. Recuerda, puedes hacer slide entre cualquiera de las opciones modificables de esta forma, y cada valor de `_slide:` puede ser totalmente distinto. Puedes hacer que el cutoff cambie lentamente, que el amp cambie rápido y que el pan cambie a un ritmo intermedio; configúralo a tu gusto, como estés pensando para tu nueva creación...

## Juntándolo todo

Veamos un breve ejemplo que demuestra el poder de controlar sintetizadores después de haberlos disparado. Fíjate en que también puedes hacer slide con los efectos igual que con los sintetizadores, aunque con una sintaxis ligeramente distinta. Mira la sección 7.2 del tutorial incluido en Sonic Pi para más información sobre el control de los FX (efectos).

Copia el código en un búfer separado y escúchalo unos instantes. No dejes que se acabe ahí: juega con el código. Cambia los tiempos de slide, cambia las notas, el sintetizador, los FX y los tiempos de pausa, ¡a ver si puedes convertirlo en algo totalmente distinto!

```
live_loop :moon_rise do
  with_fx :echo, mix: 0, mix_slide: 8 do |fx|
    control fx, mix: 1
    notes = (scale :e3, :minor_pentatonic, num_octaves: 2).shuffle
    sn = synth :prophet , sustain: 8, note: :e1, cutoff: 70, cutoff_slide: 8
    control sn, cutoff: 130
    sleep 2
    32.times do
      control sn, note: notes.tick, pan: rrand(-1, 1)
      sleep 0.125
    end
  end
end
```
