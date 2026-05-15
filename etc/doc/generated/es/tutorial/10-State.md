10 Estado del Tiempo (Time State)

# Estado del Tiempo (Time State)

A menudo es útil tener información que es *compartida a través de múltiples hilos o bucles en vivo*. Por ejemplo, es posible que quieras compartir una noción de la clave actual, el BPM o incluso conceptos más abstractos como la "complejidad" actual (que potencialmente se interpretaría de diferentes maneras a través de diferentes hilos). Tampoco queremos perder ninguna de nuestras garantías de determinismo existentes al hacer esto. En otras palabras, nos gustaría seguir siendo capaces de compartir el código con otros y saber exactamente lo que van a escuchar cuando lo ejecuten. Al final de la Sección 5.6 de este tutorial discutimos brevemente por qué *no deberíamos usar variables para compartir información entre hilos* debido a la pérdida de determinismo (a su vez debido a las condiciones de carrera).

La solución de Sonic Pi al problema de trabajar fácilmente con variables globales de forma determinista es a través de un novedoso sistema que denomina Time State. Esto puede sonar complejo y difícil (de hecho, en el Reino Unido, programar con múltiples hilos y memoria compartida suele ser una asignatura de nivel universitario). Sin embargo, como verás, al igual que cuando tocas tu primera nota, *Sonic Pi hace que sea increíblemente sencillo compartir el estado entre hilos* sin dejar de mantener tus programas *seguros y deterministas*.

Dales la bienvenida a `get` y `set`...
