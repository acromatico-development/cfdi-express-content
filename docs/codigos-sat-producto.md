# Configuración de códigos del SAT por producto y IEPS

Para poder generar facturas 4.0, el SAT requiere que cada producto tenga un código de producto y un código de unidad. Estos códigos se pueden consultar desde la siguiente liga en el portal del SAT: [Códigos de Producto](http://pys.sat.gob.mx/PyS/catPyS.aspx).

En CFDI Express, existe un código de producto y un código de unidad predeterminado que puedes editar desde la página de configuraciones.

Si se requiere, se puede configurar un código de producto y un código de unidad por producto que sobreescriba el código predeterminado, esto se puede realizar desde la página de productos -> Seleccionas el producto a editar. Dentro de la página de edición del producto, puedes encontrar los campos para configurar el código de producto, el código de unidad, si el producto lleva IEPS, la tasa de IEPS y si el producto lleva IVA especial con la tasa de IVA especial.

En esa misma pantalla, el bloque **Pedimentos de importación** guarda el número de pedimento de mercancía importada de primera mano. La guía está en [Pedimentos de importación](/docs/pedimentos-de-importacion).

En la lista de **Productos** de la app, el filtro **Clave SAT** tiene estas opciones: **Todos**, **Sin clave**, **Con clave** y **Clave específica** (código de 8 dígitos).

En el admin de Shopify, la ficha de un producto ya guardado muestra el bloque **CFDI Express · Claves SAT**. Si el admin está en inglés, el bloque se llama **CFDI Express · SAT codes**. Desde ahí buscas en el catálogo del SAT (mínimo 4 caracteres) y guardas el **Código SAT** y la **Unidad SAT** con la barra de guardar del admin, sin salir del producto. El bloque solo aparece en productos ya guardados.

Puedes consultar nuestro video tutorial si lo prefieres:

- Embed:
  <iframe width="100%" style="aspect-ratio:16/9;" src="https://www.youtube.com/embed/DPETjTdVMJ0?si=jlPNS7Zck4tAsQTO" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
- Link:
  https://youtu.be/DPETjTdVMJ0?si=_N8gTa1FOCvUHkBQ

## Metacampos de facturación

### Producto

El código, la unidad, el IEPS y el IVA del producto están en metacampos del namespace `$app`. En tu tienda puedes leerlos y editarlos. La validación de 8 dígitos aplica solo al **Código SAT** del producto.

| Clave | Nombre | Validación |
|---|---|---|
| `codigo_sat` | Código SAT | 8 dígitos (`^[0-9]{8}$`). Vacío = predeterminada de la tienda |
| `unidad_sat` | Unidad SAT | 1 a 3 caracteres A-Z o 0-9 (por ejemplo `H87`, `E48`). Vacío = predeterminada de la tienda |
| `ieps_aplica` | Aplica IEPS | Booleano |
| `tasa_ieps` | Tasa de IEPS | Porcentaje, mínimo 0. Solo si Aplica IEPS |
| `iva_personalizado` | IVA personalizado | Booleano |
| `tasa_iva` | Tasa de IVA | Porcentaje de 0 a 100 (0, 8 o 16). Solo si IVA personalizado |

### Pedido

En el pedido, estos metacampos están en el namespace `$app`. Solo puedes consultarlos. No tienen validación.

- Folio de la factura
- Fecha de la factura
- Uso del CFDI
- Forma de pago
- Método de pago (PUE/PPD)
- Descarga de la factura

### Cliente

En el cliente, estos metacampos están en el namespace `cfdi_express`. No llevan validación, a propósito. Son los datos fiscales:

- RFC
- Razón social
- Régimen fiscal
- Código postal fiscal
- Calle
- Número exterior
- Número interior
- Colonia
- Municipio
- Ciudad
- Estado

## Migración y Shopify Flow

Al abrir la app, una migración automática copia productos y clientes a los campos nuevos, solo si el campo nuevo está vacío. Un código SAT inválido no se copia: se queda en el campo anterior.

Los pedidos no se copian. La app lee el valor más reciente de los dos lugares.

Shopify Flow sigue igual: sigue leyendo el namespace anterior (`$cfdiexpress`) y la app escribe en ese namespace y en los metacampos nuevos.
