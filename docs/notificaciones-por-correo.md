# Notificaciones por Correo

CFDI Express envía correos transaccionales al cliente en los momentos clave del proceso de facturación, usando el correo que capturó el cliente en Shopify. Los correos se envían a través de Mailgun e incluyen el logo de tu negocio y la firma "Powered by CFDI Express".

## Correos que se envían

### 1. CFDI Timbrado (Modo PAC)
Cuando se timbra un CFDI con éxito (en [Modo PAC](/docs/modo-flow-cfdi-express-para-usar-tu-propio-pac-de-facturacion)), el cliente recibe:

- **Asunto:** "Tu CFDI para la orden {Folio} ha sido generada con éxito"
- **Contenido:** HTML con tu logo y datos del CFDI.
- **Adjunto:** `factura.zip` que contiene el **PDF** y el **XML** del CFDI.

<!-- TODO: captura — vista previa del correo "Tu CFDI para la orden {Folio} ha sido generada" con logo y adjunto ZIP -->
![TODO: Vista previa del correo CFDI Timbrado](TODO-screenshot)
<!-- ENDTODO -->

> Este correo **no se envía** si activaste `skipCfdiEmail` en [Configuraciones](/docs/configuraciones) (útil cuando tú reenvías la factura desde tu propio sistema). Esto aplica también a los [Complementos de Pago](/docs/complementos-de-pago).

### 2. Datos Fiscales Enviados (Modo Flow)
En [Modo Flow](/docs/modo-flow-cfdi-express-para-usar-tu-propio-pac-de-facturacion), cuando el cliente envía sus datos fiscales, recibe:

- **Asunto:** "Datos Fiscales Enviados a {tu negocio} para facturación"
- **Contenido:** Confirmación de que sus datos fueron recibidos y que el CFDI será emitido por tu PAC.
- **Adjunto:** Ninguno (el timbrado lo realiza tu PAC).

<!-- TODO: captura — vista previa del correo "Datos Fiscales Enviados a {tu negocio}" en Modo Flow -->
![TODO: Vista previa del correo Datos Fiscales Enviados](TODO-screenshot)
<!-- ENDTODO -->

## Eventos de marketing (Klaviyo)

Además del correo transaccional, CFDI Express envía un evento de métrica **"CFDI Generated"** a Klaviyo (no bloqueante), distinguiendo entre cliente "public_client" (público en general) y "fiscal_client" (con RFC real). Esto te permite segmentar y automatizar marketing en Klaviyo sin que CFDI Express envíe correos de marketing: esa parte la controlas tú desde Klaviyo.

## Correos internos (no al cliente)

- **Formulario de contacto público** (en cfdi.express): se envía a `hola@cfdi.express` con el tipo de necesidad (conexión a ERP, migración a Shopify, nueva tienda, consultoría, integraciones, otro), verificado por hCaptcha.
- **Shop redact** (cumplimiento de privacidad de Shopify): se envía a `hola@acromati.co` con el payload cuando se elimina una tienda, como parte de los webhooks obligatorios de Shopify.

## Correos que NO se envían automáticamente

- **Cancelación de CFDI:** CFDI Express **no** envía un correo automático al cliente cuando se cancela un CFDI. La cancelación se refleja en el estatus y puedes descargar el [acuse de cancelación](/docs/cancelacion-y-acuse). Si necesitas avisar al cliente, automatiza un correo con el disparador `cfdi-deleted` usando [Shopify Flow](/docs/uso-shopify-flow-automatizaciones-cfdi-express), o reenvía el acuse manualmente.

## Resumen de toggles

| Toggle | Ubicación | Efecto |
|---|---|---|
| `skipCfdiEmail` | [Configuraciones](/docs/configuraciones) | No envía el CFDI por correo al cliente |
| `skipCfdiWhatsApp` | [Configuraciones](/docs/configuraciones) | Desactiva notificaciones por WhatsApp (cuando aplique) |