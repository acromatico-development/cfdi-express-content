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
    - Timbra el complemento ante el SAT usando tu CSD.
    - Empaqueta el PDF + XML en un ZIP y lo almacena.
    - Crea un metaobject `$app:complemento_pago` y actualiza los metafields de la orden.
    - Envía por correo el ZIP del complemento al cliente (a menos que `skipCfdiEmail` esté activo, ver [Configuraciones](/docs/configuraciones)).
    - Dispara el disparador `cfdi-created` (y el evento en Klaviyo).

## Cómo cancelar un Complemento de Pago

1. Entra a la pantalla de Complementos de la orden.
2. Localiza el complemento a cancelar y haz clic en **Cancelar**.
3. CFDI Express cancela el complemento ante el SAT y elimina/descalifica sus metafields.

> Importante: **no puedes cancelar un CFDI principal** mientras tenga complementos de pago activos. Primero cancela todos sus complementos asociados y luego cancela el CFDI original.

## Complemento de una factura en dólares

Desde la app **3.28.0**, si la factura PPD es en dólares el complemento sale en dólares. El monto del abono se captura en dólares. El tipo de cambio es el de la **fecha del pago** (el FIX de Banxico de ese día). El paso a paso de la factura en USD está en [Cómo facturar en dólares desde Shopify](/blog/facturar-en-dolares-shopify).

## Un REP para varias facturas PPD

Un solo complemento de recepción de pagos (**REP**, Pagos 2.0) puede cubrir varias facturas PPD. En `POST /v1/payments`, el arreglo `documents[]` indica cada factura y el importe de su abono. Todas deben ser del mismo emisor, del mismo RFC receptor y de la misma moneda. El arreglo admite hasta 100 facturas. La parcialidad se numera por factura.

Cada REP consume **1 timbre**. Cancelarlo restaura el saldo de cada factura relacionada. Una factura ligada a un REP vigente no se puede cancelar: primero cancela el complemento.

La referencia del endpoint está en [Timbrar un complemento de pago de varias facturas](https://cfdi.express/docs/api/timbrar-complemento-de-pago-varias-facturas). El paso a paso está en [Cómo emitir un REP para varias facturas PPD](https://cfdi.express/blog/emitir-rep-varias-facturas-ppd).

Si la factura PPD está en dólares u otra moneda distinta de MXN, el REP se timbra en esa misma moneda. En `POST /v1/invoices/{id}/payments` y en `POST /v1/payments`, `exchangeRate` es el TipoCambioP de la fecha de pago: pesos mexicanos por 1 unidad, máximo 6 decimales, y es obligatorio cuando la factura no está en MXN. Para USD se usa el FIX de Banxico publicado en el Diario Oficial de la Federación de la fecha de pago. El pago (`monto` e `importePagado`) va en la moneda de la factura y los Totales se convierten a MXN. Pagar en una moneda distinta a la de la factura sigue sin estar disponible. La respuesta incluye `exchangeRate`. La referencia de un solo documento está en [Timbrar un complemento de pago](https://cfdi.express/docs/api/timbrar-complemento-de-pago).

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