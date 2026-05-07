# De lo local al stack

Hasta ahora todo pasaba en una pestaña del navegador. Canvas, ChatGPT, la página de la clase pasada. Eso funcionó porque el artefacto se sostenía solo: una UI que renderiza, un texto que se genera.

Un servidor de verdad no entra en una pestaña. Necesita un proceso que corra, un puerto donde escuchar, archivos en un disco y, si guarda datos, una base que también vive en disco. Esa máquina sos vos. La clase pivota acá.

## Por qué Canvas no alcanza

Un backend de verdad tiene tres cosas que una pestaña de Canvas no puede entregar.

Primero, un **proceso**: un programa que se queda corriendo, vivo, escuchando pedidos. No se ejecuta una vez y termina como un script: se queda esperando, y mientras esté vivo, responde.

Segundo, un **puerto**: una dirección numerada donde ese proceso escucha, típicamente algo como `localhost:8000`. El cliente le pega a esa dirección y el servidor contesta. Sin puerto no hay forma de que algo de afuera te hable.

Tercero, un **sistema de archivos** real para guardar la base. El archivo `.db` de SQLite vive en una carpeta de tu disco; queda ahí cuando cerrás la terminal y sigue ahí mañana. Una pestaña no te da eso: cuando la cerrás, el estado se evapora.

Las pestañas de sandbox son geniales para mostrar UIs; son un mal lugar para correr un servidor.

## Por qué CLI antes que IDE

Para este primer contacto con tooling de IA local vamos a usar **Claude Code**, una herramienta de IA que corre en la terminal, no un IDE gráfico.

La razón principal es transparencia. En la terminal ves cada archivo que la IA crea y cada comando que ejecuta: `pip install`, `uvicorn ...`, `sqlite3 app.db`. Nada se esconde detrás de una UI con paneles que aparecen y desaparecen.

La segunda razón es foco didáctico. En estas ocho semanas el alumno está aprendiendo a *dirigir* a una IA, no a manejar autocompletes y menús contextuales. La terminal es la mínima superficie posible: un prompt, una respuesta, un comando que se ejecuta.

La tercera razón es continuidad. La próxima clase, semana 4, sube a IDEs AI-native como Cursor o Antigravity. Acá nos quedamos en CLI por didáctica, no por capacidad. La IA no es menos potente en la terminal; es menos *vistosa*, que es exactamente lo que queremos para empezar.

## El stack del demo

El demo de la próxima clase usa un stack chico y mainstream.

**Python** es el lenguaje. Es el más común para backend en cursos introductorios y muy fuerte para el ecosistema de IA y data, así que lo que aprendas acá te sirve para los temas que vienen después.

**FastAPI** es el framework: define rutas, schemas y devuelve respuestas con muy poca ceremonia. Encaja con el "contrato" que vimos en el archivo 04: lo que dictás se parece a lo que termina escrito, así que el código no esconde la intención.

**SQLite** es la base. Vive en un archivo, sin un servidor de DB aparte que haya que instalar. Suficiente para el demo y para muchas apps reales en producción.

**Uvicorn** es el servidor ASGI: el programa que corre tu app FastAPI y la deja escuchando en un puerto.

**venv** es la herramienta de Python para aislar dependencias por proyecto, sin pisar lo que tenés instalado a nivel sistema.

## Otras stacks que vas a encontrar

FastAPI no es la única opción. La idea de mencionar otras es ubicarla en un mapa, no enseñarlas.

**Node + Express** corre JavaScript en el servidor; popular en startups donde el front-end ya manda. Compartir lenguaje entre cliente y servidor es atractivo para un equipo chico.

**Django** y **Flask** son las alternativas Python históricas. Django es opinionated y trae todo: ORM, admin, migraciones. Flask es minimal y te lo vas armando pieza por pieza, parecido en filosofía a FastAPI pero sin schemas tipados de fábrica.

**Go** con frameworks como Gin o Fiber produce servidores compilados, rápidos y con muy buen consumo de memoria. Popular para infra.

**Ruby on Rails** lleva la convención sobre configuración al extremo: si seguís las reglas, escribís muy poco código. Sigue vivo en muchas apps reales.

El punto no es aprenderlas. Es saber que **FastAPI** es *una elección*. La elegimos por tipos modernos, schemas integrados, documentación automática y baja ceremonia.

## Tarea para semana 4

Dejá la máquina lista antes de la próxima clase.

1. Tener instalado **Python** 3.11 o superior. Verificá con `python --version`.
2. Tener instalado **Node.js** 18 o superior. Claude Code corre sobre Node, así que es requisito. Verificá con `node --version`.
3. Instalar **Claude Code** desde npm:
   ```
   npm install -g @anthropic-ai/claude-code
   ```
4. Verificar la instalación. El comando `claude --version` tiene que responder con un número.
5. Si algo falla, avisar por el canal del curso *antes* de la clase. No queremos perder tiempo de semana 4 instalando herramientas.

## Vago vs. específico

```
armame el backend
```

```
creá un proyecto Python con venv y FastAPI:
- estructura: app/main.py (FastAPI), app/db.py (SQLite), requirements.txt
- dependencias: fastapi, uvicorn, pydantic
- una ruta GET /health que devuelva { "status": "ok" }
- arrancalo con: uvicorn app.main:app --reload
- mostrame en pantalla la URL y los logs
```

Los dos pueden producir algo que arranca; un LLM resuelve cualquiera. La diferencia vive en los tres ejes de siempre. Determinismo: el específico fija estructura, dependencias y comando de arranque, así que dos generaciones distintas convergen en proyectos casi idénticos en vez de inventar tres layouts. Iteración: si el primer intento no compila, pedís cambiar una pieza concreta sin reescribir el contexto. Auditoría: leés lo generado contra la lista de la izquierda y ves rápido qué falta o qué sobra.
