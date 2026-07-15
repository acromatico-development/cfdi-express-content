# Configuraciones

La página de **Configuraciones** del app de CFDI Express concentra todos los datos de tu negocio y las opciones de comportamiento de facturación. Para abrirla, entra al app desde el Admin de Shopify y selecciona **Configuraciones** en la navegación.

## Datos fiscales del emisor

- **Razón Social**
- **RFC** (validado con el patrón del SAT)
- **Régimen Fiscal** (catálogo del SAT; debe coincidir con tu Constancia de Situación Fiscal)
- **Código Postal** (el de tu CSF)
- **Logo** (se muestra en el CFDI y en los correos al cliente)

<!-- TODO: captura — sección "Datos fiscales del emisor" con logo cargado -->
![TODO: Datos fiscales del emisor](TODO-screenshot)

## Dirección fiscal completa (opcional)

Activa el toggle **"Enable full address"** (`enableFullAddress`) para capturar los campos de dirección que aparecerán como dirección del emisor en el CFDI:

- Calle, Número Exterior, Número Interior, Colonia, Ciudad, Municipio, Estado

Si está apagado, el CFDI se emite sólo con el código postal del emisor.

## Certificado de Sello Digital (CSD) — solo Modo PAC

En [Modo PAC](/docs/modo-flow-cfdi-express-para-usar-tu-propio-pac-de-facturacion) debes cargar:

- Archivo **`.cer`**
- Archivo **`.key`**
- **Contraseña** del CSD

Estos se registran/cambian ante Facturama al guardar. Si cambias tu CSD, vuelve a cargarlo aquí.

## Selección de modo: PAC o Flow

El toggle **"¿Ya tienes un PAC de facturación?"** controla el modo:

- **Apagado = Modo PAC:** CFDI Express timbra con tu CSD vía Facturama. Se requiere CSD.
- **Encendido = Modo Flow:** No se requiere CSD. Los datos fiscales se envían a tu propio PAC a través de Shopify Flow. Ver [Modo Flow](/docs/modo-flow-cfdi-express-para-usar-tu-propio-pac-de-facturacion).

<!-- TODO: captura — toggle "¿Ya tienes un PAC de facturación?" en Configuraciones -->
![TODO: Toggle Modo PAC / Modo Flow](TODO-screenshot)

## Códigos del SAT predeterminados

Puedes definir los códigos que se usarán cuando un producto no tenga código asignado:

- **Código de producto** predeterminado (por defecto `53101600`)
- **Código de unidad** predeterminado (por defecto `H87`)
- **Código de envío** predeterminado (por defecto `78102200`)
- **Código de unidad de envío** predeterminado (por defecto `E48`)

Cada uno cuenta con autocompletado desde los catálogos de Facturama. Para sobreescribirlos por producto individual, ver [Códigos del SAT por producto](/docs/codigos-sat-producto).

## Reglas de periodo de facturación

Estas reglas controlan **cuándo** tus clientes pueden auto-facturarse desde la Thank You Page o la página de estado de orden:

- **Sin límite (`no_limit`)** — el cliente puede facturar en cualquier momento.
- **Fin del mes de compra (`end_of_month`)** — el cliente puede facturar hasta el último día del mes en que se hizo la compra.
- **Días después de la compra (`days_after_purchase`)** — defines un número de días (por defecto 30) dentro de los cuales el cliente puede facturar. Pasado ese tiempo, el formulario se desactiva.

> Si el periodo vence, el cliente debe solicitar la factura por otra vía y tú puedes facturar manualmente desde el [admin](/docs/facturacion-desde-admin-shopify) o automatizarlo con [Shopify Flow](/docs/uso-shopify-flow-automatizaciones-cfdi-express).

<!-- TODO: captura — reglas de periodo de facturación (no_limit / end_of_month / days_after_purchase) -->
![TODO: Reglas de periodo de facturación](TODO-screenshot)

## Notificaciones

- **`skipCfdiEmail`** — si lo activas, CFDI Express **no** envía el correo al cliente con el CFDI adjunto. Útil cuando tú reenvías la factura desde tu propio sistema.
- **`skipCfdiWhatsApp`** — toggle para desactivar notificaciones por WhatsApp (cuando esté disponible).

Para el detalle de los correos que se envían, ver [Notificaciones por correo](/docs/notificaciones-por-correo).

## Solicitud de Constancia de Situación Fiscal (CSF)

El toggle **`enableCsfUpload`** agrega un campo de carga de archivo (CSF) a los formularios de facturación públicos (Thank You Page, página de estado de orden, formulario de tema). La CSF subida se guarda y su URL se incluye en el disparador `cfdi-flow-mode` y el evento `cfdi-created`.

## Gestión del plan

Desde Configuraciones también puedes ver tu **plan actual** y, si corresponde, cambiarte de plan (Standard, Plus, Pro, Enterprise). Para tarifas y límites, ver [Planes y Precios](/docs/planes-y-precios).

## Guardar cambios

- Los **datos fiscales, CSD, logo, dirección y modo** se guardan al pulsar guardar (vía `/api/csd` en Modo PAC o `/api/csd/flow` en Modo Flow).
- Las **reglas de periodo y notificaciones** se guardan en automático vía `/api/invoicing-settings`.

### Video Tutorial

<!-- TODO: grabar tutorial de Configuraciones -->

- Embed:
  <iframe width="100%" style="aspect-ratio:16/9;" src="https://www.youtube.com/embed/TODO_VIDEO_ID?si=TODO" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
- Link:
  https://www.youtube.com/watch?v=TODO_VIDEO_ID