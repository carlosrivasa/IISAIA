# Spine — Semana 04: Fundamentos de Agentic AI y Claude Code

**Whole-week through-line:** Una IA que *actúa* (no que sugiere) se dirige con un set de herramientas conceptuales más amplio que "escribir un buen prompt". La parte 1 (§1–§5) construye un marco de **tres ingenierías anidadas** — `prompt ⊂ context ⊂ harness` — sin nombrar ninguna herramienta. La parte 2 (§6–§11) revela que cada feature de Claude Code llena uno de esos niveles, y propone una pregunta-lente que el alumno arrastra slide a slide: *¿esto actúa sobre qué ve el agente dentro de una sesión (context), o sobre el entorno donde el agente opera (harness)?* Prompt engineering ya lo venían practicando; lo que esta clase abre son los dos niveles nuevos.

**Dispositivos pedagógicos de toda la semana:**
- **Las tres ingenierías como marco**: se introducen anidadas en §3.3 (`prompt ⊂ context ⊂ harness`), §4 desarrolla **context engineering** completo, §5 desarrolla **harness engineering** completo, §5.5 cierra Parte 1 con el recap de los 3 niveles. Cada feature de Parte 2 se ubica explícitamente en uno de los dos niveles nuevos.
- **Lens-tracker `CONTEXT ↔ HARNESS`**: helper visual compartido (`.lens-tracker` en el scaffold). Se introduce neutral en §6.5, aterriza en CONTEXT en §7–§10, **flipea a HARNESS en §11** (primer flip, momento deliberado), y vuelve a neutral en §12 como síntesis. Mismo rol que el `pipeline-roadmap` re-iluminado en semana 03.
- **Animación del loop (bespoke, nueva)**: una sola animación JS nueva en todo el deck (`four-loop-anim.js`). Cicla pensar → actuar → observar → repetir con las 4 condiciones de corte como chips de salida. Se introduce en §2.2 (ReAct), se re-conduce en §2.3 (fragment-driven highlight de cada fase) y en §6.3 (modo `cc` con los nombres reales de tools de Claude Code).
- **Costura parte 1 / parte 2**: `section-divider` fuerte entre §5 y §6 ("Parte 2 — Claude Code"). §1–§5 nunca nombran Claude Code. §6 revela que el loop abstracto de §2 era Claude Code todo el tiempo y reaterriza explícitamente en las tres ingenierías de §3.3.

**Nota de escala:** 14 secciones, ~220 min de contenido. Con tratamiento estructurado de 7 demos en vivo (uno por feature de Parte 2) el deck es grande (~120–140 slides). Es una clase larga, dictada en 2 sesiones; la costura parte 1 / parte 2 entre §5 y §6 es el corte natural.

**Nota sobre el rediseño de Parte 2 (2026-05-19):** la Parte 2 se rediseñó para cubrir 7 features de Claude Code como secciones independientes (§7–§13) siguiendo una plantilla documentation-walkthrough (qué es / dónde vive / cuándo se carga / cómo se usa / casos límite / mini-demo). El `.lens-tracker` introducido en §6.5 **ya no se recoge en §7–§13** — la decisión es deliberada (foco en cada feature por su propia anatomía, no por su ubicación en una taxonomía). Se acepta el costo de que el setup del tracker en §6 queda sin payoff. El dispositivo recurrente nuevo es la **tarjeta de 5 preguntas** que abre cada sección. El demo se hace en un repo aparte (`semanas/04/demo-repo/`, FastAPI + frontend) que el profesor copia fuera del repo del curso antes de dictar para aislarlo.

**Nota sobre relación con `source_material/index.md`:** el material fuente todavía organiza Parte 1 alrededor de "loop + contexto" y trata §3 como "Tools — las manos del agente" y §5 como "Modos de falla". El deck reorganizó esos contenidos: las tools entraron en §2 (como bridge de single-turn → ReAct), §3 pasó a ser "Las tres ingenierías" (un slide marco nuevo), y §5 pasó a ser "Harness engineering" (el otro nivel nuevo). El espíritu del material fuente sobrevive (loop, finitud, tools como manos, supervisión, etc.) pero la organización es distinta — el spine es el contrato canónico de qué hay en cada sección.

---

## Section 1: De escribir a actuar
**Source material:** `source_material/01-de-escribir-a-actuar.md` (+ callback a S1)
**Through-line:** El cambio de paradigma no es "una IA que escribe mejor" — eso sería más de lo mismo. Es que la IA ahora *actúa*: cierra el loop sobre tu repo sin que vos intervengas en cada vuelta. Para dirigir algo que actúa hay que abrir la caja del "programa que envuelve al LLM" — esa caja se va a llenar de piezas en las próximas secciones.
**Hook:** La última vez que le pediste a una IA que arreglara un endpoint: copiaste el error, lo pegaste, copiaste el código, lo pegaste, corriste, viste un 500 distinto, volviste al chat. Cada vuelta de esa rueda la hacías vos. *Vos eras el que corría el loop.*
**What students walk away knowing:**
- Recap S1 expandido en 5 estados: la unidad del LLM es predecir un token, pero alrededor siempre hay un **programa externo** que (1) lo mete en un loop con `max_tokens`, (2) le envuelve la entrada con tokens de rol (SFT), (3) filtra los tokens de razonamiento del output (RL), y (4) ensambla `system + historial + mensaje nuevo` turno a turno. La caja punteada es el protagonista de toda la semana.
- Son dos paradigmas distintos, no versiones mejor/peor: IA que sugiere + vos conducís, vs. agente que planifica y ejecuta en loop; el LLM debajo puede ser el mismo, lo que cambia es la arquitectura encima — el programa externo evoluciona.
- El rol de supervisor (semanas 2–3) no desaparece; lo que cambia es qué hay para supervisar. Hoy no aprendés una herramienta — aprendés la máquina que vas a dirigir el resto del curso.
**Animations / interactive:** El recap S1 usa fragments para revelar progresivamente los 5 estados (el primero arranca visible; los stages 2-4 aparecen al costado uno por uno; el stage 5 reemplaza la fila completa para mostrar el ensamblaje conversacional con system / historial / mensaje nuevo). `comparison-2col` para los dos paradigmas + diagrama cíclico CSS de la rueda copy-paste manual con la caja "Chat" dashed que representa al subsistema del slide anterior. Sin animación JS — la del loop entra en §2.
**Slide budget actual:** 3 slides.

## Section 2: El loop — pensar, actuar, observar
**Source material:** `source_material/02-el-loop-agentic.md` + `source_material/03-tools-las-manos.md` (las tools entran acá como bridge desde S1)
**Through-line:** La diferencia entre un chat que sugiere y un agente que resuelve no está en el modelo de adentro: está en que el agente no da una respuesta, entra en un loop (pensar → actuar → observar) que se repite vuelta a vuelta hasta una condición de corte. Las tools — ya conocidas de S1 — eran **una vuelta sola**; ReAct es el paso al **loop**. Un agente = LLM + loop + tools + entorno; sin condición de corte el loop no converge.
**Hook (§2.1):** Las tools que vieron en S1 ubicadas en el programa externo del §1 — visualmente del mismo idioma (caja punteada + reglas del runtime). El énfasis está en que esa arquitectura es **una sola vuelta del LLM**: aunque llame varias tools dentro de esa vuelta, sigue siendo single-turn. Eso prepara el contraste con ReAct.
**What students walk away knowing:**
- Tools = otra responsabilidad del programa externo (callback explícito a S1): cuando el LLM emite un token de tool call, el programa pausa la generación, ejecuta, inyecta el resultado, el LLM sigue generando. Single-turn — el modelo no decide qué hacer con los resultados.
- ReAct = cuando una vuelta no alcanza. El programa externo entra en un **loop**: el LLM piensa, actúa usando tools, observa los resultados, y decide la próxima acción. La diferencia con single-turn no es la cantidad de tools — es la **iteración**: el modelo decide entre vuelta y vuelta a partir de lo observado.
- Tres fases que se *alternan* (Reasoning + Acting entrelazados), no etapas separadas; cada fase tiene una salida concreta: pensar → decisión; actuar → cambio en el entorno; observar → información nueva en el contexto. Slide consolidado: misma animación, tres iluminaciones por fragment.
- El trace concreto (404 en producción): 2 vueltas — la primera identifica (logs → router con slash duplicado), la segunda corrige y verifica (edit → curl 200). La condición de corte es "objetivo alcanzado".
- Un agente son 4 piezas necesarias en conjunto (LLM + loop + tools + entorno); cuando algo falla, el diagnóstico empieza por "¿cuál de las 4?". Cierre operativo de la sección.
- Cuatro condiciones de corte que aparecen en la práctica: objetivo alcanzado / límite de pasos / pide ayuda / falla no recuperable. Ya se adelantaron como chips en la animación al introducir ReAct; este slide las abre.
**Animations / interactive:** **Bespoke loop animation — NUEVA, se introduce acá** (`four-loop-anim.js`, mode `intro`). Dos instancias en la sección: (a) §2.2 ReAct intro — animación completa con chips de corte, escalada 1.4×, fragment-mounted; (b) §2.3 Tres fases — la misma animación con `phase` cambiando por fragment (pensar → actuar → observar). Bridge §2.1 reusa el idiom visual del §1 (caja punteada + runtime rules). Trace concreto (§2.6) es CSS-only — un trace de 6 filas con color por fase y conectores verticales, sin animación; el puntero del docente lo navega.
**Slide budget actual:** 6 slides (bridge tools → ReAct intro → tres fases consolidado → trace 404 → 4 piezas → condiciones de corte).

## Section 3: Las tres ingenierías
**Source material:** **No mapea a un archivo único del source_material** — es un slide marco nuevo que organiza el resto de la clase. Recoge motivación de `03-tools-las-manos.md` (el abanico de capacidades) y `05-cuando-el-agente-falla.md` (gran poder, gran responsabilidad) para justificar por qué prompt engineering ya no alcanza.
**Through-line:** ReAct + tools no son piezas técnicas aisladas — combinadas abren un rango operativo enorme (escribir código de cero, correr tests, diagnosticar bugs, refactorizar, sintetizar docs, encadenar tools). Ese abanico cambia la pregunta de la clase: ya no es "¿cómo escribo un buen prompt?", es "¿cómo dirijo algo que puede hacer todo eso?". La respuesta es un marco de **tres ingenierías anidadas por alcance** — `prompt ⊂ context ⊂ harness` — donde prompt engineering es lo que ya conocen y los dos niveles exteriores son lo que abre esta clase y la parte 2.
**Hook (§3.1):** Recorrido visual del abanico de capacidades — seis cards de capacidades concretas (escribir, correr tests, diagnosticar, refactorizar, sintetizar docs, encadenar tools), revelado de una con cierre punzante: "casi cualquier cosa que un developer hace en una terminal y un editor, el agente la puede hacer".
**What students walk away knowing:**
- Cuando un sistema puede modificar tu repo, ejecutar comandos y tomar decisiones por su cuenta, **lo que sale bien escala — y lo que sale mal también**. Un fix automático vale por diez horas tuyas; un `rm` equivocado las cuesta. Slide bisagra: el alcance abierto por ReAct + tools exige una disciplina más amplia que prompt engineering.
- **Tres ingenierías por alcance creciente**: (a) **prompt engineering** — alcance: una interacción; ya lo conocen de S2–S3, sigue siendo la base, no desaparece. (b) **context engineering** — alcance: una sesión; la ventana de contexto como recurso a gestionar (qué se carga, qué se mantiene, qué se descarta). (c) **harness engineering** — alcance: múltiples sesiones; el entorno completo (tools disponibles, guardrails, scripts deterministas, feedback loops).
- Los tres alcances **se anidan**: `prompt ⊂ context ⊂ harness`. Cualquier prompt vive dentro de una sesión, cualquier sesión vive dentro de un harness. Esta línea es la que monta el resto del deck — §4 abre context, §5 abre harness, y toda la Parte 2 ubica cada feature de Claude Code en uno de los tres niveles.
**Animations / interactive:** CSS-only. §3.1 grilla 3×2 + reveal de la línea de cierre. §3.2 quote slide con peso tipográfico + dos reveals (la línea media, el punch). §3.3 grilla 1×3 con reveals progresivos (prompt → context → harness → línea del anidamiento). Sin JS nuevo.
**Slide budget actual:** 3 slides (abanico → gran poder, gran responsabilidad → tres ingenierías anidadas).

## Section 4: Context engineering
**Source material:** `source_material/04-la-ventana-es-todo.md`
**Through-line:** La working memory del agente (la ventana de contexto) es finita por diseño; el loop la llena vuelta a vuelta más rápido de lo que intuís. Cuando se llena, algo se va o algo para. El **context rot** — instrucciones del inicio sepultadas bajo observaciones recientes que pesan más — es la consecuencia. **Tesis de la disciplina:** quien dirige bien un agente no es el que escribe el mejor prompt, es el que mantiene la ventana enfocada en lo que importa. Y eso importa por dos razones: **calidad** (un prompt brillante no te salva si a la vuelta 40 la ventana está saturada) y **costo** (cada token se factura en cada vuelta del loop).
**Hook:** Slide opener consolida la definición operativa de context engineering (`alcance: una sesión completa — qué se carga, qué se mantiene, qué se descarta`) con la tesis revelada por fragment. El opener narrativo del material fuente ("¿por qué un agente que venía funcionando 'olvida'?") está absorbido en las anotaciones y en el desarrollo de context rot más adelante.
**What students walk away knowing:**
- **Parámetros vs ventana** (callback S1): los parámetros son un recuerdo vago (algo que leíste hace un mes); la ventana es working memory — acceso directo, lo que está ahí el modelo **lo ve**, no lo recuerda. La ventana pasa a ser **el recurso que vas a administrar**.
- **El loop llena la ventana**: barra CSS animada con tres anotaciones progresivas (~150k tokens tarea acotada / ~500k sesión normal / ~900k sesión larga sobre ventana de 1M). **Cada token cuenta dos veces**: ocupa lugar en la ventana **y** se factura en cada llamada al modelo — 50 vueltas con ventana al 90% = ~45M tokens facturados solo en input. La conciencia de costo entra acá, no más adelante.
- **Finita por diseño, no por descuido**: propiedad estructural de los transformers, no un defecto a corregir. Ventanas más grandes desplazan el límite, no lo eliminan.
- **Context rot — cómo ocurre**: barra + diagrama de capas mostrando la instrucción original ("no uses dependencias externas") sepultada bajo observaciones de las vueltas 14, 28, 35; vuelta 42 el agente decide `npm install express lodash axios` contradiciendo la instrucción que técnicamente sigue ahí. La señal no desapareció — se diluyó en el ruido.
- **Síntomas que reconoce un supervisor** (grid 2×2): contradice restricciones / repite trabajo hecho / respuestas genéricas / "no recuerda" lo que está ahí. La línea de cierre invierte el framing: *no es que el modelo falló — la ventana está trabajando contra vos*.
- **Dos operaciones** (agnósticas de herramienta): **compactar** (resumir y soltar lastre — preserva la señal, descarta los detalles, sigue la sesión) y **resetear** (empezar limpio cargando solo lo necesario — pierde la conversación, arranca fresco). Ninguna es mejor en abstracto; toda estrategia real combina las dos.
- **Cierre**: "elegir qué entra a la ventana, mantener lo que sirve, soltar lo que pesa". Bridge al siguiente nivel: el modelo opera sobre un entorno — ese entorno también se diseña, y ese es el harness.
**Animations / interactive:** CSS-only. Dos barras CSS de relleno (una en §4.3 con tres reveals que la llenan al 15/50/90%, otra en §4.5 que crece con cada capa del diagrama de context rot: 25 → 55 → 85 → 95%). Listeners de Reveal (`fragmentshown`/`fragmenthidden`/`slidechanged`) recalculan la altura desde los `data-fill` / `data-rot-fill` visibles para que ir y volver sea consistente. Sin JS de animación nuevo — la animación del loop no se reusa acá.
**Slide budget actual:** 8 slides (opener + tesis → parámetros vs ventana → el loop llena la ventana → finita por diseño → context rot → síntomas → compactar/resetear → cierre + bridge).

## Section 5: Harness engineering
**Source material:** **Reorganiza fuerte respecto a `source_material/05-cuando-el-agente-falla.md`.** El archivo del source_material trataba "modos de falla y el rol del supervisor"; el deck lo reemplazó por harness engineering. La motivación de "falla = entorno que ya cambió" sobrevive distribuida (en §3.2 "gran poder, gran responsabilidad", y en el lenguaje de guardrails / verificación de §5.2). No hay sección dedicada a los 4 modos de falla con el detalle del source.
**Through-line:** Acabamos de cerrar context engineering. **Falta el nivel exterior**: el entorno donde opera el agente. Harness — literalmente, el arnés del caballo: lo que canaliza la potencia del modelo en la dirección que querés. **No es pedirle al modelo que se comporte ("no cometas errores"), es diseñar el entorno para que tire de la carreta**. La disciplina se divide en 6 dimensiones y cada una mapea a una feature concreta de Claude Code, motivando la Parte 2.
**Hook (§5.1):** Metáfora del caballo en 3 tiempos. Entry: solo la etimología ("harness = arnés"). Primer reveal: imagen `caballo-descontrolado.png` + caption irónico "no cometas errores" (en mono, como prompt fallido). Segundo reveal: imagen `caballlo-harness.png` + caption "Harness Engineering". El contraste visual hace el argumento sin texto explicativo.
**What students walk away knowing:**
- Harness engineering = **diseñar el arnés que canaliza al modelo**, no pedirle que se comporte solo.
- **Seis dimensiones** del harness (grid 3×2 con reveals progresivos): (1) estructurás el prompt — instrucciones fijas, voz, prioridades, convenciones; (2) exponés tools — el borde de lo que sabe hacer; (3) cargás contexto — info variable que entra cada turno; (4) armás feedback loops — el canal que devuelve resultados al modelo; (5) verificás outputs — el criterio externo de correctitud (contratos, tests, schemas); (6) descomponés tareas — cómo se rompe el problema en pasos o sub-agentes.
- **La frontera con context engineering** (slide de honestidad disciplinaria): los límites son difusos — "qué contexto cargás" es parte del harness *y* es lo que define context engineering. La lectura específica que sirve para Claude Code: harness = la capa de runtime que rodea al modelo (system prompt, definiciones de tools, lógica de reintentos, orquestación de sub-agentes, pasos de verificación) — distinta del modelo en sí y de un prompt suelto.
- **Harness en Claude Code** (mapping bridge a Parte 2, grid 3×2): `CLAUDE.md` (system prompt + contexto que se carga al arrancar) / slash commands custom (prompts reutilizables, atajos a workflows) / servidores MCP (qué tools exponés) / sub-agent workflows (descomposición en contextos aislados) / hooks (scripts antes/después de cada tool call — verificación + feedback automáticos) / eval loops (medir si los cambios al harness sirven, no suponer).
- **Cierre de Parte 1** (recap tres ingenierías): grilla con prompt / context / harness y la línea bridge — "cada feature de Claude Code que viene llena uno de estos tres niveles. Cuando veas una feature nueva, la pregunta ya no es '¿qué hace?' — es '¿en qué nivel actúa?'". Esa pregunta es exactamente lo que va a aterrizar como tracker en §6.5.
**Animations / interactive:** CSS-only. Reveals progresivos en la metáfora del caballo (las dos imágenes aparecen una por una), en la grilla de 6 dimensiones (una card por reveal), en el harness CC (todas visibles juntas), y en el recap final (las 3 cards + bridge revealed). Sin JS de animación nuevo. Imágenes en `../img/caballo-descontrolado.png` y `../img/caballlo-harness.png`.
**Slide budget actual:** 5 slides (opener caballo → 6 dimensiones → frontera con context → harness en Claude Code → cierre tres ingenierías + bridge a Parte 2).

---

# Parte 2 — Claude Code (§6–§13)

> **Rediseño 2026-05-19.** Parte 2 cubre las 7 piezas configurables del runtime de Claude Code, una por sección (§7–§13), siguiendo plantilla documentation-walkthrough: *qué es / dónde vive / cuándo se carga / cómo se usa / casos límite / mini-demo*. La fuente canónica del contenido es `source_material/06-claude-code-es-el-loop.md` y `source_material/07-...md` a `13-...md`. **El `.lens-tracker` introducido en §6.5 ya no se recoge** — decisión deliberada del rediseño. El dispositivo recurrente nuevo es la tarjeta de 5 preguntas como divider de cada sección.

## Section 6: Claude Code es ese loop
**Source material:** `source_material/06-claude-code-es-el-loop.md`
**Through-line:** Claude Code no es un concepto nuevo: es el loop del §2 con nombre propio y pantalla. Las 4 piezas del §2 mapean 1:1. (Nota del rediseño: el `.lens-tracker` que esta sección introduce ya no se recoge en §7–§13; el setup queda colgado.)
**Hook (§6.2):** "Lo que describimos en §2 como pensar → actuar → observar no era un diagrama abstracto. Era la descripción de algo que podés abrir ahora mismo en la terminal y dejar corriendo."
**Animations / interactive:** Re-uso de `four-loop-anim.js` en modo `cc`. Se introduce el `.lens-tracker` neutral (sin recogida posterior). `section-divider` fuerte para abrir la Parte 2.
**Slide budget actual:** 4 slides.

## Section 7: CLAUDE.md y memoria automática
**Source material:** `source_material/07-CLAUDE-md.md`
**Through-line:** Archivos markdown leídos al arranque de cada sesión; texto del usuario inyectado en la ventana de contexto antes del primer mensaje. Jerarquía de 4 niveles concatenados (no override). Auto memory como sistema distinto que vive en el mismo archivo conceptual.
**What students walk away knowing:**
- CLAUDE.md siempre cuesta contexto. Jerarquía de 4 niveles (managed / user / project / local) concatenados caminando hacia arriba.
- `@import` no ahorra contexto. Comentarios HTML descartados. Guía <200 líneas.
- Auto memory: lo escribe Claude, vive en `~/.claude/projects/<project>/memory/`, primeras 200 líneas o 25 KB.
**Plantilla obligatoria (6 sub-secciones):** Qué es / Dónde vive / Cuándo se carga / Cómo se usa / Casos límite / Mini-demo. Tarjeta de 5 preguntas como divider de apertura.
**Animations / interactive:** Tarjeta de plantilla. `pipeline-roadmap` o tabla para los 4 niveles. `code-walkthrough` del `CLAUDE.md` del demo-repo.
**Mini-demo (único bloque al final):** abrir Claude Code en `demo-repo/`, ejecutar `/memory` y `/context`, mostrar las 2 fuentes (user + project), mostrar carpeta de auto memory. Plan B: solo `/memory`.
**Slide budget objetivo:** 10 slides.

## Section 8: Rules (`.claude/rules/`)
**Source material:** `source_material/08-rules.md`
**Through-line:** Archivos `.md` modulares de instrucciones. Sin frontmatter = siempre cargados (ventaja organizativa sobre CLAUDE.md monolítico). Con frontmatter `paths:` + globs = path-scoped, cero contexto hasta tocar archivo que matchee.
**What students walk away knowing:**
- Estructura de `.claude/rules/` con un archivo por tema (`code-style.md`, `testing.md`, `security.md`, `api.md`).
- Frontmatter `paths:` activa la rule solo al leer un archivo en el glob.
- NO es `.cursorrules` (mecanismo distinto, no compatible).
**Plantilla obligatoria:** misma de 6 sub-secciones.
**Animations / interactive:** Tarjeta de plantilla. `code-walkthrough` de `api.md` con frontmatter. Tabla con los 4 archivos del demo-repo y su comportamiento.
**Mini-demo:** el árbol `.claude/rules/`, `/memory` antes/durante/después de tocar `backend/routers/users.py`. Plan B: por comportamiento (la rule prescribe regenerar `openapi.yaml`).
**Slide budget objetivo:** 9 slides.

## Section 9: `settings.json` (config del runtime)
**Source material:** `source_material/09-settings-json.md`
**Through-line:** Archivo JSON que configura el runtime de Claude Code. NO son instrucciones para el agente (eso es CLAUDE.md/rules); son parámetros del programa que envuelve al LLM. 4 niveles: user / project / local (gitignored) / managed.
**What students walk away knowing:**
- Campos clave: `model`, `env`, `permissions`, `hooks`, `includeCoAuthoredBy`, `cleanupPeriodDays`, `permissionMode`.
- Precedencia entre niveles. `.local.json` siempre al `.gitignore`.
- No confundir con `mcp.json`. `permissions` es el campo más impactante (§10).
**Plantilla obligatoria:** misma de 6 sub-secciones.
**Animations / interactive:** Tarjeta de plantilla. Walkthrough del `.claude/settings.json` del demo-repo campo por campo.
**Mini-demo:** abrir los 3 archivos de settings simultáneamente, cambiar `model`, mostrar el `.gitignore` con `.claude/settings.local.json`.
**Slide budget objetivo:** 9 slides.

## Section 10: Permisos (control de tools)
**Source material:** `source_material/10-permisos.md`
**Through-line:** Sistema que decide qué tools puede ejecutar Claude sin pedir confirmación. Vive dentro de `permissions` en `settings.json`. Tres listas: `allow` / `deny` / `ask`. **`deny` tiene precedencia sobre `allow`.**
**What students walk away knowing:**
- Sintaxis `Tool(pattern)` con ejemplos (`Bash(uv run pytest)`, `Edit(./backend/**)`, `Read(./.env)` en deny).
- Lógica de evaluación: deny → bloquea; allow + no deny → ejecuta; ask + no allow → pregunta.
- Permission modes como overlay (`default`, `acceptEdits`, `plan`, `bypassPermissions`) — se introducen acá, `plan` se desarrolla en §13.
**Plantilla obligatoria:** misma de 6 sub-secciones.
**Animations / interactive:** Tarjeta de plantilla. Tres code blocks separados (allow / deny / ask) con ejemplos del demo-repo. Tabla de los 4 permission modes.
**Mini-demo:** prompt de permiso aparece, `/permissions` lo agrega al `allow`, repetir y pasa sin prompt. Cerrar con `deny` que bloquea `Read(./.env)`.
**Slide budget objetivo:** 11 slides.

## Section 11: Skills y slash commands
**Source material:** `source_material/11-skills-y-slash-commands.md`
**Through-line:** Skill = procedimiento completo (fases, verificaciones, anti-patterns). Slash command = atajo. Distinción central. Las skills NO están en el contexto al arranque — solo nombre + descripción.
**What students walk away knowing:**
- Estructura del SKILL.md (frontmatter + fases + anti-patterns). Ejemplo: `.claude/skills/add-endpoint/` del demo-repo.
- Slash command como disparador. Ejemplo: `.claude/commands/add-endpoint.md` invoca la skill.
- Algunos slash commands NO invocan skills (`/memory`, `/context`, `/permissions`).
**Plantilla obligatoria:** misma de 6 sub-secciones.
**Animations / interactive:** Tarjeta de plantilla. Tabla "skill vs slash command". `code-walkthrough` del SKILL.md y del slash command.
**Mini-demo:** sesión nueva, `/context` sin la skill activa, invocar `/add-endpoint`, Claude sigue las 6 fases del SKILL.md. Contrastar con misma tarea sin la skill.
**Slide budget objetivo:** 11 slides.

## Section 12: Sub-agents
**Source material:** `source_material/12-sub-agents.md`
**Through-line:** Instancia separada del loop con su propia ventana de contexto. Mismo LLM + loop + tools + entorno instanciado aparte. Al padre solo vuelve el resultado.
**What students walk away knowing:**
- Sub-agent NO es "otra IA" — es la misma lógica, aislada.
- Definición con frontmatter (`name`, `description`, `tools` restringidos). Ejemplo: `.claude/agents/researcher.md`.
- El padre ve solo el resultado, no el razonamiento → la delegación no exime de supervisar.
- Paralelización es beneficio secundario; el motivo principal es proteger el contexto.
**Plantilla obligatoria:** misma de 6 sub-secciones.
**Animations / interactive:** Tarjeta de plantilla. Diagrama del padre + sub-loop con su ventana propia.
**Mini-demo:** misma pregunta de auth resuelta inline vs delegada al sub-agent `researcher`; `/context` antes/después en cada caso. Mostrar `researcher.md` con `tools:` restringido.
**Slide budget objetivo:** 9 slides.

## Section 13: Plan mode (+ permission modes)
**Source material:** `source_material/13-plan-mode.md`
**Through-line:** Modo de operación que separa "pensar" de "actuar". Claude lee y propone; las escrituras esperan aprobación. Uno de los 4 permission modes (`default` / `acceptEdits` / `plan` / `bypassPermissions`).
**What students walk away knowing:**
- Activación de plan mode (flag al arranque, campo en settings.json, `Shift+Tab` durante sesión).
- Flujo: tarea → plan → revisión → `ExitPlanMode` → ejecución.
- Criterios para elegir cada modo según reversibilidad. Plan mode = defensa más barata contra la deriva.
- Plan mode NO es sandbox. El gate de permisos (§10) y el modo son ortogonales.
**Plantilla obligatoria:** misma de 6 sub-secciones.
**Animations / interactive:** Tarjeta de plantilla. Tabla de los 4 permission modes con criterios. Diagrama del loop con la pausa entre pensar/actuar.
**Mini-demo:** refactor multi-archivo. Sin plan mode: edita de inmediato (cancelar). Con plan mode: propone, corregir, aprobar, ejecuta.
**Slide budget objetivo:** 11 slides.

## Section 14: El trabajo final
**Source material:** `source_material/14-trabajo-final.md`
**Through-line:** El cierre del arco completo del curso: modelo (S1) + rol (S2–3) + runtime (S4) se usan al mismo tiempo en un proyecto real. El trabajo final no examina sintaxis: examina si sabés *dirigir*. Apropiación = poder explicar, corregir y extender.
**What students walk away knowing:**
- Las tres piezas del curso y por qué cada una es necesaria: el modelo (sin él el agente es caja negra), el rol/vocabulario (sin él aceptás lo primero que el modelo tira), el runtime (sin él el proyecto largo da vueltas). El trabajo final las usa a la vez.
- La forma del proyecto (frontend + backend + persistencia; el dominio se define aparte) activa exactamente las decisiones aprendidas.
- Apropiación: el producto es tuyo aunque el agente escriba la mayoría del código.
**Animations / interactive:** Reusa `pipeline-roadmap` para el arco del curso. (Nota: la última aparición del tracker neutral ya no aplica — el tracker se descartó en §7–§13.)
**Slide budget actual:** se mantiene.
