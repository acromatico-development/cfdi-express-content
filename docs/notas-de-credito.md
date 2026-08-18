# Notas de Crédito (Devoluciones)

Cuando un cliente devuelve mercancía o recibe un reembolso, el SAT requiere que lo ampasses con un **CFDI de egreso (Nota de Crédito)** relacionado a la factura original. CFDI Express te permite registrar una **Nota de Crédito** directamente desde la pantalla de facturación de la orden, **sin necesidad de cancelar la factura original**.

> Esto reemplaza el flujo anterior donde la única forma de reflejar una devolución era **cancelar** el CFDI de ingreso y re-facturar con un monto menor.

## Requisitos

- La orden debe tener un CFDI **activo** (timbrado) generado **por CFDI Express**.
- El proveedor de facturación debe ser **CFDI Express** (las facturas timbradas mediante Modo Flow no califican, ya que la Nota de Crédito se genera con el mismo emisor).
- El **monto total acreditado** (suma de Notas de Crédito activas) no puede exceder el total de la factura.

## Sección en la pantalla de facturación

Cuando los requisitos se cumplen, la pantalla de facturación de la orden muestra una sección **"Devoluciones (Notas de Crédito)"** con:

- Una **tabla** con las Notas de Crédito ya emitidas (folio, fecha, monto, estado: Activa/Cancelada, y acciones de Descarga y Cancelación).
- Un botón **"Devolución (Nota de Crédito)"** para abrir el formulario de nueva Nota.
- Si el total de la factura ya fue acreditado en su totalidad, el botón se reemplaza por un banner informativo: *"El total de la factura ya fue acreditado con notas de crédito."*

![Sección de Notas de Crédito](https://videos.acromatico.dev/api/images/assets/ca905a14-540e-4db5-bee3-8a7f4c6a7a30.png)

## Cómo generar una Nota de Crédito

1. Abre la orden deseada desde la [lista de órdenes](/docs/ordenes-estatus) o desde la [facturación del admin](/docs/facturacion-desde-admin-shopify).
2. En la sección **"Devoluciones (Notas de Crédito)"**, haz clic en **"Devolución (Nota de Crédito)"** para abrir el modal.
3. Completa los datos:
   - **Tipo de relación** (código del SAT):
     - `01` — Nota de crédito (reembolso o descuento)
     - `03` — Devolución de mercancía
   - **Forma de pago del reembolso** (catálogo del SAT, p. ej. `01` transferencia electrónica, `03` cheque, `15` condonación).
   - **Monto a acreditar (impuestos incluidos)**: el valor de la devolución. El campo muestra el **disponible por acreditar** (total de la factura menos el monto ya acreditado con Notas de Crédito activas).
   - **Descripción**: texto descriptivo de la devolución.
   - **Correo del receptor (opcional)**: si lo proporcionas, CFDI Express envía la Nota de Crédito por correo electrónico.
4. Haz clic en **"Generar Nota de Crédito"**. CFDI Express:
   - Timbra un **CFDI de egreso** (tipo de comprobante `E`) relacionado a la factura original, con tu CSD.
   - Genera el PDF + XML y los empaqueta en un ZIP almacenable y descargable.
   - Si indicaste un correo, envía la Nota de Crédito por email al receptor.
   - Registra la operación para efectos de [uso](/docs/planes-y-precios).

![Modal de nueva Nota de Crédito](https://videos.acromatico.dev/api/images/assets/fdb2de01-fdf9-4c32-9bd0-5a36e65cc246.png)

## Monto máximo acreditado

- La suma de los montos de todas las Notas de Crédito **activas** no puede exceder el **total de la factura** original.
- Si intentas generar una Nota cuyo monto exceda el disponible, CFDI Express muestra un error con el monto disponible por acreditar.
- Puedes generar **múltiples Notas de Crédito** parciales sobre la misma factura, siempre que el acumulado no supere el total.

## Cancelar una Nota de Crédito

1. En la sección **"Devoluciones (Notas de Crédito)"**, localiza la Nota a cancelar (estado **Activa**).
2. Haz clic en **"Cancelar"**.
3. CFDI Express cancela la Nota de Crédito ante el SAT y la marca con estado **Cancelada**.

> Al cancelar una Nota de Crédito, su monto vuelve a estar **disponible por acreditar**, por lo que puedes generar otra Nota de Crédito por ese monto.

## Cancelación de la factura bloqueada

Una factura que tiene **Notas de Crédito activas** **no puede cancelarse**, igual que la restricción vigente para [Complementos de Pago](/docs/complementos-de-pago).

- En la pantalla de facturación aparece un banner de advertencia: *"No se puede cancelar esta factura porque tiene notas de crédito activas. Cancela primero las notas de crédito antes de cancelar la factura."*
- El botón **"Cancelar CFDI"** queda deshabilitado.

Para cancelar la factura, primero cancela todas sus Notas de Crédito activas y luego el CFDI original. Ver [Cancelación de CFDI y Acuse de Cancelación](/docs/cancelacion-y-acuse).

## Descarga de la Nota de Crédito

Cada Nota de Crédito emitida tiene disponible un botón **"Descargar"** que te entrega el **ZIP con el PDF y el XML** del CFDI de egreso. Puedes descargarlo en cualquier momento desde la sección de Notas de Crédito de la orden.

<!-- TODO: captura — tabla de notas de crédito con botón de descarga -->
![TODO: Descarga de Nota de Crédito](TODO-screenshot)
<!-- ENDTODO -->

## ¿Qué cuenta como un uso?

Cada **Nota de Crédito timbrada** registra un cargo de uso igual que cualquier otro CFDI, de acuerdo a tu [plan](/docs/planes-y-precios). La **cancelación** de una Nota de Crédito ajusta el uso (decrementa el cargo, igual que las cancelaciones de CFDI de ingreso).

## Diferencia vs. Cancelación de CFDI

| | Nota de Crédito | Cancelación de CFDI |
|---|---|---|
| **Cuándo usar** | Devolución parcial o total, reembolso, descuento | Operación errónea, cliente requiere re-facturar con otros datos |
| **Efecto** | Genera un CFDI de egreso relacionado; la factura original sigue activa | El CFDI de ingreso queda cancelado ante el SAT y la orden se re-factura |
| **Monto** | Cualquier importe ≤ total de la factura (parciales permitidas) | La factura completa se cancela |
| **Re-facturación** | No requiere | Sí: la orden queda libre para facturarse de nuevo |

<!-- TODO: grabar tutorial de Notas de Crédito -->

### Video Tutorial

- Embed:
  <iframe width="100%" style="aspect-ratio:16/9;" src="https://www.youtube.com/embed/TODO_VIDEO_ID?si=TODO" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
- Link:
  https://www.youtube.com/watch?v=TODO_VIDEO_ID
<!-- ENDTODO -->
