A.6 Minecraft Musical

# Minecraft Musical


Minecraft Pi


¡Hola y bienvenid@ de nuevo! En tutoriales anteriores, nos hemos centrado sólamente en las posibilidades musicales de Sonic Pi - (convirtiendo tu Raspberry Pi en un instrumento musical preparado para tu concierto). Hasta ahora hemos aprendido cómo:

* Hacer Live Coding - cambiar sonidos al vuelo,
* Programar unos beats muy salvajes,
* Generar sintetizadores lead potentes,
* Re-crear el famoso sonido acid bass del TB-303.

Hay mucho más por ver (que exploraremos en futuras ediciones). Sin embargo, este mes vamos a centrarnos en algo que Sonic Pi hace y que de lo que probablemente no te has dado cuenta: controlar Minecraft.

## Hola Mundo Minecraft

Vale, allá vamos. Enciende tu Raspberry Pi, abre Minecraft Pi y crea un nuevo mundo. Ahora inicia Sonic Pi, y mueve y cambia el tamaño de ambas ventanas, para que puedas ver Sonic Pi y Minecraft Pi al mismo tiempo.

Escribe lo siguiente en un búfer vacío:

```
mc_message "Hola Minecraft desde Sonic Pi!"
```
    
Ahora, dale a Ejecutar. ¡Boom! ¡Tu mensaje apareció en Minecraft! ¿A que ha sido fácil? Ahora, deja de leer esto por un momento y prueba a poner tus propios mensajes. ¡Que te diviertas!

![Pantalla 0](../../../etc/doc/images/tutorial/articles/A.06-minecraft/Musical-Minecraft-0-small.png)

## Teletransportador Sonoro

Ahora vamos a explorar el mundo. La opción estándar es coger el teclado y el ratón y empezar a moverte alrededor. Esto funciona, pero es bastante lento y aburrido. Sería mucho mejor si tuviésemos una especie de máquina de teletransporte. Bueno, tenemos una, gracias a Sonic Pi. Prueba esto:

```
mc_teleport 80, 40, 100
```
    
¡Vaya! Eso ha sido una subida. Si no estuvieses en modo vuelo habrías caído toda esa distancia hasta el suelo. Si pulsas dos veces la tecla espacio para entrar en el modo vuelo y teletransportarte de nuevo, te quedarás flotando en el lugar al que has volado a toda pastilla.

¿Y qué significan esos números? Tenemos tres números que indican las coordenadas del lugar del mundo donde te encuentras. Le damos un nombre a cada número: x, y, z:

* x - cómo de lejos estás a la derecha o a la izquierda (80 en nuestro ejemplo)
* y - cómo de alto queremos estar (40 en nuestro ejemplo)
* z - cómo de lejos hacia delante o hacia atrás (100 en nuestro ejemplo)

Al escoger distintos valores de x, y y z, podemos teletransportarnos *a cualquier lugar* en nuestro mundo. ¡Pruébalo! Escoge distintos números y mira dónde puedes acabar. Si la pantalla se vuelve negra es porque te has teletransportado debajo del suelo o dentro de una montaña. Sencillamente pon un valor más alto de y, para volver a estar por encima de la tierra. Sigue explorando hasta que encuentres algo que te guste...

Usando estas ideas, vamos a construir un Teletransportador Sonoro que haga un sonido de teletransporte divertido mientras nos hace pasar zumbando por el mundo de Minecraft:

```
mc_message "Preparing to teleport...."
sample :ambi_lunar_land, rate: -1
sleep 1
mc_message "3"
sleep 1
mc_message "2"
sleep 1
mc_message "1"
sleep 1
mc_teleport 90, 20, 10
mc_message "Whoooosh!"
```
    
![Pantalla 1](../../../etc/doc/images/tutorial/articles/A.06-minecraft/Musical-Minecraft-1-small.png)

## Bloques Mágicos

Ahora que has encontrado un sitio que te guste, ¡empecemos a construir!. Podrías hacer lo que sueles hacer y empezar a hacer click con el ratón una y otra vez (furiosamente) para poner bloques debajo del cursor. O podrías usar la magia de Sonic Pi. Prueba esto:

```
x, y, z = mc_location
mc_set_block :melon, x, y + 5, z
```

¿Lo ves? ¡Hay un melón en el cielo! Tómate un momento para mirar el código. ¿Qué hemos hecho? En la línea uno, hemos cogido nuestra localización actual de Steve con las variables x, y, z. Éstas corresponden a nuestras coordenadas descritas anteriormente. Usamos esas coordenadas en la función `mc_set_block`, que deja el bloque que queramos en las coordenadas especificadas. Para hacer algo más alto en el cielo, sólo tenemos que subir el valor de y, que es el motivo por el cual le hemos añadido 5. Vamos a hacer una tira de melones:

```
live_loop :melon_trail do
  x, y, z = mc_location
  mc_set_block :melon, x, y-1, z
  sleep 0.125
end
```

Ahora, vuelve a Minecraft, asegúrate de que estás en modo vuelo (si no, pulsa la tecla espacio dos veces), y vuelva por el mundo. Mira detrás tuya para ver... ¡una preciosa tira de bloques de melón! A ver qué patrones puedes hacer en el cielo.

## Haciendo Live Coding con Minecraft

Aquell@s que hayáis seguido este tutorial durante los últimos meses estaréis flipando en colores. La tira de melones está muy chula, pero la parte más increíble de este ejemplo es que ¡podéis usar `live_loop` con Minecraft! Para quienes no lo sepáis, `live_loop` es una habilidad mágica de Sonic Pi que no tiene ningún otro lenguaje de programación. Te permite ejecutar varios bucles al mismo tiempo y cambiarlos mientras tanto. Son increíblemente potentes y divertidos. Yo uso los `live_loop`s para dar conciertos de música en discotecas con Sonic Pi - los DJs usan discos y yo uso `live_loop`s :-) Sin embargo, hoy vamos a hacer live coding con música y con Minecraft.

Empecemos. Ejecuta el código de arriba y empieza a hacer tu propia tira de melones de nuevo. Ahora, sin parar el código, cambia `:melon` por `:brick` y dale a Ejecutar. ¡Abracadabra!, estás haciendo una tira de ladrillos. ¡A que es fácil! ¿Y si le ponemos música chula de fondo? Pan comido. Prueba esto:

```
live_loop :bass_trail do
  tick
  x, y, z = mc_location
  b = (ring :melon, :brick, :glass).look
  mc_set_block b, x, y -1, z
  note = (ring :e1, :e2, :e3).look
  use_synth :tb303
  play note, release: 0.1, cutoff: 70
  sleep 0.125
end
```
    
Ahora, mientras suena, empieza a cambiar el código. Cambia los tipos de bloques - prueba `:water`, `:grass` o tu tipo de bloque favorito. Prueba también a cambiar el valor de cutoff de `70` a `80` y así hasta `100`. ¿Divertido, eh?

## Juntándolo todo

![Pantalla 2](../../../etc/doc/images/tutorial/articles/A.06-minecraft/Musical-Minecraft-2-small.png)

Vamos a combinar todo lo que hemos visto hasta ahora con un poco de magia extra. Vamos a combinar nuestra habilidad de teletransporte con moviminto de bloques y música, para crear un Videoclip de Minecraft. No te preocupes si no lo entiendes todo, tú dale y prueba a cambiar algunos valores mientras se ejecuta. Pásatelo bien y hasta la próxima...
    
```
live_loop :note_blocks do
  mc_message "This is Sonic Minecraft"
  with_fx :reverb do
    with_fx :echo, phase: 0.125, reps: 32 do
      tick
      x = (range 30, 90, step: 0.1).look
      y = 20
      z = -10
      mc_teleport x, y, z
      ns = (scale :e3, :minor_pentatonic)
      n = ns.shuffle.choose
      bs = (knit :glass, 3, :sand, 1)
      b = bs.look
      synth :beep, note: n, release: 0.1
      mc_set_block b, x+20, n-60+y, z+10
      mc_set_block b, x+20, n-60+y, z-10
      sleep 0.25
    end
  end
end
live_loop :beats do
  sample :bd_haus, cutoff: 100
  sleep 0.5
end
```
