# Cancelación de CFDI y Acuse de Cancelación

CFDI Express permite **cancelar** un CFDI ya timbrado ante el SAT, y descargar el **acuse de cancelación** (la constancia que emite el SAT). Puedes re-facturar la orden después si es necesario.

## Cancelar un CFDI

1. Abre la orden desde la [lista de órdenes](/docs/ordenes-estatus) o [facturación del admin](/docs/facturacion-desde-admin-shopify).
2. Cuando la orden ya está facturada, aparece el botón **Cancelar Factura** (arriba a la derecha de la pantalla de facturación).

<!-- TODO: captura — botón "Cancelar Factura" en la pantalla de facturación -->
![TODO: Botón Cancelar Factura](TODO-screenshot)
3. Selecciona el **motivo de cancelación** del SAT:
    - `01` — Comprobante emitido con errores con relación
    - `02` — Comprobante emitido sin errores con relación
    - `03` — No se llevó a cabo la operación
    - `04` — Operación nominativa relacionada con la facturación global
4. Confirma la cancelación. CFDI Express:
    - Cancela el CFDI en Facturama ante el SAT.
    - Marca el CFDI con estatus **Cancelado** y actualiza el metafield `$app.facturado` a falso.
    - Ajusta el uso facturado (decrementa el cargo por uso del CFDI cancelado).
    - Dispara el disparador `cfdi-deleted` en [Shopify Flow](/docs/uso-shopify-flow-automatizaciones-cfdi-express).

## Restricciones al cancelar

- **No se puede cancelar** un CFDI que tenga **[Complementos de Pago](/docs/complementos-de-pago) activos**. Cancela primero todos sus complementos y luego cancela el CFDI original.
- La cancelación es definitiva ante el SAT; queda registrado en el historically de la app.

## Acuse de Cancelación

Después de cancelar, CFDI Express te permite descargar el **acuse de cancelación**, que es el documento PDF y XML que el SAT devuelve para confirmar la cancelación.

1. En la pantalla de facturación de la orden, localiza el CFDI cancelado.
2. Pulsa **Descargar Acuse**. CFDI Express obtiene el acuse desde Facturama y te entrega el PDF (y XML) para tu constancia contable.

<!-- TODO: captura — botón "Descargar Acuse" sobre un CFDI cancelado -->
![TODO: Botón Descargar Acuse](TODO-screenshot)

El acuse se puede descargar cuando quieras desde la misma orden, incluso tiempo después de la cancelación.

## Re-facturar la orden

Después de cancelar, la orden queda libre para volver a facturarse (por ejemplo, con datos fiscales distintos). Simplemente ejecuta de nuevo el flujo de facturación (desde el [admin](/docs/facturacion-desde-admin-shopify) o desde los formularios del cliente). El nuevo CFDI tendrá un UUID distinto.

> Nota: la cancelación **no envía un correo automático** al cliente. Si necesitas avisarle, descarga el acuse y reenvíaselo, o automatiza un correo con el disparador `cfdi-deleted` usando [Shopify Flow](/docs/uso-shopify-flow-automatizaciones-cfdi-express).

### Video Tutorial

<!-- TODO: grabar tutorial de Cancelación y Acuse -->

- Embed:
  <iframe width="100%" style="aspect-ratio:16/9;" src="https://www.youtube.com/embed/TODO_VIDEO_ID?si=TODO" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
- Link:
  https://www.youtube.com/watch?v=TODO_VIDEO_ID