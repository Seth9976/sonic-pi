A.8 Conviértete en un VJ de Minecraft

# Conviértete en un VJ de Minecraft

![Pantalla 0](../../../etc/doc/images/tutorial/articles/A.08-minecraft-vj/minecraft-vj-0-small.png)


Minecraft Pi


Todo el mundo ha jugado a Minecraft. Seguro que tod@s vosotr@s habéis construido estructuras increíbles, habéis diseñado trampas ingeniosas, e incluso habéis creado complejas líneas de carros con interruptores de roca roja. ¿Cuánt@s habéis hecho una actuación con Minecraft? Apuesto a que no sabíais que se puede usar Minecraft para crear efectos visuales alucinantes, como un/a VJ profesional.

Si tu única forma de cambiar Minecraft era con el ratón, te habrá costado cambiar cosas rápidamente. Por suerte, Raspberry Pi contiene una versión de Minecraft que se puede controlar con código. También tiene una app llamada Sonic Pi que no sólo hace fácil programar Minecraft, sino que lo hace increíblemente divertido.

En el artículo de hoy, vamos a enseñarte algunos consejos y trucos que hemos usado para dar conciertos en discotecas y escenarios por todo el mundo.

Empecemos...

## Manos a la Obra

Vamos a empezar con un ejercicio fácil de calentamiento, para refrescar los conceptos básicos. Primero, enciende tu Raspberry Pi e inicia Minecraft y Sonic Pi. En Minecraft, crea un nuevo mundo, y en Sonic Pi, escoge un búfer vacío y escribe este código:

```
mc_message "Comencemos..."
```
    
Dale al botón de Ejecutar y verás el mensaje en la ventana de Minecraft. Vale, ya estamos en marcha, ahora vamos a divertirnos...

## Tormentas de Arena

Cuando usamos Minecraft para crear efectos visuales, intentamos pensar en algo que sea interesante y que al mismo tiempo sea fácil de generar con código. Un truco muy chulo es crear una tormenta de arena, dejando caer bloques de arena del cielo. Para ello necesitamos algunas funciones básicas:

* `sleep` - para insertar una pausa entre acciones
* `mc_location` - para conocer nuestra posición actual
* `mc_set_block` - para colocar bloques de arena en un sitio en concreto
* `rrand` - para generar valores aleatorios dentro de un rango
* `live_loop` - para hacer que llueva arena constantemente

Si no estás familiarizad@ con funciones como `rrand`, sólo tienes que escribir la palabra en tu búfer, hacer click en ella, y teclear el atajo de teclado `Control-i` para mostrar la documentación incluida en Sonic Pi. También puedes navegar por la pestaña *lang* en el sistema de Ayuda y mirar las funciones directamente, entre otras cosas alucinantes que puedes hacer.

Vamos a hacer que llueva un poco antes de desatar completamente el poder de la tormenta. Mira tu posición actual y úsala para crear unos cuantos bloques de arena en el cielo, cerca de ti:

```
x, y, z = mc_location
mc_set_block :sand, x, y + 20, z + 5
sleep 2
mc_set_block :sand, x, y + 20, z + 6
sleep 2
mc_set_block :sand, x, y + 20, z + 7
sleep 2
mc_set_block :sand, x, y + 20, z + 8
```
    
Cuando le des a Ejecutar, puedes mirar a tu alrededor, ya que los bloques pueden empezar a caer detrás tuya, dependiendo de a dónde estés mirando ahora mismo. No te preocupes; si no los has llegado a ver, sólo tienes que darle a Ejecutar de nuevo para tener otra ronda de lluvia de arena - ¡asegúrate de que miras en la dirección adecuada!

Vamos a hacer un repaso rápido lo que está pasando. En la primera línea hemos usado la posición de Steve en coordenadas con la función `mc_location`, y las hemos puesto en las variables `x`, `y` y `z`. En las siguientes líneas hemos usado la función `mc_set_block` para colocar unos cuantos bloques de arena en las mismas coordenadas que Steve, pero con algunas modificaciones. Escogimos la misma coordenada x, una coordenada y 20 bloques más alta, y coordenadas z cada vez más grandes, para que la arena caiga en una línea que se aleja de Steve.

¿Por qué no coges el código y empiezas a trastear tú mism@? Prueba a añadir más líneas, cambia las pausas, mezcla `:sand` con `:gravel` (gravilla), y escoge distintas coordenadas. ¡Experimenta y diviértete!

## Live Loops Desatados

Vale, es hora de tener la ira de la tormenta... Para ello vamos a desatar todo el poder del `live_loop`, la habilidad mágica de Soni Pi que desencadena el verdadero poder del live coding - ¡cambiar el código al vuelo mientras se ejecuta!

```
live_loop :sand_storm do
  x, y, z = mc_location
  xd = rrand(-10, 10)
  zd = rrand(-10, 10)
  co = rrand(70, 130)
  synth :cnoise, attack: 0, release: 0.125, cutoff: co
  mc_set_block :sand, x + xd, y+20, z+zd
  sleep 0.125
end
```
    
¡A que mola! Estamos dando vueltas muy rápido (8 veces por segundo), y en cada segundo estamos encontrando la posición de Steve igual que antes, pero después generamos 3 valores aleatorios:

* `xd` - la diferencia de x, entre -10 y 10
* `zd` - la diferencia de z, también entre -10 y 10
* `co` - un valor de cutoff para el filtro de paso bajo, entre 70 y 130

Después usamos esos valores aleatorios en las funciones `synth` y `mc_set_block`, con lo que tenemos arena cayendo en posiciones aleatorias alrededor de Steve, al mismo tiempo que se oye un sonido percusivo de lluvia, que viene del sintetizador `:cnoise`.

Para aquell@s que no conozcáis los live loops, son la parte donde empieza la diversión en Sonic Pi. Mientras el código se ejecuta y diluvia la arena, prueba a cambiar uno de los valores, tal vez el tiempo de pausa (sleep) a `0.25`, o el tipo de bloque de `:sand` a `:gravel` (gravilla). Ahora dale a Ejecutar *de nuevo*. ¡Abracadabra! Las cosas han cambiado sin tener que parar el código. Esto te abre las puertas a actuar como un/a verdader@ VJ. Sigue practicando y cambiando cosas. ¿De cuántas formas puedes cambiar los efectos visuales sin parar el código?

## Patrones de Bloques Épicos

![Screen 1](../../../etc/doc/images/tutorial/articles/A.08-minecraft-vj/minecraft-vj-1-small.png)

Finalmente, otra forma excelente de generar efectos visuales interesantes es crear paredes con patrones, y volar alrededor para verlas de cerca. Para este efecto, necesitamos pasar de colocar los bloques aleatoriamente a colocarlos de forma ordenada. Podemos hacer esto anidando dos iteraciones (dale al botón de Ejecutar y navega por la sección 5.2 del tutorial "Iteración y Bucles" para tener más información sobre las iteraciones). El divertido `|xd|` después del `do` significa que `xd` va a cambiar en cada iteración. La primera vez será 0, luego 1, luego 2... etc. Al anidar dos bloques de iteración juntos como hemos hecho, podemos generar las coordenadas de un cuadrado. Después podemos escoger tipos de bloques aleatoriamente, con un anillo de bloques, para obtener un efecto interesante:

```
x, y, z = mc_location
bs = (ring :gold, :diamond, :glass)
10.times do |xd|
  10.times do |yd|
    mc_set_block bs.choose, x + xd, y + yd, z
  end
end
```

¡Genial! Mientras nos lo pasamos pipa por aquí, prueba a cambiar `bs.choose` a `bs.tick` para pasar de un patrón aleatorio a uno más regular. Prueba a cambiar los tipos de bloque. Para l@s más atrevid@s, podéis probar a meter este cambio en un `live_loop`, para que los patrones sigan cambiando automáticamente.

Y para el final apoteósico - cambia los dos `10.times` a `100.times` y dale a Ejecutar. ¡Kaboom! Una pared ENORME con bloques puestos aleatoriamente. ¡Imagina cuánto tiempo te habría llevado construirlo a mano con el ratón! Pulsa la tecla Espacio dos veces para activar el modo vuelo, y baja en picado para ver efectos visuales alucinantes. No pares todavía - usa tu imaginación para darle rienda suelta a tus ideas, y usa el poder de Sonic Pi para hacerlas realidad. Cuando hayas practicado, apaga las luces y ¡muéstrale tu show de VJ a tus amig@s!
