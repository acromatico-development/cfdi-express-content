# Eliminar CSD

> `DELETE https://api.cfdi.express/v1/merchants/{id}/csd` — Elimina el CSD de un comercio. El comercio no se borra y los CFDIs ya timbrados no cambian. Sin CSD no puede timbrar facturas, notas de crédito, nóminas ni complementos de pago, ni cancelar CFDIs, hasta que vuelvas a subir el certificado.

Disponible a partir de la versión **v1.34.0** de la API.

URL de esta página: https://cfdi.express/docs/api/eliminar-csd
Autenticación: requiere API key (`Authorization: Bearer sk_test_…` o `sk_live_…`)

La operación borra el certificado y su llave privada cifrada, y desactiva el CSD en el PAC. Si otro comercio usa el mismo certificado, en el PAC se queda activo. No se puede deshacer: para restaurarlo, vuelve a subir el `.cer` y el `.key`.

### Parámetros de ruta

| Campo | Tipo | Requerido | Descripción |
|---|---|---|---|
| `id` | string | sí | Id del comercio. |

## Ejemplos

### cURL

```bash
curl -X DELETE https://api.cfdi.express/v1/merchants/mkq2f8v3x1t7p9d4n6c0b5rz/csd \
  -H "Authorization: Bearer sk_test_..."
```

## Respuesta

### 200 OK

Devuelve el recurso Merchant completo, con `csd` en `null`.

```json
{
  "id": "mkq2f8v3x1t7p9d4n6c0b5rz",
  "object": "merchant",
  "livemode": false,
  "rfc": "EKU9003173C9",
  "legalName": "ESCUELA KEMPER URGATE",
  "regimenFiscal": "601",
  "zip": "42501",
  "serie": "A",
  "defaultClaveProdServ": "01010101",
  "defaultClaveUnidad": "E48",
  "csd": null,
  "logoUrl": "https://files.cfdi.express/logos/mkq2f8...png?X-Amz-Expires=900&...",
  "createdAt": "2026-08-18T16:20:03.114Z"
}
```

### 400 — documentos en curso

`problem+json` con `code` `validation_error` y `subcode` `documents_in_flight`. Ocurre cuando hay facturas, pagos o nóminas en `stamping` o `cancel_pending`. Se puede reintentar cuando terminen.

```json
{
  "type": "https://api.cfdi.express/docs/errors/validation_error",
  "title": "Validation error",
  "status": 400,
  "code": "validation_error",
  "detail": "This merchant has documents still stamping or awaiting cancellation — try again once they finish",
  "subcode": "documents_in_flight"
}
```

### 404 Not Found

`code` `not_found` si el comercio no existe, si está eliminado o si no tiene CSD. En este último caso el mensaje es `This merchant has no CSD uploaded`.

```json
{
  "type": "https://api.cfdi.express/docs/errors/not_found",
  "title": "Not found",
  "status": 404,
  "code": "not_found",
  "detail": "This merchant has no CSD uploaded"
}
```

## Errores

| Status | code | Cuándo aparece |
|---|---|---|
| 400 | `validation_error` | `subcode` `documents_in_flight`. Hay facturas, pagos o nóminas en `stamping` o `cancel_pending`. Se puede reintentar cuando terminen. |
| 404 | `not_found` | El comercio no existe, está eliminado o no tiene CSD. Sin CSD, el mensaje es `This merchant has no CSD uploaded`. |

## Notas

- Borra el certificado y su llave privada cifrada, y desactiva el CSD en el PAC. Si otro comercio usa el mismo certificado, en el PAC se queda activo.
- No se puede deshacer. Para restaurarlo, vuelve a subir el `.cer` y el `.key`.
- Sin CSD, ese comercio no puede timbrar facturas, notas de crédito, nóminas ni complementos de pago, ni cancelar CFDIs.
- El comercio no se borra. Los CFDIs ya timbrados no cambian.
- A partir de la **v1.35.0**, `DELETE /v1/merchants/{id}` también borra el CSD de ese comercio. Si vuelves a crear el RFC, se restaura el mismo comercio (mismo `id`, historial y folios), sin CSD.

## Dashboard

La opción «Eliminar CSD» solo aparece en el menú de acciones si el comercio tiene CSD. El diálogo se titula «Eliminar CSD — {RFC}» y explica que se borran el certificado {serial} y su llave privada. Los botones son Cancelar y Eliminar CSD; este último va en rojo.

## v1.35.0

A partir de la versión **v1.35.0** de la API, borrar un comercio también borra su CSD, y volver a crear un RFC eliminado restaura ese mismo comercio.

### Borrar el comercio

`DELETE /v1/merchants/{id}` borra el comercio y también su CSD.

[Eliminar CSD](https://cfdi.express/docs/api/eliminar-csd) (`DELETE /v1/merchants/{id}/csd`) borra el certificado y su llave privada cifrada, y deja el comercio. Los CFDIs ya timbrados no cambian.

### Restaurar un RFC eliminado

Si vuelves a crear un RFC que habías borrado, se restaura el mismo comercio, con su `id`, su historial y sus folios. Antes, esa operación fallaba con un error 500.

El comercio vuelve sin CSD. Para timbrar hay que subir el certificado de nuevo con [POST /v1/merchants/{id}/csd](https://cfdi.express/docs/api/subir-csd). Hasta entonces no puede timbrar facturas, notas de crédito, nóminas ni complementos de pago, ni cancelar CFDIs. Qué se elimina del certificado está en [Eliminar CSD](https://cfdi.express/docs/api/eliminar-csd).

## Relacionados

- [POST /v1/merchants/{id}/csd](https://cfdi.express/docs/api/subir-csd) — Subir CSD
- [PUT /v1/merchants/{id}/csd](https://cfdi.express/docs/api/reemplazar-csd) — Reemplazar CSD
- [GET /v1/merchants/{id}/csd](https://cfdi.express/docs/api/obtener-csd) — Consultar CSD
- [DELETE /v1/merchants/{id}](https://cfdi.express/docs/api/eliminar-emisor) — Eliminar comercio
- [POST /v1/merchants](https://cfdi.express/docs/api/crear-emisor) — Crear comercio

---

Referencia completa: https://cfdi.express/docs/api · OpenAPI: https://api.cfdi.express/openapi.json · Llaves: https://dash.cfdi.express
