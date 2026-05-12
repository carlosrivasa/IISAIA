# Prompt inicial, a partir de la idea de construir una botonera de sonidos, y las indicaciones vistos en la clase

Actúa como un desarrollador frontend sádico y experto en diseño hostil, fuertemente inspirado en la comunidad r/badUIbattles. 

Tu objetivo es escribir el código completo para una "Botonera de Efectos de Sonido" (Soundboard) completamente funcional, pero diseñada para causar frustración y confusión al usuario. 

Constraints: 
- Todo el código (HTML, CSS y JavaScript) debe estar contenido obligatoriamente dentro de un único archivo index.html. 
- No uses frameworks externos de JS ni llamadas a APIs; usa Vanilla JS.

- 1. Estructura (HTML): Construye la interfaz usando etiquetas como <main>, <section> y <button>. Distribúyelas de forma caótica. Usa una etiqueta <marquee> para el título principal. Cada botón debe tener tamaños completamente irregulares. 

- 2. Estilo (CSS): Diseña una pesadilla visual. Usa una paleta de colores de alto contraste que lastime la vista. La tipografía debe ser Comic Sans MS o Papyrus, con tamaños de fuente que no tengan ningún sentido lógico. Añade animaciones que hagan que algunos botones palpiten o giren lentamente sin ningún motivo. El cursor debe ocultarse o cambiar a algo inútil al pasar por encima de los botones. 

- 3. Comportamiento (JavaScript y Audio): La botonera debe generar ruidos estridentes, graciosos y molestos. Audio: No uses etiquetas <audio> con archivos .mp3 externos. Usa la Web Audio API (Oscillators) para sintetizar tonos directamente con código (ondas de sierra, cuadradas, frecuencias aleatorias, glissandos exagerados). 
Implementa las siguientes lógicas en los botones: Botones escurridizos: Algunos botones deben huir del cursor (mouseover) cambiando su posición antes de que el usuario pueda hacer clic. Retrasos aleatorios: Algunos sonidos deben reproducirse 3 o 4 segundos después de haber hecho clic. Falsa interactividad: Un botón que cambie de tamaño al hacer clic, pero reproduzca el sonido de otro botón. Volumen variable: El volumen de cada sintetizador debe ser aleatorio en cada clic (a veces casi inaudible, a veces saturado). 

Genera el archivo completo. Solo devuelve el bloque de código, sin explicaciones introductorias.


# El resultado fue muy completo, pero demasiado caótico, necesitaba ajustes para hacerlo funcional. Prompt 2:

Me encantó la estructura y el estilo.
Como revisor, cambiaría el hecho de que es realmente complicado de darle click a los botones, se puede ser un poco más flexible con el usuario en términos de la lógica de los botones. De hecho el puntero desaparece y es imposible rastrearlo, sería frustrante que el cursor aparezca y que se mantenga el resto de la funcionalidad. 

Dame solo los cambios puntuales en el código con este cambio de experiencia, JS, y CSS


# Un ajuste menor en términos de la lógica de los botones huidizos. Prompt 3:

La probabilidad de huir está bien, ahora, no entiendo esta lógica /* Movimiento corto en vez de teletransporte total */ const offsetX = (Math.random() - 0.5) * 140; const offsetY = (Math.random() - 0.5) * 90; podemos hacerla más radical ahora?

# Todo en orden. Prompt 4:

Ayúdame a generar un readme file README.md teniendo en cuenta lo siguiente: qué es la página, quién lo construyó, qué funcionó y qué no.


# Me llamó la atención el tono irónico del README generado, ¿cómo lo hizo? Prompt 5:

"Cómo haces para generar el tono irónico del README, pareciera contextualizado en los aportes de la comunidad r/badUIbattles. Me puedes dar un resumen de cómo funciona el mecanismo del chat gpt para contextualizar el prompt de manera tan específica?
O más resumido y concreto posible.

Tu prompt define un “universo estilístico”.
El LLM detecta patrones asociados a:

humor irónico
UX hostil
cultura internet/dev
estética meme

Y luego predice el texto que “más encaja” con ese contexto.
comprime patrones y continúa el tono que ya estableciste."

