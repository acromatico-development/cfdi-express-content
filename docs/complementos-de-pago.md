# Complementos de Pago (PPD)

Cuando una orden se factura con método de pago **PPD (Pago en Parcialidades o Diferido)**, el CFDI original refleja únicamente el monto de la venta y el SAT requiere que, conforme el cliente vaya abonando, generes **Complementos de Pago (Comprobantes de Ingreso con complemento para recepción de pagos)** para amparar cada abono. CFDI Express gestiona la generación y cancelación de complementos de pago directamente desde la app.

## Requisitos

- La orden debe tener un CFDI **activo** y generado con **método de pago PPD** (si fue PUE, no corresponde complemento).
- El complemento se genera sobre el CFDI original; CFDI Express calcula el **saldo insoluto** y el **número de parcialidad** de forma automática.

<!-- TODO: captura — botón/enlace "Complementos" en la pantalla de facturación de una orden PPD -->
![TODO: Botón de acceso a Complementos de Pago](TODO-screenshot)
<!-- ENDTODO -->

## Cómo generar un Complemento de Pago

1. Abre la orden deseada desde la [lista de órdenes](/docs/ordenes-estatus) o desde [facturación del admin](/docs/facturacion-desde-admin-shopify).
2. En la pantalla de facturación, si el CFDI está activo y el tipo de pago es PPD, aparecerá un botón / enlace a **Complementos** (te lleva a `/app/complementos/:orderId`).
3. Escribe los datos del pago:
    - **Fecha de pago**
    - **Forma de pago** (efectivo, transferencia, tarjeta, etc., catálogo del SAT)
    - **Monto** del abono
4. CFDI Express calcula y muestra:
    - El **saldo anterior**
    - El **saldo insoluto** restante
    - El **número de parcialidad** correspondiente

<!-- TODO: captura — formulario de generación de complemento (fecha, forma de pago, monto, saldo insoluto, parcialidad) -->
![TODO: Formulario de nuevo Complemento de Pago](TODO-screenshot)
<!-- ENDTODO -->
5. Haz clic en **Generar Complemento**. El app:
    - Timbra el complemento en Facturama (PAC) usando tu CSD.
    - Empaqueta el PDF + XML en un ZIP y lo almacena.
    - Crea un metaobject `$app:complemento_pago` y actualiza los metafields de la orden.
    - Envía por correo el ZIP del complemento al cliente (a menos que `skipCfdiEmail` esté activo, ver [Configuraciones](/docs/configuraciones)).
    - Dispara el disparador `cfdi-created` (y el evento en Klaviyo).

## Cómo cancelar un Complemento de Pago

1. Entra a la pantalla de Complementos de la orden.
2. Localiza el complemento a cancelar y haz clic en **Cancelar**.
3. CFDI Express cancela el complemento ante el SAT (Facturama) y elimina/descalifica sus metafields.

> Importante: **no puedes cancelar un CFDI principal** mientras tenga complementos de pago activos. Primero cancela todos sus complementos asociados y luego cancela el CFDI original.

## Campos guardados en cada complemento

Cada complemento registra: UUID, folio, fecha de pago, forma de pago, monto, número de parcialidad, saldo anterior, saldo insoluto, correo del receptor y URL de descarga del ZIP. Estos datos también quedan disponibles en el [reporte](/docs/reportes) exportable.

## Complementos en Modo Flow

En [Modo Flow](/docs/modo-flow-cfdi-express-para-usar-tu-propio-pac-de-facturacion), la generación de complementos igual dispara el flujo `cfdi-flow-mode` con los datos del pago, para que tu PAC timbre el complemento. El control del saldo insoluto y parcialidad se basa en los complementos ya registrados en CFDI Express.

<!-- TODO: grabar tutorial de Complementos de Pago (PPD) -->

### Video Tutorial

- Embed:
  <iframe width="100%" style="aspect-ratio:16/9;" src="https://www.youtube.com/embed/TODO_VIDEO_ID?si=TODO" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
- Link:
  https://www.youtube.com/watch?v=TODO_VIDEO_ID
<!-- ENDTODO -->