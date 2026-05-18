## Prompt inicial

Actúa como un Arquitecto de APIs experto. Necesito diseñar un contrato OpenAPI 3.1 en formato YAML para un sistema llamado "Text to Ambience" (un generador de ambientes musicales para locales comerciales basado en descripciones de texto e IA).

El diseño debe seguir una estructura jerárquica basada en un modelo de "Curación", donde el recurso principal es "Ambience" (el ambiente creado) y el recurso anidado o hijo son los "Filters" (los parámetros técnicos de audio y reglas que determinan qué canciones aplican).

Por favor, genera el archivo openapi.yaml cumpliendo estrictamente con las siguientes especificaciones:

1. Versión de OpenAPI: 3.1.0
2. Recursos y Atributos:
   - Ambience: { id (string/uuid), prompt_text (string), name (string) }
   - Filter: { id (string/uuid), parameter_name (string, ej: "energy", "acousticness", "genre"), value_type (string, ej: "range", "keyword"), min_value (number, opcional), max_value (number, opcional), keyword_value (string, opcional) }

3. Endpoints requeridos (Jerarquía visible):
   - GET /ambiences (Listar ambientes generados)
   - POST /ambiences (Crear un nuevo ambiente desde un prompt)
   - GET /ambiences/{id}/filters (Listar los filtros técnicos que componen ese ambiente)
   - POST /ambiences/{id}/filters (Agregar/Modificar una regla o filtro al ambiente)
   - DELETE /ambiences/{id}/filters/{filterId} (Remover un filtro del ambiente)

4. Contrato específico para el POST de la jerarquía (POST /ambiences/{id}/filters):
   - Entrada (Body JSON): `parameter_name` (requerido), `value_type` (requerido), `min_value` (opcional), `max_value` (opcional), `keyword_value` (opcional).
   - Respuesta 201 (Created): Devuelve el objeto Filter completo con su ID asignado.
   - Respuesta 400 (Bad Request): Si faltan campos requeridos o si los tipos de datos no coinciden. Devuelve un objeto `{ error: string }`.
   - Respuesta 404 (Not Found): Si el `id` del Ambience en el path no existe. Devuelve un objeto `{ error: string }`.

5. Buenas prácticas de OpenAPI 3.1:
   - Usa la sección `components/schemas` para definir de forma tipada y reutilizable los modelos `Ambience`, `Filter` y el objeto de error.
   - Aplica validaciones básicas (`type`, `required`, `format`) donde corresponda.
   - Utiliza `$ref` para referenciar los schemas.

Entrega solo el código YAML limpio dentro de un bloque de código, asegurándote de que la jerarquía de los paths sea perfectamente visible para su posterior edición.


## La entrega es aceptable, no hice correcciones, al leer el contrato en editor.swagger.io se veia bien, solo se me ocurrió pedirle algo más

Está perfecto, creo que no tiene errores, 
ahora quiero implementar un rate limit, necesito un response de tipo 429 too many requests, en el caso de que el cliente supere un límite a definir de llamados al post de creación de ambientes. 

## En este caso me dio demasiados headers del response 200 y 429, y prefiero ocultar cierta información de cara a los límites por una decisión de diseño, dejé solo uno.