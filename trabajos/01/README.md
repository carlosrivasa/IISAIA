# Trabajo 01 de IISAIA: Front-end asistido por IA con  ChatGPT Canvas

#### Qué es la página, quién lo construyó, qué funcionó y qué no.

- Qué: la página es una "Botonera de efectos de sonido"
- Quién: Lo hizo ChatGPT, asistido por Carlos Rivas, SIU a2227
- Funcionó: el prompt inicial tuvo mucho éxito, requirió solo de dos iteraciones cortas para refinar detalles de la implementación inicial.
- No funcionó: al comienzo el cursor desaparecía por mucho tiempo y era imposible retomar la funcionalidad, se solucionó rápidamente en el siguiente prompt.
- Nota: el último prompt está relacionado con lo de la primera clase, llama la atención el tono irónico del README generado.








##### A continuación el resultado del prompt 4, sin edición

# 🔊 Soundboard From Hell™

Una soundboard interactiva construida deliberadamente con malas decisiones de UX/UI, inspirada en la gloriosa energía caótica de comunidades como r/badUIbattles.

El objetivo del proyecto NO era crear una experiencia usable.

El objetivo era crear una experiencia técnicamente funcional… pero emocionalmente cuestionable.

---

# ✨ ¿Qué es esto?

Esta página es una “botonera de efectos de sonido” hecha con:

- HTML
- CSS
- Vanilla JavaScript
- Web Audio API

Todo contenido dentro de un único archivo `index.html`.

No hay frameworks.
No hay assets externos.
No hay archivos `.mp3`.
No hay dignidad.

Los sonidos son sintetizados en tiempo real usando osciladores (`OscillatorNode`) del navegador.

---

# 🧠 Filosofía de diseño

La interfaz fue diseñada siguiendo principios de:

- Frustración controlada
- Jerarquía visual inexistente
- Sobreestimulación cromática
- Interacciones hostiles
- Retroalimentación engañosa
- “¿Por qué el botón se movió otra vez?”

La experiencia intenta mantenerse en la línea entre:

> “esto está roto”

y

> “lamentablemente funciona”.

---

# 👷 Construido por

Diseño y desarrollo:
- Un humano con demasiado tiempo libre
- ChatGPT
- Energía caótica acumulada
- Malas decisiones conscientes

Inspiraciones:
- r/badUIbattles
- páginas web de 2004
- interfaces diseñadas por villanos
- software instalado desde CDs sospechosos

---

# 🔊 Features

## ✔️ Lo que sí funcionó

### Audio sintético con Web Audio API
Los sonidos se generan dinámicamente usando:
- ondas cuadradas
- ondas sierra
- glissandos
- frecuencias aleatorias
- cambios absurdos de volumen

---

### Botones escurridizos
Algunos botones:
- escapan del cursor
- se reposicionan aleatoriamente
- huyen justo antes del click

Pero ahora con una probabilidad razonable para evitar sufrimiento irreversible.

---

### Retrasos aleatorios
Algunos sonidos:
- responden 3–4 segundos después
- hacen creer que el botón no funcionó
- castigan la impaciencia

---

### Falsa interactividad
Hay botones que:
- cambian de tamaño
- parecen reaccionar correctamente
- pero reproducen otro sonido completamente distinto

Porque sí.

---

### Volumen ruleta rusa
Cada click puede ser:
- casi inaudible
- aceptable
- peligrosamente fuerte

Nunca lo sabrás.

---

### Estética visual ofensiva
Incluye:
- Comic Sans
- Papyrus
- magenta saturado
- verde neón
- amarillo insoportable
- animaciones innecesarias
- botones girando sin explicación

---

# ❌ Lo que NO funcionó tan bien

## Cursor invisible
Inicialmente el cursor desaparecía completamente.

Resultado:
- experiencia demasiado frustrante
- imposible rastrear interacción
- sensación genuina de software roto

Fue reemplazado por:
```css
cursor: crosshair;