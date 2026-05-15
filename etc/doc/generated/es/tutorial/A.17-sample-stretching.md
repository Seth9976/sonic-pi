A.12 Cortando Samples

# Cortando Samples

Cuando la gente descubre Sonic Pi, una de las primeras cosas que aprenden es lo fácil que resulta tocar sonidos pre-grabados usando la función `sample`. Por ejemplo, puedes tocar un loop de batería con sonido de tecno industrial, escuchar el sonido de un coro, o incluso oír un tocadiscos... todo con una sola línea de código. Sin embargo, mucha gente no se da cuenta de que puedes cambiar la velocidad del sample que estás tocando, consiguiendo efectos muy chulos y un logrando un nuevo nivel de control de tus sonidos grabados. Ahora, abre Sonic Pi y, ¡vamos a estirar samples!

## Sampleos

Para cambiar la velocidad a la que suena un sample (playback rate), usamos la opción `rate:`:

```
sample :guit_em9, rate: 0.5
```
    
Si escribimos un `rate:` de `1`, el sample suena a la velocidad normal. Si queremos tocarlo a la mitad de velocidad, sólo tenemos que usar un `rate:` de `0.5`:


```
sample :guit_em9, rate: 0.5
```
    
Observa que esto tiene dos efectos en el audio. En primer lugar, la muestra suena con un tono más bajo y, en segundo lugar, tarda el doble de tiempo en reproducirse (véase la barra lateral para una explicación de por qué esto es así). Incluso podemos elegir tasas cada vez más bajas acercándonos a `0`, de modo que una `rate:` de `0.25` es un cuarto de velocidad, `0.1` es un décimo de la velocidad, etc. Prueba a jugar con algunas tasas bajas y comprueba si puedes convertir el sonido en un estruendo bajo.

## Acelerando las muestras

Además de hacer el sonido más largo y más grave usando una velocidad baja, podemos usar velocidades altas para hacer el sonido más corto y más agudo. Ahora vamos a tocar con un loop de batería. Primero, escucha cómo suena con la velocidad por defecto de `1`:

```
sample :loop_amen, rate: 1
```


Ahora, vamos a acelerarlo un poco:

```
sample :loop_amen, rate: 1.5
```
    
¡Ja! Acabamos de pasar de tecno de la vieja escuela al jungle. Fíjate en cómo aumenta la altura de cada nota, y también en cómo se acelera el ritmo. Ahora, prueba con velocidades todavía más altas, y averigua cómo de alto y de corto puedes hacer el loop de batería. Por ejemplo, si usas una velocidad de `100`, ¡el loop se convierte en un click!

## Al Revés

Seguro que much@s estáis pensando en lo mismo ahora... "¿y qué pasa si usas un número negativo para la velocidad? ¡Muy buena pregunta! Vamos a pensar un momento. Si nuestra opt `rate:` representa la velocidad a la que se reproduce nuestro sample, `1` es velocidad normal, `2` es el doble de velocidad, y `0.5` es la mitad de velocidad, entonces, ¡con `-1` debe sonar al revés! Vamos a probar con una caja de batería. Primero, toca el sample a la velocidad normal:

```
sample :elec_filt_snare, rate: 1
```
    
Ahora, ejecutado al revés:

```
sample :elec_filt_snare, rate: -1
```
    
Por supuesto, puedes tocarlo al revés y al doble de velocidad con una velocidad de `-2`, o puedes tocarlo al revés a la mitad de velocidad con un `rate:` de `-0.5`. Prueba con distintos valores negativos y diviértete. ¡Suena especialmente curioso con el sample `:misc_burp`!


## Sample, Velocidad y Altura [Barra lateral]

Uno de los efectos de modificar la velocidad en los samples es que cuanta mayor es la velocidad, más agudo suena el sample en altura; a menor velocidad, más grave suena. Puede que hayas escuchado este efecto en tu día a día, cuando vas en bicicleta o en coche y pasas un paso de cebra con pitido; a medida que te acercas a la fuente del sonido, la altura es mayor que cuando estabas más lejos del paso de cebra. Esto se conoce como efecto Doppler. ¿Por qué ocurre?

Veamos un pitido sencillo (beep) representado con una onda de seno. Si usamos un osciloscopio para generar una gráfica del beep, veremos algo parecido a la Figura A. Si dibujamos un beep una octava más agudo, veremos la Figura B, y con una octava más se vería como en la Figura C. Fíjate en que las ondas de notas más agudas son más compactas, y que las notas más graves están más separadas.

El sample de un beep sólo es un montón de números (coordenadas x,y) que dibujan las curvas originales del sonido que se grabó. Mira la Figura D, donde cada círculo representa una coordenada. Para convertir las coordenadas a audio, el ordenador coge cada valor x y le envía su correspondiente valor y a los altavoces. El truco está en que el ordenador no tiene por qué procesar los valores de x y guardarlos al mismo tiempo. O sea, el hueco (la cantidad de tiempo) entre cada círculo se puede estirar o comprimir. Si el ordenador lee los valores de x más rápido que como sonaban en la vida real, los círculos se harán más pequeños y el beep sonará más agudo. También hará el beep más corto, porque pasamos por los círculos más rápido. Puedes ver este efecto en la Figura E.

Por último, tal vez te interese saber que un matemático llamado Fourier demostró que cualquier sonido es en realidad muchas ondas de seno juntas. Por eso, cuando comprimimos y estiramos cualquier sonido grabado, lo que estamos haciendo en realidad es comprimir y estirar muchas ondas de seno al mismo tiempo.

## Pitch Bending

Hemos visto que con una velocidad (rate) más alta, el sonido suena más agudo; y que con una velocidad más baja, el sonido suena más grave. Hay un truco muy fácil y útil que tiene que ver con esto. Cuando duplicas la velocidad, el sonido suena una octava más alto; y al revés, si divides la velocidad por dos, el sonido suena una octava más bajo. Esto quiere decir que si duplicas / divides por dos la velocidad de un sample melódico, va a seguir sonando bastante bien:

```
sample :bass_trance_c, rate: 1
sample :bass_trance_c, rate: 2
sample :bass_trance_c, rate: 0.5
```
    
Pero, ¿y si queremos cambiar el rate para que el sample sólo suene un semitono (una tecla más arriba en el piano) más agudo, en vez de una octava entera? Podemos hacerlo fácilmente con Sonic Pi usando la opción `rpich:`:

```
sample :bass_trance_c
sample :bass_trance_c, rpitch: 3
sample :bass_trance_c, rpitch: 7
```
    
Si miras los mensajes del registro (log) a la derecha, verás que un `rpitch:` de `3` se corresponde con un rate de `1.1892`, y un `rpitch` de `7` se corresponde con un rate de `1.4983`. Por último, podemos combinar los opts `rate:` y `rpitch:`:

```
sample :ambi_choir, rate: 0.25, rpitch: 3
sleep 3
sample :ambi_choir, rate: 0.25, rpitch: 5
sleep 2
sample :ambi_choir, rate: 0.25, rpitch: 6
sleep 1
sample :ambi_choir, rate: 0.25, rpitch: 1
```
    

## Juntándolo todo

Vamos a echarle un vistazo a una pieza sencilla que combina estas ideas. Cópiala en un búfer vacío de Sonic Pi, dale a Ejecutar, escúchala un rato y úsala para empezar tu propia pieza. ¿Has visto qué divertido es jugar con el rate de los samples? Y aquí tienes un ejercicio extra: prueba a grabar tus propios sonidos y a cambiar la velocidad de reproducción para ver qué sonidos locos puedes hacer.

```
live_loop :beats do
  sample :guit_em9, rate: [0.25, 0.5, -1].choose, amp: 2
  sample :loop_garzul, rate: [0.5, 1].choose
  sleep 8
end
 
live_loop :melody do
  oct = [-1, 1, 2].choose * 12
  with_fx :reverb, amp: 2 do
    16.times do
      n = (scale 0, :minor_pentatonic).choose
      sample :bass_voxy_hit_c, rpitch: n + 4 + oct
      sleep 0.125
    end
  end
end
```







