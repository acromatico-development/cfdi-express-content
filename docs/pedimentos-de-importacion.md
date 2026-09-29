# Pedimentos de importación

Si vendes de primera mano mercancía importada, el número de pedimento va en el concepto de la factura (información aduanera del CFDI). En CFDI Express lo guardas en el producto o en la variante, y la factura lo toma de ahí.

Estos campos solo aparecen si el proveedor de facturación de la tienda es **CFDI Express**. Si la tienda sigue en Facturama, la sección y la columna no se muestran. No dependen de un plan en particular.

Si no ves **Pedimentos de importación** en el producto, actualiza la app de CFDI Express.

El número de ejemplo de esta guía (`25  47  3807  5001234`) es el que muestra el formulario. Sirve para ver la forma. No es un pedimento publicado por el SAT.

## Qué es y quién lo necesita

El pedimento es el que amparó la importación del bien. El texto de ayuda del producto lo dice así: si vendes de primera mano mercancía importada, los números se incluyen en cada factura de ese producto.

En una reventa —el producto ya se vendió antes en México— ese dato no corresponde. La app no decide si tu venta es de primera mano: si guardas un número válido, lo manda en el concepto.

Pide al cliente sus **datos fiscales** (RFC, nombre o razón social, régimen fiscal, código postal y uso de CFDI), igual que en cualquier otra factura. La guía del comprobante está en [facturación desde el admin](/docs/facturacion-desde-admin-shopify).

## Cómo se combinan producto, variante y factura

En cada concepto gana **una sola lista**. No se mezclan.

1. **La factura**, si en esa orden cambiaste la celda respecto a lo que se había cargado. Vale solo para ese timbrado. No se escribe de vuelta al producto. Si dejas la celda vacía, esa factura sale sin pedimentos.
2. **La variante**, si tiene al menos un pedimento válido. La lista del producto no se usa.
3. **El producto**, si la variante no tiene.

Se envían los de la lista ganadora, hasta 100, y se descarta el repetido (se queda la primera vez).

La fila de **envío** no lleva pedimentos.

## En el producto

1. Abre **Productos** en CFDI Express.
2. Entra al producto. La pantalla se llama **Configuración Producto: {título}**.
3. En la misma sección del código SAT, la unidad, el IEPS y el IVA está el bloque **Pedimentos de importación**. Si ya hay valores, un badge dice `1 pedimento` o `{n} pedimentos`.
4. En **Números de pedimento** escribe uno por línea o separados por comas. El detalle del campo dice: `Uno por línea o separados por comas. 15 dígitos: año, aduana, patente y consecutivo. Cuando se agoten las piezas de un pedimento, reemplázalo por el nuevo.`
5. Al salir del campo, si todos son válidos, el texto se reescribe a un pedimento por línea, ya en el formato de 21 caracteres.
6. Guarda con la barra de Shopify. La app no pone un botón propio de guardar. El aviso de éxito es **Códigos SAT actualizados**.

<!-- TODO: captura — bloque Pedimentos de importación en Configuración Producto -->
![TODO: Pedimentos de importación en el producto](TODO-screenshot)
<!-- ENDTODO -->

**Limpiar códigos** borra también los pedimentos. El aviso es **Códigos SAT eliminados**.

Si alguna variante tiene lista propia, un banner avisa: `Estas variantes tienen sus propios pedimentos y se usan en lugar de los del producto: {Variante} ({n}), …. Puedes editarlos desde la variante en Shopify.` Ese aviso revisa las primeras 100 variantes.

La tabla de **Productos** dentro de la app no tiene columna de pedimentos. En el admin de Shopify, el campo del producto se puede usar como filtro. El de la variante, no.

Los códigos SAT de esa misma pantalla están en [Códigos del SAT por producto](/docs/codigos-sat-producto). Para muchos productos, usa el CSV de [carga masiva](/docs/carga-masiva-productos-csv).

## En la variante

La variante no se edita en CFDI Express. Se edita en el admin de Shopify, en la variante, en el campo **Pedimentos de importación**.

La descripción de ese campo dice: si se llenan, reemplazan los del producto en la factura CFDI. El ejemplo es el mismo: `25  47  3807  5001234`.

El CSV de la app escribe el producto, no la variante.

## En la factura de una orden

La pantalla es **CFDI para orden {nombre}**, sección **Formulario de Facturación**, tabla **Productos**.

La columna **Pedimentos** aparece solo si algún concepto ya trae pedimentos (del producto, de la variante o de un CFDI ya timbrado). Si nadie tiene, la columna no está: desde esta pantalla no se agregan por primera vez. Captúralos antes en el producto o en la variante.

Cuando la columna sí está:

- Es un campo de una sola línea. El título visible es el de la columna. El placeholder es `25  47  3807  5001234`.
- Lo que escribas ahí, si cambia lo que se había cargado, se usa en ese timbrado.
- La celda de envío queda vacía.
- Con un CFDI activo el campo se deshabilita y muestra lo que se timbró, separado por coma.
- **Facturar al Publico en General** y **Facturar al Publico en General Extranjero** no borran los pedimentos: vuelven a cargar los del producto o la variante.

La barra de esa pantalla dice **Facturar** y **Descartar**. El resto del flujo está en [facturación desde el admin](/docs/facturacion-desde-admin-shopify).

<!-- TODO: captura — columna Pedimentos en la tabla Productos de CFDI para orden -->
![TODO: Columna Pedimentos en la factura](TODO-screenshot)
<!-- ENDTODO -->

Un artículo sin producto ni variante solo puede llevar pedimento si la columna ya está visible y lo escribes en esa fila.

Una línea reembolsada o en cero pesos no se factura, así que tampoco lleva pedimento.

## Carga masiva (CSV)

En **Productos de {razón social}**, el botón **Importar códigos** abre el modal **Importar códigos SAT**.

El archivo lleva las columnas `sku`, `codigo`, `unidad`, `ieps_enabled`, `ieps_rate`, `iva_override`, `iva_rate` y `pedimentos`. Todas excepto `sku` son opcionales. Puedes partir del botón **Descargar ejemplo CSV**.

Para `pedimentos`:

- Separa varios con punto y coma (`;`).
- La celda **reemplaza** la lista del producto. No la fusiona.
- Una celda vacía **borra** los pedimentos guardados de ese producto.
- Si el archivo no trae la columna, no se modifican.
- El CSV busca el producto por SKU. Escribe el producto, no la variante. Dos filas con SKUs de variantes del mismo producto se pisan entre sí.

Si una celda de pedimentos es inválida, el resto de la fila sí se guarda, la lista de pedimentos no se toca y el SKU sale en el correo de resultados. El correo se titula `CFDI Express - Actualización masiva de productos completada`.

El paso a paso del CSV está en [Carga masiva de productos](/docs/carga-masiva-productos-csv).

## Otros flujos

En estos flujos el pedimento sale del producto o de la variante. El formulario no tiene un campo para editarlo.

| Dónde | Qué pasa con el pedimento |
|---|---|
| [Thank You Page](/docs/facturacion-checkout-thank-you-page) y [estado de la orden](/docs/facturacion-pagina-estado-de-orden) (**Formulario de autofacturación**) | Se incluyen los del producto o la variante. |
| Bloque [Formulario de Facturación](/docs/formulario-de-facturacion-theme-block) | Igual. |
| [POS](/docs/facturacion-pos-punto-de-venta-shopify): **CFDI / Factura** (después de la venta) y **CFDI / Factura (Órdenes)** | Igual. |
| [Público en general](/docs/facturacion-al-publico-en-general-manual-y-automatica), incluida la factura global | Siguen en cada concepto. No hay un concepto único de “global”. |
| [Shopify Flow](/docs/uso-shopify-flow-automatizaciones-cfdi-express), acción **Crear CFDI** | Los incluye. La acción no tiene campo para editarlos. |
| [Sidekick](/docs/facturacion-con-sidekick-ia), **Generar factura CFDI** | Abre el mismo formulario del admin y rellena datos fiscales. Si el producto ya los tiene, salen al oprimir **Facturar**. |

## Notas de crédito de la app

La nota de crédito que emites en la orden, y la acción de Flow **Crear Nota de Crédito**, no llevan pedimento. Van en un solo concepto genérico (clave `84111506`, unidad `ACT`).

El detalle del egreso está en [Notas de crédito](/docs/notas-de-credito).

## Formato que acepta la app

Cada pedimento son **15 dígitos**: año (2), aduana (2), patente (4) y consecutivo (7).

Puedes escribirlos pegados, con espacios o con guiones. Entre un pedimento y otro sirven la coma, el punto y coma, `|` o un salto de línea. `25 47 3807 5001234` y `254738075001234` son el mismo ejemplo.

Lo que se guarda y se envía es siempre la forma de **21 caracteres**, con dos espacios entre grupos:

```text
25  47  3807  5001234
```

Máximo **100** por concepto. Un número repetido se descarta.

Si el texto no es válido, o si pasas de 100, no se guarda:

- En el producto: `No son pedimentos válidos: {valores}. Cada pedimento lleva 15 dígitos (año, aduana, patente y consecutivo), por ejemplo 25  47  3807  5001234.` También puede salir `Un concepto admite máximo 100 pedimentos.` El toast dice **Revisa los pedimentos**.
- En la factura: `No válido: {valores}` o `Máximo 100 pedimentos por concepto`. El toast dice: `Revisa los pedimentos: cada uno lleva 15 dígitos (año, aduana, patente y consecutivo).`
- En el CSV: `Pedimentos no válidos: {valores} (15 dígitos: año, aduana, patente y consecutivo)` o `Máximo 100 pedimentos por producto`.

La app no revisa que la aduana o la patente existan en los catálogos del SAT. Si el SAT rechaza el timbrado y el detalle ya contiene la palabra “pedimento”, se agrega: `Verifica que la aduana y la patente del pedimento sean correctas (catálogos del SAT); puedes corregirlo en el producto o en la tabla de la factura.`

Si un valor guardado no es un pedimento válido, al armar la factura ese valor se omite.

## Qué sale en el CFDI

La app manda los pedimentos del concepto. En el XML, cada uno queda en el concepto como información aduanera: nodo `InformacionAduanera`, atributo `NumeroPedimento`, en los 21 caracteres. Hay un nodo por pedimento.

Ese mismo número se imprime en el PDF de la factura.

## Preguntas frecuentes

### ¿Puedo capturar el pedimento por primera vez al facturar la orden?

Solo si la columna **Pedimentos** ya está visible, porque algún concepto ya trae pedimentos. Si el producto y la variante están vacíos, la columna no aparece.

### ¿El de la variante se suma al del producto?

No. Si la variante tiene al menos uno válido, se usa solo esa lista.

### ¿La factura global también los lleva?

Sí, en cada concepto que los tenga. Los botones de público en general no los limpian.

### ¿La nota de crédito del app los incluye?

No. Ni desde la orden ni con la acción **Crear Nota de Crédito** de Flow.

### ¿Hay un costo distinto?

No hay un plan aparte para pedimentos. Se timbra el mismo CFDI. Los planes están en [Planes y precios](/docs/planes-y-precios).

## También por API

El mismo dato se puede mandar por la API o por el servidor MCP, en el concepto. Esa guía es para quien integra: [Factura con pedimento en CFDI 4.0 por API](/blog/factura-con-pedimento-cfdi-express-api).

En la API, la nota de crédito sí puede llevar pedimentos en sus conceptos. En la app, la nota de crédito no los lleva.
