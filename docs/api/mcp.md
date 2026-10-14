# Servidor MCP de CFDI Express

> Servidor Model Context Protocol para que un agente de IA (claude.ai, Claude Desktop, ChatGPT, Claude Code, Cursor, Windsurf o uno propio) timbre facturas CFDI 4.0, notas de crédito y complementos de pago, cancele CFDIs y consulte catálogos del SAT en tu cuenta de CFDI Express, con las mismas reglas y validaciones que la API REST.

`create_invoice` y `create_credit_note` aceptan `fecha` a partir de la versión **v1.36.0** de la API.

- Producción: https://api.cfdi.express/mcp — timbra CFDIs reales y consume saldo.
- Pruebas: https://api.cfdi.express/mcp/test — timbra contra el sandbox del SAT, gratis e ilimitado.
- Transporte: Streamable HTTP, sin estado.
- Guía completa: https://cfdi.express/docs/api/mcp
- Referencia REST: https://cfdi.express/docs/api
- Skill para escribir la integración REST: `npx skills add CFDI-Express/skills` (https://github.com/CFDI-Express/skills)
- Llaves y saldo: https://dash.cfdi.express

## Autenticación

- **API key** (clientes que permiten headers: Claude Code, Cursor, Windsurf, agentes propios): `Authorization: Bearer sk_test_…` o `sk_live_…`. La llave decide el modo, así que una `sk_test_` contra /mcp sigue timbrando en el sandbox.
- **OAuth 2.1** (claude.ai, Claude Desktop, conectores de ChatGPT): pegas la URL y firmas con tu cuenta del dashboard, sin manejar llaves. Aquí la ruta decide el modo: /mcp es producción y /mcp/test es pruebas.

El alcance es el mismo que el de la API REST sobre esa cuenta y ese modo. Los tokens OAuth expiran en 1 hora y se renuevan solos hasta por 30 días.

## Conectar

**claude.ai y Claude Desktop:** Configuración → Conectores → Agregar conector personalizado → pega https://api.cfdi.express/mcp (o https://api.cfdi.express/mcp/test) y firma con tu cuenta del dashboard.

**ChatGPT:** requiere plan Plus, Pro, Business o Enterprise en la app web. Settings → Apps & Connectors → activa Developer mode y agrega el servidor con la misma URL.

**Claude Code:**

```shell
claude mcp add --transport http cfdi https://api.cfdi.express/mcp \
  --header "Authorization: Bearer sk_test_..."
```

**Cursor y Windsurf (mcp.json):**

```json
{
  "mcpServers": {
    "cfdi": {
      "url": "https://api.cfdi.express/mcp",
      "headers": { "Authorization": "Bearer sk_test_..." }
    }
  }
}
```

**SDK de MCP (agentes propios):**

```ts
import { Client } from "@modelcontextprotocol/sdk/client/index.js";
import { StreamableHTTPClientTransport } from "@modelcontextprotocol/sdk/client/streamableHttp.js";

const client = new Client({ name: "mi-agente", version: "1.0.0" });

await client.connect(
  new StreamableHTTPClientTransport(new URL("https://api.cfdi.express/mcp"), {
    requestInit: {
      headers: { authorization: `Bearer ${process.env.CFDI_API_KEY}` },
    },
  }),
);
```

## Herramientas (18)

| Herramienta | Qué hace |
|---|---|
| `create_invoice` | Timbra una factura CFDI 4.0 de ingreso (PUE, PPD o global). Acepta `fecha` opcional, el mismo campo que en POST /v1/invoices: hora local de Ciudad de México, `YYYY-MM-DDTHH:mm:ss`, sin zona, hasta 70 horas atrás. Cada concepto acepta `items[].pedimentos`. Espera hasta ~20 s por el UUID; si regresa status "stamping", hay que consultar get_invoice. |
| `create_credit_note` | Timbra una nota de crédito (CFDI de egreso) por devolución, reembolso o descuento; se relaciona con relatedInvoiceId o relatedUuid. Acepta el mismo `fecha` opcional que POST /v1/credit_notes. Cada concepto acepta `items[].pedimentos`. |
| `get_invoice` | Estatus, UUID, cancelación y ligas firmadas de XML, PDF y ZIP (~15 min). También para notas de crédito. El recurso trae `fecha`: la del CFDI ya armado, la solicitada antes de eso, o null si no se conoce. |
| `list_invoices` | Lista paginada con filtros por tipo, estatus, emisor, fechas y UUID (kind=credit_note para notas de crédito). |
| `cancel_invoice` | Cancela ante el SAT con motivo 01–04 (01 exige folioSustitucion). Consume un timbre. |
| `create_payment` | Timbra un complemento de pago REP 2.0 contra una factura PPD. |
| `get_payment` | Consulta un complemento de pago con sus saldos y archivos. |
| `list_payments` | Lista los complementos de pago con filtros y cursor. |
| `cancel_payment` | Cancela un REP y restaura el saldo pendiente de la factura. |
| `create_nomina` | Timbra un recibo de nómina (CFDI tipo N + complemento Nómina 1.2), uno por empleado y periodo; los totales del SAT se calculan del lado del servidor. |
| `get_nomina` | Consulta un recibo de nómina: estatus, UUID, empleado, totales y archivos. |
| `list_nominas` | Lista los recibos de nómina con filtros por RFC del empleado, fecha de pago y estatus. |
| `cancel_nomina` | Cancela un recibo de nómina ante el SAT con motivo 01–04. Consume un timbre. |
| `list_merchants` | Los emisores de la cuenta; el agente necesita un merchantId para facturar. |
| `search_product_codes` | Búsqueda por palabra clave sobre c_ClaveProdServ, con sinónimos del SAT. |
| `search_unit_codes` | Búsqueda por palabra clave sobre c_ClaveUnidad. |
| `get_catalog` | Catálogos completos de usos de CFDI, regímenes fiscales, formas de pago y los catálogos de nómina (nomina/*). |
| `get_balance` | Saldo prepagado y timbres restantes del modo conectado. |

Las herramientas que timbran o cancelan aceptan `idempotency_key`: con un valor estable, los reintentos devuelven el documento original sin gastar otro timbre.

`create_invoice` y `create_credit_note` aceptan `items[].pedimentos`: arreglo opcional de 1 a 100 cadenas, para la mercancía importada que vendes de primera mano. Mismo contrato que la API REST. Cada valor son 15 dígitos — año (2), aduana (2), patente (4) y consecutivo (7) — con o sin espacios; se emite en InformacionAduanera en el formato de 21 caracteres del SAT, con dos espacios entre grupos. Un nodo por pedimento; los duplicados se descartan. Un agente conectado por MCP puede timbrar la factura o la nota de crédito con el pedimento de cada concepto.

`create_invoice` y `create_credit_note` aceptan `fecha` (v1.36.0), el mismo campo opcional que [POST /v1/invoices](https://cfdi.express/docs/api/timbrar-factura) y [POST /v1/credit_notes](https://cfdi.express/docs/api/crear-nota-de-credito). Es la Fecha del CFDI en hora local de Ciudad de México, `YYYY-MM-DDTHH:mm:ss`, sin zona: por ejemplo `2026-10-13T09:30:00`. CDMX es UTC-6 todo el año desde 2022. Puede ir hasta 70 horas atrás (el SAT permite 72 antes de la certificación; las 2 horas de margen son porque el timbrado es asíncrono). No puede ser futura, y el CSD del emisor tiene que haber estado vigente en esa fecha. Si no la mandas, se usa la hora de timbrado. Si queda a menos de 5 minutos del timbrado, el worker la ajusta a ese margen. Una fecha inválida, futura, de más de 70 horas o fuera de la vigencia del CSD regresa `validation_error` con `subcode` `fecha_out_of_range` y `errors[{path:'fecha'}]`. Un formato mal escrito falla la validación del cuerpo, el mismo `code`, sin ese `subcode`.

## Comportamiento

- El servidor manda instrucciones al conectarse: resolver el emisor con list_merchants (y preguntar cuál usar si hay varios), buscar claves del SAT antes de timbrar y consultar get_invoice cuando el timbrado quedó en proceso.
- Los errores traen el código estable de la API (validation_error, insufficient_credits, sat_rejected…) con punteros por campo, así que el agente corrige y reintenta solo. `fecha_out_of_range` es el `subcode` cuando `fecha` está fuera de rango o el CSD no estaba vigente.
- Los CSDs no se administran por MCP: los certificados sólo se suben y validan desde el dashboard o por la API REST.
- Rate limit: cada llamada a una herramienta consume 2 tokens del presupuesto por minuto de la cuenta. Las conexiones por OAuth comparten el bucket del usuario que firmó.

## Preguntas frecuentes

- ¿Qué es? Un servidor MCP que le da a un agente de IA 18 herramientas para facturar CFDI 4.0 y timbrar nómina en México.
- ¿Cómo pruebo sin gastar timbres? Conecta https://api.cfdi.express/mcp/test o usa una llave sk_test_.
- ¿Puedo subir mi CSD desde el agente? No: los certificados sólo se administran desde el dashboard o la API REST.
- ¿Puedo timbrar con fecha anterior? Sí, desde la v1.36.0: dile al agente la `fecha` en hora local de Ciudad de México (`YYYY-MM-DDTHH:mm:ss`), hasta 70 horas atrás. `create_invoice` y `create_credit_note` la aceptan.
- ¿Necesito Shopify? No: la API y el MCP son independientes de Shopify.
- ¿Y si quiero que el agente escriba la integración en vez de facturar por chat? Instala la skill oficial: `npx skills add CFDI-Express/skills`. El MCP factura en vivo; la skill le da el contexto para programar contra la API REST.

---

Referencia completa: https://cfdi.express/docs/api · OpenAPI: https://api.cfdi.express/openapi.json · Llaves: https://dash.cfdi.express
