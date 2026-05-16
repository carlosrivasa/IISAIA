# CLAUDE.md: la memoria que vos escribís

¿Esto actúa sobre el loop o sobre el contexto? El contexto. Cada vez que arrancás una sesión de Claude Code, antes de que escribas una sola instrucción, el agente ya leyó un conjunto de archivos de texto plano. Esos archivos son los CLAUDE.md, y su contenido aparece inyectado en la ventana de contexto desde el primer intercambio. No hay prompts implícitos ni magia: es texto tuyo, cargado automáticamente, siempre.

La propiedad es simple y tiene consecuencias importantes: todo lo que ponés en un CLAUDE.md ocupa contexto en cada sesión, sin excepción. Eso hace a CLAUDE.md la herramienta más directa para moldear el comportamiento del agente de manera persistente — y también la que exige más disciplina sobre qué escribís.

## La jerarquía

CLAUDE.md no es un solo archivo. Es un sistema de cuatro niveles que Claude combina en orden, de lo más amplio a lo más específico.

**Nivel 1 — Política organizacional (managed policy).** Un administrador de sistemas instala este archivo en la máquina. Claude lo carga siempre y el usuario no puede excluirlo. Rutas: macOS `/Library/Application Support/ClaudeCode/CLAUDE.md`; Linux/WSL `/etc/claude-code/CLAUDE.md`; Windows `C:\Program Files\ClaudeCode\CLAUDE.md`. Caso de uso típico: estándares de seguridad y compliance que aplican a todos los proyectos de la organización, independientemente de quién sea el desarrollador.

**Nivel 2 — Instrucciones de usuario.** `~/.claude/CLAUDE.md`. Preferencias personales que seguís a través de todos los proyectos: estilo de respuesta, idioma de comentarios, convenciones propias. Solo las lés vos; no se comparte con el equipo.

**Nivel 3 — Instrucciones de proyecto.** `./CLAUDE.md` o `./.claude/CLAUDE.md` — en la raíz del repositorio. Este es el nivel del equipo: arquitectura, convenciones de código, workflows de la herramienta, restricciones del dominio. Se versiona junto con el código y lo leen todos los que trabajan en el repo.

**Nivel 4 — Instrucciones locales.** `./CLAUDE.local.md`. Nivel personal dentro de un proyecto específico: rutas de tu máquina, configuraciones que no querés en el repo, experimentos temporales. Agregalo al `.gitignore` para que no viaje al control de versiones.

## Orden de carga

Claude no elige uno de estos niveles: los combina todos. El mecanismo es una caminata hacia arriba en el árbol de directorios desde el directorio de trabajo actual. Claude recolecta cada CLAUDE.md que encuentra en cada directorio del camino hacia la raíz del sistema de archivos.

El orden de carga va de más amplio a más específico: primero lo que está cerca de la raíz del sistema, último lo que está más cerca del directorio de trabajo actual. El resultado es **concatenación**, no sobreescritura. Si el nivel de usuario dice "respondé en inglés" y el nivel de proyecto dice "respondé en español", ambas instrucciones entran al contexto y Claude puede resolverlas de manera arbitraria — a favor de una u otra, o de la que leyó último. La consecuencia práctica es que las contradicciones entre niveles no producen un error: producen comportamiento impredecible. Revisá y limpiá las instrucciones que se pisarían.

Dentro de cada directorio, si existen ambos archivos, `CLAUDE.local.md` se agrega después de `CLAUDE.md`. Y los CLAUDE.md que estén en subdirectorios no se cargan al arranque: se cargan bajo demanda, cuando Claude lee archivos que viven en esos subdirectorios.

## @import

Un CLAUDE.md puede referenciar otros archivos con la sintaxis `@ruta/al/archivo`. La ruta puede ser relativa (se resuelve respecto al archivo que contiene el import) o absoluta. Los imports son recursivos hasta 5 niveles de profundidad.

El caso de uso más frecuente es mantener organización sin duplicar contenido: un CLAUDE.md de proyecto que importa un `AGENTS.md` compartido entre varios repos, o partir un archivo de instrucciones muy largo en secciones temáticas separadas.

## La propiedad clave: siempre cuesta contexto

Acá está el punto que más se confunde, y conviene dejarlo claro: **`@import` no ahorra contexto.** Los archivos importados se expanden e inyectan en la ventana de contexto al arranque de la sesión, exactamente igual que el CLAUDE.md que los referencia. Dividir las instrucciones en varios archivos e importarlos ayuda con la organización — un archivo por tema, más fácil de mantener — pero no reduce ni un token el costo en contexto.

Esto contrasta con lo que veremos en §8. CLAUDE.md es el mecanismo **siempre cargado**: cualquier instrucción que escribís ahí está en la ventana desde el primer intercambio de cada sesión, sin importar si esa instrucción es relevante para la tarea del momento. El costo es constante.

Dos detalles operativos que ayudan a mantener los archivos útiles. Primero, los comentarios HTML en bloque (`<!-- ... -->`) se eliminan antes de que el contenido se inyecte en el contexto; podés escribir notas para otros mantenedores del archivo sin gastar tokens. Segundo, la guía de tamaño recomendada es menos de 200 líneas por archivo CLAUDE.md — los archivos más largos tienden a reducir la adherencia a las instrucciones que contienen.

## Este repo como ejemplo vivo

El CLAUDE.md en la raíz de este repositorio (`/Users/enzo/Documents/IISAIA/CLAUDE.md`) es una instancia concreta del nivel de proyecto. Tiene una tabla de rutas del repositorio, un routing table que mapea tareas a archivos, y convenciones de workflow de git. No define preferencias personales ni políticas organizacionales: define lo que cualquier persona que trabaje en este repo necesita saber para no adivinar la estructura.

Cuando abrís Claude Code en este directorio, ese CLAUDE.md entra completo a la ventana antes de tu primer mensaje. El agente ya sabe que las slides están en `semanas/NN/slides/`, que los patrones reutilizables están en `shared/patterns/`, y que nunca se commitea directo a `main`. No se lo dijiste vos en ese momento; lo encontró en el archivo.

## Mini-demo en vivo: la ventana antes y después

El objetivo de este demo (D3) es hacer visible que CLAUDE.md ocupa contexto desde el arranque, y que la jerarquía es real — no un diagrama de documentación.

**Intro.** Vamos a ver la diferencia de contexto entre una sesión sin instrucciones y una sesión con un CLAUDE.md de proyecto. No es una diferencia teórica: es una diferencia medible en la ventana.

**Beat 1.** Qué hago: abro Claude Code en un directorio vacío, sin ningún CLAUDE.md. Ejecuto `/context`. Qué digo: *"Esto es lo que carga de entrada, sin instrucciones nuestras. La ventana existe, pero está casi vacía de configuración nuestra."* Qué mirar: el tamaño del contexto base, sin instrucciones de proyecto.

**Beat 2.** Qué hago: agrego un `CLAUDE.md` con algunas instrucciones — convenciones de nombrado, una regla de estilo, la estructura del directorio — y reinicio la sesión. Vuelvo a ejecutar `/context`. Qué digo: *"Mismo repo, una sesión nueva. Miren qué cambió antes de que yo pida nada."* Qué mirar: el delta de contexto que se puede atribuir al CLAUDE.md. Las instrucciones que escribimos aparecen ahí, antes del primer intercambio.

**Beat 3.** Qué hago: ejecuto `/memory`. Qué digo: *"La jerarquía no es un dibujo. Esta es la lista real de fuentes de instrucción cargadas en esta sesión."* Qué mirar: los niveles efectivamente cargados — usuario, proyecto, local si existe — con sus rutas.

**Plan B si no coopera:** si `/context` no está disponible en la versión instalada, usar `/memory` solo para mostrar la lista de archivos cargados. El punto de "siempre cargado" se sostiene con solo narrar: *"todo lo que ven en esta lista entró a la ventana antes de este mensaje."*

---

CLAUDE.md está siempre en la ventana. Es el mecanismo más directo y también el menos selectivo: cada sesión, cada tarea, con o sin relevancia para lo que estás haciendo en ese momento. Lo que lleva a una pregunta natural: ¿y si una instrucción pudiera estar en el contexto *solo cuando hace falta*, y no ocupar espacio el resto del tiempo? Eso es exactamente lo que veremos en §8.
