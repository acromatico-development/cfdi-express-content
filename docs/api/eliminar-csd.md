# Eliminar CSD

> `DELETE https://api.cfdi.express/v1/merchants/{id}/csd` — Elimina el CSD de un comercio. El comercio no se borra y los CFDIs ya timbrados no cambian, pero no puede timbrar ni cancelar hasta que suba un CSD nuevo.

Disponible a partir de la versión **v1.34.0** de la API.

URL de esta página: https://cfdi.express/docs/api/eliminar-csd
Autenticación: requiere API key (`Authorization: Bearer sk_test_…` o `sk_live_…`)

Quitar el certificado no da de baja al comercio. Las facturas, los complementos de pago y las nóminas que ya están timbrados siguen igual. Sin CSD, ese comercio no puede timbrar ni cancelar. Para volver a hacerlo, sube un CSD nuevo.

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

El comercio se mantiene y los CFDIs ya timbrados no cambian. Hasta que haya un CSD nuevo, ese comercio no puede timbrar ni cancelar.

## Errores

| Status | code | Cuándo aparece |
|---|---|---|
| TODO | `documents_in_flight` | Hay facturas, pagos o nóminas timbrándose o con cancelación pendiente. |

## Notas

En el dashboard, el menú de acciones del comercio incluye «Eliminar CSD», con diálogo de confirmación.

## Relacionados

- [POST /v1/merchants/{id}/csd](https://cfdi.express/docs/api/subir-csd) — Subir CSD
- [PUT /v1/merchants/{id}/csd](https://cfdi.express/docs/api/reemplazar-csd) — Reemplazar CSD
- [GET /v1/merchants/{id}/csd](https://cfdi.express/docs/api/obtener-csd) — Consultar CSD

---

Referencia completa: https://cfdi.express/docs/api · OpenAPI: https://api.cfdi.express/openapi.json · Llaves: https://dash.cfdi.express
