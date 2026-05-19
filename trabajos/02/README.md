# Text to Ambience API

Contrato OpenAPI 3.1 para un sistema de gestión de ambientes musicales.

## Recursos

- `Ambience`: ambiente musical generado.
- `Filter`: reglas técnicas asociadas al ambiente.

## Endpoints

GET    /ambiences  
POST   /ambiences  

GET    /ambiences/{id}/filters  
POST   /ambiences/{id}/filters  

DELETE /ambiences/{id}/filters/{filterId}

## Implementación

El contrato utiliza:

- `components/schemas`
- referencias con `$ref`
- validaciones básicas (`type`, `required`, `format`)
- estructura jerárquica entre recursos

## Cambio adicional

Se agregó soporte para rate limiting en:

POST /ambiences

documentando la respuesta HTTP 429 Too Many Requests y exponiendo únicamente el header `Retry-After` para evitar publicar detalles internos de la política de límites.