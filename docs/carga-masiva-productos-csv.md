# Carga Masiva de Productos (CSV)

Para tiendas con muchos productos, CFDI Express permite asignar los [códigos del SAT](/docs/codigos-sat-producto) (código de producto, unidad, IEPS e IVA especial) a varios productos a la vez mediante una **carga masiva por CSV**. Esto evita editar producto por producto.

## Formato del CSV

El archivo debe tener encabezados y una fila por producto, identificado por su **SKU**:

| Columna | Descripción |
|---|---|
| `sku` | SKU del producto en Shopify (obligatorio) |
| `codigo` | Código de producto/servicio del SAT |
| `unidad` | Código de unidad del SAT |
| `ieps_enabled` | `true` / `false` — si el producto lleva IEPS |
| `ieps_rate` | Tasa de IEPS (ej. `0.08` para 8%) |
| `iva_override` | `true` / `false` — si usas IVA especial |
| `iva_rate` | Tasa de IVA especial (ej. `0.0` para 0%, `0.16` para 16%) |
| `pedimentos` | Números de pedimento del producto, separados con `;`. Opcional. Ver [Pedimentos de importación](/docs/pedimentos-de-importacion) |

<!-- TODO: captura — ejemplo del archivo CSV abierto en una hoja de cálculo con las columnas requeridas -->
![TODO: Ejemplo de archivo CSV con las columnas](TODO-screenshot)
<!-- ENDTODO -->

> Si dejas vacías las columnas de códigos por producto, conservará el valor que ya tenga configurado. La columna `pedimentos` es distinta: una celda vacía borra los pedimentos del producto; si el archivo no trae esa columna, no se modifican. El detalle está en [Pedimentos de importación](/docs/pedimentos-de-importacion). Si quieres **limpiar** todos los metafields de un producto, usa la acción `clear` en la página individual del producto ([Códigos del SAT por producto](/docs/codigos-sat-producto)). **Limpiar códigos** también borra los pedimentos.

## Metacampos de producto

El código, la unidad, el IEPS y el IVA del producto están en metacampos del namespace `$app`. En tu tienda puedes leerlos y editarlos. Las columnas del CSV siguen el formato de la tabla de arriba. Estas son las validaciones de esos metacampos. La de 8 dígitos aplica solo al **Código SAT** del producto.

| Clave | Nombre | Validación |
|---|---|---|
| `codigo_sat` | Código SAT | 8 dígitos (`^[0-9]{8}$`). Vacío = predeterminada de la tienda |
| `unidad_sat` | Unidad SAT | 1 a 3 caracteres A-Z o 0-9 (por ejemplo `H87`, `E48`). Vacío = predeterminada de la tienda |
| `ieps_aplica` | Aplica IEPS | Booleano |
| `tasa_ieps` | Tasa de IEPS | Porcentaje, mínimo 0. Solo si Aplica IEPS |
| `iva_personalizado` | IVA personalizado | Booleano |
| `tasa_iva` | Tasa de IVA | Porcentaje de 0 a 100 (0, 8 o 16). Solo si IVA personalizado |

Pedido, cliente, migración y Shopify Flow están en [Códigos del SAT por producto](/docs/codigos-sat-producto).

## Cómo subir el CSV

1. Entra a CFDI Express → **Productos**.
2. Localiza el botón de **carga masiva (CSV)**.

<!-- TODO: captura — botón de carga masiva CSV en la página de Productos -->
![TODO: Botón de carga masiva CSV](TODO-screenshot)
<!-- ENDTODO -->

3. Sube tu archivo `.csv` con las columnas indicadas.
4. CFDI Express encola un **trabajo en segundo plano** (BullMQ) que irá actualizando los metafields de los productos uno por uno mediante GraphQL de Shopify.

> Por el volumen, la actualización **no es instantánea**; puedes cerrar la app y regresar más tarde. Cada producto actualizado pasa a tener sus códigos disponibles para el timbrado.

Cuando termina, te llega el correo «CFDI Express - Actualización masiva de productos completada» al correo de contacto de tu tienda en Shopify. Si llenaste el [correo del responsable fiscal](/docs/correo-del-responsable-fiscal), también le llega copia.

## Recomendaciones

- Usa los **códigos predeterminados** de [Configuraciones](/docs/configuraciones) para los productos genéricos y sólo sobreescribe los específicos en el CSV (menos trabajo).
- Verifica los SKUs; si un SKU no existe en tu tienda, esa fila no tendrá efecto.
- Para tasas, usa formato decimal con punto (ej. `0.08`), no porcentaje.

## Limpieza por producto

Si en algún momento requieres quitar todos los metafields de SAT de un producto en específico, abre el producto desde **Productos** y usa la acción **"clear"**, que eliminará todos los metafields asociados de ese producto.

<!-- TODO: grabar tutorial de Carga Masiva de Productos (CSV) -->

### Video Tutorial

- Embed:
  <iframe width="100%" style="aspect-ratio:16/9;" src="https://www.youtube.com/embed/TODO_VIDEO_ID?si=TODO" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
- Link:
  https://www.youtube.com/watch?v=TODO_VIDEO_ID
<!-- ENDTODO -->