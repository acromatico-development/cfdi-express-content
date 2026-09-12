---
title: "Webhooks CFDI: notificaciones en tiempo real en CFDI Express API"
description: "Deja de hacer polling: CFDI Express API envía webhooks firmados (invoice.stamped, cancelaciones, complementos de pago y nómina). Crea el endpoint y verifica CFDI-Signature."
image: "https://videos.acromatico.dev/api/images/assets/04989632-9039-468c-be2e-0558d456a0b9.png"
author: "Rafael González"
date: "2026-09-12"
keywords: "webhooks CFDI, webhooks API facturación México, CFDI Express webhooks, eventos invoice.stamped, notificaciones CFDI API, CFDI-Signature, webhook endpoints facturación, evitar polling CFDI, complementos de pago webhooks, nómina webhooks"
---
# Lanzamos webhooks en CFDI Express API: deja de hacer polling

Si ya integraste la [API de CFDI Express](https://cfdi.express/api), conoces el flujo: mandas `POST /v1/invoices`, llega el XML y el PDF, y tu sistema sigue con lo suyo. El problema aparece **después** del request: ¿el SAT ya timbró? ¿la cancelación ya tiene acuse? ¿el complemento de pago ya quedó sellado?

Hasta hoy, la respuesta típica era **polling**: consultar otra vez, y otra. Eso gasta rate limit y retrasa a tu ERP.

Por eso agregamos **webhooks de salida** a la API: CFDI Express hace un `POST` firmado a tu URL cuando cambia el estado de una factura, un complemento de pago o un recibo de nómina. Tú respondes `200` y sigues trabajando.

Esto es la **API pública** (`https://api.cfdi.express`), no los webhooks de una app de Shopify. Si buscas “webhooks CFDI” para un ERP, un marketplace o tu backend, este es el camino.

## Por qué un webhook gana al polling

- **Te enteras cuando pasa**, no cuando te toca preguntar. Un `invoice.stamped` llega en cuanto el timbre existe; no esperas al siguiente cron.
- **Menos requests, menos 429.** La API limita a 300 requests por minuto por llave. Un worker que pregunta cada segundo por cada factura se come ese cupo en silencio.
- **El flujo PPD/REP y la nómina no son un solo request.** Timbras la factura, luego el complemento, a veces cancelas. Cada paso dispara su propio evento (`payment.*`, `nomina.*`). Encadenar eso con polling es frágil.
- **Los fallos también se notifican.** `invoice.stamp_failed` (y los equivalentes de pago y nómina) te evitan asumir que “si no hay UUID, reintento a ciegas”.
- **Hay bitácora y reintento.** Si tu servidor se cayó, no perdiste el evento: lo ves en las entregas y lo vuelves a mandar.

Si estás evaluando la API por primera vez, el anuncio de lanzamiento está en [Lanzamos CFDI Express API](/blog/lanzamiento-cfdi-express-api). Este post asume que ya tienes (o vas a crear) una llave en [dash.cfdi.express](https://dash.cfdi.express).

## Qué eventos puedes suscribir

El catálogo vivo lo obtienes con `GET /v1/webhook_event_types`. Hoy la API publica estos tipos (el enum de OpenAPI):

| Evento | Recurso | Para qué lo usas |
| --- | --- | --- |
| `invoice.stamped` | factura | Guardar UUID, XML/PDF y marcar la orden como facturada |
| `invoice.stamp_failed` | factura | Alertar, no reintentar a ciegas y revisar el rechazo |
| `invoice.cancelled` | factura | Actualizar el estatus y guardar el acuse |
| `payment.stamped` | complemento de pago (REP) | Registrar la parcialidad cuando el SAT ya lo selló |
| `payment.stamp_failed` | complemento de pago | Detectar un REP que no pasó |
| `payment.cancelled` | complemento de pago | Restaurar el flujo de saldos en tu sistema |
| `nomina.stamped` | recibo de nómina | Confirmar el CFDI de nómina timbrado |
| `nomina.stamp_failed` | recibo de nómina | Enterarte si el timbrado de nómina falló |
| `nomina.cancelled` | recibo de nómina | Reflejar la cancelación en tu nómina |

Al crear el endpoint eliges de 1 a 9 eventos (duplicados se colapsan). No tienes que suscribirte a todos.

Además, `POST /v1/webhook_endpoints/{id}/test` te manda un evento especial `webhook_endpoint.test` — no forma parte del catálogo de suscripción; sirve para probar que tu URL responde.

Cada entrega lleva un cuerpo con esta forma:

```json
{
  "id": "…",
  "object": "event",
  "type": "invoice.stamped",
  "livemode": false,
  "created": 1778700000,
  "data": {}
}
```

`data` trae el recurso del evento. Las URLs de archivos se firman **por intento** y en el detalle guardado de la entrega se omiten.

## Paso a paso: de cero a tu primer webhook

### 1. Crea el endpoint

Con tu llave `sk_test_` (sandbox) o `sk_live_` (producción):

```bash
curl https://api.cfdi.express/v1/webhook_endpoints \
  -H "Authorization: Bearer sk_test_..." \
  -H "Content-Type: application/json" \
  -d '{
    "url": "https://tu-sistema.com/webhooks/cfdi",
    "description": "ERP producción",
    "events": [
      "invoice.stamped",
      "invoice.stamp_failed",
      "invoice.cancelled",
      "payment.stamped",
      "payment.stamp_failed",
      "payment.cancelled"
    ]
  }'
```

Reglas que sí están en OpenAPI y te evitan un `400`:

- En **live** la URL tiene que ser **HTTPS**. En test-mode se acepta `http://`.
- No se aceptan hosts privados ni loopback.
- La URL va hasta 2048 caracteres; la descripción, hasta 200.

La respuesta `201` es un objeto `webhook_endpoint` con `id`, `livemode`, `url`, `events` y `status` (`enabled` / `disabled`).

### 2. Guarda el `whsec_…` en ese instante

El campo `secret` **solo aparece** al crear el endpoint y al rotar el secreto. OpenAPI lo dice claro: *store it; it cannot be retrieved again*. Empieza con `whsec_`. Si lo pierdes, rota — no hay un `GET` que te lo vuelva a mostrar.

### 3. Verifica `CFDI-Signature` en cada POST

Cada entrega llega como `POST` firmado a tu URL. El header a validar es **`CFDI-Signature`**, con el `whsec_…` que guardaste.

El OpenAPI no publica aquí el algoritmo byte a byte (apunta a la guía de webhooks). No copies un HMAC de otro proveedor: abre las [docs interactivas](https://api.cfdi.express/docs), sección Webhooks, y verifica **antes** de parsear JSON o de tocar tu base.

En alto nivel:

```js
// Node — esqueleto. El detalle de CFDI-Signature: https://api.cfdi.express/docs
export async function POST(request) {
  const signature = request.headers.get("CFDI-Signature");
  const deliveryId = request.headers.get("CFDI-Delivery");
  const rawBody = await request.text();
  if (!signature) return new Response("missing CFDI-Signature", { status: 400 });

  // Verifica la firma con el whsec_ (y el anterior, si rotaste).
  // Parsea solo si es válida. Idempotencia: event.id o CFDI-Delivery.
  const event = JSON.parse(rawBody);
  if (await alreadyProcessed(event.id, deliveryId)) {
    return new Response("ok", { status: 200 });
  }

  switch (event.type) {
    case "invoice.stamped":
      await markInvoiceStamped(event.data);
      break;
    case "invoice.stamp_failed":
      await alertStampFailed(event.data);
      break;
    case "invoice.cancelled":
      await markInvoiceCancelled(event.data);
      break;
    case "payment.stamped":
    case "payment.stamp_failed":
    case "payment.cancelled":
      await handlePaymentEvent(event);
      break;
    case "nomina.stamped":
    case "nomina.stamp_failed":
    case "nomina.cancelled":
      await handleNominaEvent(event);
      break;
    default:
      break; // incluye webhook_endpoint.test
  }
  return new Response("ok", { status: 200 });
}
```

Dos headers que sí nombra la spec:

- **`CFDI-Signature`**: la firma de esa entrega. Si rotas el secreto, durante **24 horas** las entregas se firman con el secreto nuevo **y** el anterior, para que cambies el verificador sin tirar eventos.
- **`CFDI-Delivery`**: id de la entrega. Junto con `event.id`, es tu llave de idempotencia. `POST /v1/webhook_deliveries/{id}/retry` puede reenviar incluso una entrega que ya fue `succeeded` — tu handler tiene que aguantar el replay.

Responde rápido con `200`. Si el trabajo tarda (bajar XML, pegarle a un ERP), encola y contesta. El test usa un timeout de **10 s** en un solo intento.

### 4. Prueba con `sk_test_` antes de ir a live

```bash
curl -X POST https://api.cfdi.express/v1/webhook_endpoints/{id}/test \
  -H "Authorization: Bearer sk_test_..."
```

Te regresa la `webhook_delivery` con status HTTP, latencia y el primer KB de tu respuesta. **También funciona si el endpoint está `disabled`**, así puedes validar la URL sin abrir el fuego.

Usa `sk_test_` contra sandbox y `sk_live_` en producción. Cada endpoint trae `livemode`: no mezcles un URL de staging con una llave live.

### 5. Revisa entregas, reintenta y rota el secreto

Operación del día a día, todos con Bearer token:

| Acción | Endpoint |
| --- | --- |
| Listar endpoints | `GET /v1/webhook_endpoints` |
| Ver uno | `GET /v1/webhook_endpoints/{id}` |
| Cambiar URL, eventos, descripción o `enabled`/`disabled` | `PATCH /v1/webhook_endpoints/{id}` |
| Borrar (corta entregas y reintentos al instante; el log se sigue leyendo por id) | `DELETE /v1/webhook_endpoints/{id}` |
| Entregas de un endpoint (las más nuevas primero; filtro `status`) | `GET /v1/webhook_endpoints/{id}/deliveries` |
| Detalle de una entrega (incluye el body del evento) | `GET /v1/webhook_deliveries/{id}` |
| Reencolar una entrega `failed` o `pending` (también una `succeeded`) | `POST /v1/webhook_deliveries/{id}/retry` → `202` |
| Rotar el `whsec_…` | `POST /v1/webhook_endpoints/{id}/rotate_secret` |

Los status de entrega son `pending`, `succeeded` y `failed`. La lista usa el mismo cursor que el resto de la API (`limit`, `starting_after`, `has_more`).

Si tu URL falla de forma seguida, el endpoint puede pasar a `disabled` con `disabledReason: consecutive_failures` (también puedes deshabilitarlo tú: `user`). Volver a `enabled` después de **72 h** de fallos resetea los contadores y retoma las entregas pendientes.

## Cómo se ve en un flujo real

1. Tu checkout llama `POST /v1/invoices` con `Idempotency-Key`.
2. El worker de CFDI Express timbra ante el SAT.
3. Si sale bien, tu URL recibe `invoice.stamped` y guardas UUID + archivos. Si el SAT o el PAC rechazan, llega `invoice.stamp_failed`.
4. En una PPD, cada `POST /v1/invoices/{id}/payments` termina en `payment.stamped` o `payment.stamp_failed`.
5. Una cancelación (`POST /v1/invoices/{id}/cancel` o el equivalente de pago/nómina) dispara `*.cancelled`.

Sin un cron que pregunte “¿ya?”. Si tu servidor no contestó, tienes `retry` y la bitácora.

## Empieza hoy

1. Crea tu cuenta (o entra) en [dash.cfdi.express](https://dash.cfdi.express) y copia una llave `sk_test_`.
2. Declara tu URL con `POST /v1/webhook_endpoints` y **guarda el `whsec_…`**.
3. Implementa la verificación de `CFDI-Signature` según las [docs de la API](https://api.cfdi.express/docs) y haz `POST .../test`.
4. Cuando el test llegue en verde, cambia a `sk_live_` y una URL HTTPS de producción.

¿Aún no tienes la integración de timbrado? Empieza por la [landing de la API](https://cfdi.express/api) y el post de [lanzamiento](/blog/lanzamiento-cfdi-express-api). Si quieres revisarlo en una llamada: [agenda una demo](https://cal.com/team/acromatico-development/cfdi-express) o escribe a [hola@cfdi.express](mailto:hola@cfdi.express).

El SAT no espera a tu cron. Con webhooks, tu backend tampoco.
