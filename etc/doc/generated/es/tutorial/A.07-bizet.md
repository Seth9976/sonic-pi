A.7 Bizet Beats

# Bizet Beats

Tras nuestra pequeña excursión por el mundo fantástico de Minecraft programando con Sonic Pi este último mes, vamos a hacer música de nuevo. Hoy vamos a traer una pieza ópera clásica al siglo 21, usando el poder alucinante de la programación.

## Atroz y Disruptivo

Viajemos atrás en el tiempo, al año 1875. Un compositor llamado Bizet ha acabado su última ópera, Carmen. Por desgracia, como ocurre con las piezas de música más disruptivas, no le gustó a la gente porque era demasiado expererimental y diferente. Tristemente, Bizet murió diez años antes de que la ópera se hiciese conocida internacionalmente y se convirtiese en una de las óperas más famosas y más tocadas de todos los tiempos. En un acto de simpatía con este genio y su tragedia, vamos a coger uno de los temas principales de Carmen y a convertirlo a un formato moderno de música que también es muy atroz y diferente para la mayoría de la gente de nuestra época - ¡la música con live coding!

## Descifrando la Habanera

Intentar hacer live coding con la ópera entera sería todo un desafío para este tutorial, así que vamos a centrarnos en una de las partes más conocidas - la línea de bajo de la Habanera:

![Riff Habanera](../../../etc/doc/images/tutorial/articles/A.07-bizet/habanera.png)

Esto te puede parecer extremadamente ilegible si nunca has estudiado notación musical. No obstante, l@s programador@s vemos la notación musical como otra forma de código - sólo representa instrucciones para un/a músic@ en vez de un ordenador. Por tanto necesitamos encontrar una forma de descifrarlo.

## Notas

Las notas están colocadas de izquierda a derecha como las palabras en esta revista, pero tienen distintas alturas. *La altura en la partitura representa la altura de la nota*. Cuanto más alta sea la nota en la partitura, mayor será la altura de la nota.

En Sonic Pi ya sabemos cómo cambiar la altura de una nota - usamos números más o menos altos como `play 75` y `play 80`, o usamos los nombres de las notas: `play :E` y `play :F`. Por suerte, cada una de las posiciones verticales de la partitura representa el nombre de una nota específica. Échale un vistazo a esta tabla, es bastante útil:

![Notas](../../../etc/doc/images/tutorial/articles/A.07-bizet/notes.png)

## Silencios

Las partituras musicales son un tipo de código extremadamente rico y expresivo capaz de comunicar muchas cosas. Por tanto, no debería sorprenderte que las partituras no sólo te muestran cuándo tocar notas, sino también cuándo *no* tocarlas. En programación, esto es bastante parecido al concepto de `nil` o `null` - la ausencia de valor. En otras palabras, no tocar una nota es como la ausencia de una nota.

Si has mirado de cerca la partitura, verás que en realidad es una combinación de puntos negros, con líneas que representan las notas a tocar, y con cosas con forma de garrapata que representan silencios. Afortunadamente, Sonic Pi tiene una representación muy útil para el silencio: `:r`, así que si ejecutamos: `play :r`, en realidad, ¡está tocando silencio! También podríamos escribir `play :rest`, `play nil` o `play false`, que son formas equivalentes de representar silencios.

## Ritmo

Finalmente, hay una última cosa que debemos aprender para descifrar la notación - los ritmos de las notas. En la notación original puedes ver que las notas están conectadas con líneas gruesas, llamadas barras. La segunda nota tiene dos de estas barras, que significa que dura la dieciseisava parte de un compás (lo que se llama beat en Sonic Pi). Las otras notas tienen una sola barra, que significa que duran la octava parte de un compás. El silencio tiene dos barras con forma de garrapata, que significa que también representa un dieciseisavo de un compás.

Cuando intentamos descifrar y explorar cosas nuevas, un truco muy útil es hacer todo lo más parecido posible, y buscar relaciones o patrones comunes. Por ejemplo, cuando reescribimos nuestra notación sólo con semicorcheas, vemos una secuencia muy sencilla de notas y silencios.

![Riff Habanera 2](../../../etc/doc/images/tutorial/articles/A.07-bizet/habanera2.png)

## Re-programando la Habanera

Ahora podemos empezar a traducir esta línea de bajo en Sonic Pi. Vamos a codificar estas notas y silencios en un anillo:

```
(ring :d, :r, :r, :a, :f5, :r, :a, :r)
```
    
Veamos cómo suena. Ponlo en un live loop y haz tick sobre el mismo:

```
live_loop :habanera do
  play (ring :d, :r, :r, :a, :f5, :r, :a, :r).tick
  sleep 0.25
end
```
    
¡Fabuloso! La famosa melodía cobra vida a través de tus altavoces. Costó un poquitín llegar hasta aquí, pero ha merecido la pena - ¡choca los cinco!
    
## Sintetizadores Enfadados

Ahora que tenemos la línea de bajo, vamos a recrear parte de la atmósfera de esta escena de ópera. Un sintetizador que podríamos probar es `:blade`, que es un sintetizador lead con tono enfadado de los 80s. Vamos a probarlo con la nota inicial `:d`, que vamos a pasar por un slicer y un efecto de reverberación:

```
live_loop :habanera do
  use_synth :fm
  use_transpose -12
  play (ring :d, :r, :r, :a, :f5, :r, :a, :r).tick
  sleep 0.25
end
with_fx :reverb do
  live_loop :space_light do
    with_fx :slicer, phase: 0.25 do
      synth :blade, note: :d, release: 8, cutoff: 100, amp: 2
    end
    sleep 8
  end
end
```

Ahora, prueba con otras notas en la línea de bajo: `:a` y `:f5`. Recuerda, no tienes por qué parar, puedes modificar el código mientras suena la música y darle a Ejecutar de nuevo. Además, puedes probar distintos valores para el opt `phase:` del slicer, como `0.5`, `0.75` y `1`.

## Juntándolo todo

Finalmente, vamos a combinar todas las ideas que hemos visto hasta ahora en un nuevo remix de la Habanera. Habrás visto que he incluido otra parte de la línea de bajo como comentario. Una vez hayas escrito todo en un búfer vacío, dale a Ejecutar para escuchar la composición. Ahora, sin parar el progama, *descomenta* la segunda línea borrando el `#` y dándole a Ejecutar de nuevo - ¿a que suena genial? Ahora, empieza a mezclarlo tú mism@ y diviértete.

```
use_debug false
bizet_bass = (ring :d, :r, :r, :a, :f5, :r, :a, :r)
#bizet_bass = (ring :d, :r, :r, :Bb, :g5, :r, :Bb, :r)
 
with_fx :reverb, room: 1, mix: 0.3 do
  live_loop :bizet do
    with_fx :slicer, phase: 0.125 do
      synth :blade, note: :d4, release: 8,
        cutoff: 100, amp: 1.5
    end
    16.times do
      tick
      play bizet_bass.look, release: 0.1
      play bizet_bass.look - 12, release: 0.3
      sleep 0.125
    end
  end
end
 
live_loop :ind do
  sample :loop_industrial, beat_stretch: 1,
    cutoff: 100, rate: 1
  sleep 1
end
 
live_loop :drums do
  sample :bd_haus, cutoff: 110
  synth :beep, note: 49, attack: 0,
    release: 0.1
  sleep 0.5
end
```
