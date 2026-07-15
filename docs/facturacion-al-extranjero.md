# Facturación al Extranjero (RFC XEXX010101000)

CFDI Express soporta facturar a **clientes extranjeros** usando el RFC genérico del SAT para operaciones con extranjeros: **`XEXX010101000`**. Esto aplica cuando un cliente fuera de México solicita factura y el SAT exige IVA al **0%** en esos comprobantes.

## Cuándo usarlo

- El cliente no es residente fiscal en México (no tiene RFC mexicano).
- La venta se realiza a un cliente extranjero y requieres emitir un CFDI que refleje IVA 0% por concepto de exportación / no residente.

> El RFC `XEXX010101000` es el valor genérico oficial del SAT para **operaciones con extranjeros** (distinto de `XAXX010101000` que es público en general).

## Cómo facturar al extranjero

### Desde el Admin de Shopify
1. Abre la orden en [facturación del admin](/docs/facturacion-desde-admin-shopify).
2. Pulsa **"Facturar al Público en General Extranjero"** (botón para extranjeros). Esto prellenará:
    - RFC: `XEXX010101000`
    - Razón social del cliente
    - Régimen, código postal y uso CFDI correspondientes

<!-- TODO: captura — botón "Facturar al Público en General Extranjero" en la pantalla de facturación -->
![TODO: Botón Facturación al Extranjero](TODO-screenshot)
<!-- ENDTODO -->
3. CFDI Express habilita los **checkboxes "IVA 0%" por línea**. Marca los productos que deben ir a 0% (típicamente todos para exportación). El app recalcula los importes y genera la previsualización vía `/api/invoice-data`.

<!-- TODO: captura — checkboxes "IVA 0%" por línea activados en una orden extranjera -->
![TODO: Checkboxes IVA 0% por línea](TODO-screenshot)
<!-- ENDTODO -->
4. Verifica que los impuestos queden correctos y haz clic en **Generar CFDI**.

### Desde los formularios del cliente (Thank You Page / estado de orden / formulario de tema)
El formulario del cliente detecta el RFC `XEXX010101000` y despliega automáticamente las opciones de **IVA 0% por producto**, igual que en el admin. El cliente marca los productos que apliquen y envía.

## Detalle técnico: IVA 0% por línea

- El cálculo del IVA por producto **se respeta por separado** (no es "todo o nada"). Puedes mezclar líneas con IVA al 16% y líneas con IVA 0% en el mismo CFDI si así lo requieres.
- El campo `ivaZeroItems` viaja desde el formulario (cliente o admin) hasta la previsualización y el timbrado, recalculando los importes de base, IVA traslados y totales en [Modo PAC](/docs/configuraciones).
- Si un producto ya tiene configurado un IVA especial en [Códigos del SAT por producto](/docs/codigos-sat-producto), la sobreescritura de 0% del extranjero toma precedencia para esa emisión.

## Notas

- La facturación al extranjero también dispara el disparador `cfdi-created` y envía el correo con el CFDI al cliente (salvo que `skipCfdiEmail` esté activo).
- Si requieres configurar el uso de CFDI o régimen específico que tu SAT/contador recomienda para exportaciones, puedes ajustarlo en la emisión; consulta con tu contador el régimen correcto para tu caso.

<!-- TODO: grabar tutorial de Facturación al Extranjero (IVA 0%) -->

### Video Tutorial

- Embed:
  <iframe width="100%" style="aspect-ratio:16/9;" src="https://www.youtube.com/embed/TODO_VIDEO_ID?si=TODO" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
- Link:
  https://www.youtube.com/watch?v=TODO_VIDEO_ID
<!-- ENDTODO -->