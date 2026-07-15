# Modo Flow en CFDI Express para usar tu propio PAC de Facturación

CFDI Express normalmente opera en "Modo PAC": nosotros timbramos tus CFDIs usando el Certificado de Sello Digital (CSD) que cargaste y facturamos a través de Facturama (nuestro PAC). El "Modo Flow" es la alternativa para comercios que ya cuentan con su propio PAC o sistema de facturación y quieren mantener el control directo del timbrado.

## ¿Qué hace el Modo Flow?

En Modo Flow, CFDI Express **no timbra** el CFDI.Únicamente se encarga de:

- Presentar los formularios de facturación a tus clientes (Thank You Page, página de estado de orden, POS, formulario de tema, admin).
- Recopilar y validar los datos fiscales del cliente (RFC, razón social, régimen, código postal, uso de CFDI, método de pago, dirección completa opcional, CSF opcional).
- Enviar esos datos fiscales a tu propio PAC / ERP / API a través de una automatización de **Shopify Flow** (disparador `cfdi-flow-mode`).
- Confirmarle al cliente por correo que sus datos fueron recibidos ("Datos Fiscales Enviados a {tu negocio} para facturación").
- Guardar un registro del CFDI con estatus "flow" y crear el metaobject correspondiente en la orden para que sepas que ese pedido ya fue solicitado para facturar.

El timbrado real lo ejecuta tu PAC, usando la automatización que tú diseñes en Shopify Flow. CFDI Express actúa como el puente que recolecta los datos y dispara tu flujo.

## ¿Cuándo usar Modo Flow?

- Ya tienes un PAC con el que quieres seguir timbrando (Facturama, Edicom, SW, Finkok, un ERP propio, etc.).
- Necesitas que los CFDIs se generen en tu sistema contable/ERP y no en una herramienta externa.
- Tu negocio maneja reglas de facturación particulares que procesa tu propio sistema antes de timbrar.
- Quieres evitar el cargo por facturación de CFDI Express (Modo Flow igual consume uso de facturación, pero no usas Facturama de nuestro lado).

## Configuración del Modo Flow

1. Durante el [onboarding](/docs/onboarding), selecciona **"Modo Flow"** en lugar de "Modo PAC".

<!-- TODO: captura — selector "Modo PAC / Modo Flow" en el onboarding -->
![TODO: Selector de modo PAC / Flow en onboarding](TODO-screenshot)
<!-- ENDTODO -->
2. Completa el formulario. En Modo Flow **no se requiere CSD** (no se pide `.cer`, `.key` ni contraseña), porque CFDI Express no timbrará; tú lo harás con tu propia CSD cargada en tu PAC.
3. Sí se requiere tu RFC, razón social, régimen fiscal, código postal y logo (estos datos se comparten con el cliente y se incluyen en el disparador de Flow).
4. Una vez finalizado el onboarding, puedes ajustar la configuración en cualquier momento desde [Configuraciones](/docs/configuraciones).

## El disparador `cfdi-flow-mode`

Cada vez que un cliente (o tú mismo desde el admin) envía una solicitud de facturación en Modo Flow, CFDI Express dispara el evento **`cfdi-flow-mode`** en Shopify Flow. El disparador incluye:

- Referencia de la orden (`order_reference`).
- Datos fiscales del receptor: RFC, razón social, régimen fiscal, código postal, dirección completa (si se habilitó), uso CFDI, método de pago, tipo de pago (PUE/PPD), correo.
- URL de la Constancia de Situación Fiscal (CSF) si el cliente la cargó y tú habilitaste `enableCsfUpload`.
- ID de la orden y el UUID interno del CFDI en estatus "flow".

Tu automatización de Flow escucha este disparador y, con una acción HTTP / conector, envía los datos a tu PAC para que éste timbre el CFDI. Revisa el detalle de triggers y acciones en [Shopify Flow](/docs/uso-shopify-flow-automatizaciones-cfdi-express).

## Estatus de los CFDIs en Modo Flow

En tu [lista de órdenes](/docs/ordenes-estatus) y en la pantalla de facturación, los CFDIs generados en Modo Flow aparecen con estatus **"flow"** (no "Facturado"). Esto indica que los datos fiscales fueron enviados a tu PAC pero el timbrado lo realiza tu sistema. Cuando tu PAC timbra, puedes actualizar el estatus del CFDI manualmente o mediante una automatización que regrese la información al app.

## Diferencias clave: Modo PAC vs Modo Flow

| Aspecto | Modo PAC | Modo Flow |
|---|---|---|
| ¿Quién timbra? | CFDI Express (Facturama) | Tu propio PAC / ERP |
| ¿Se requiere CSD (.cer/.key)? | Sí, cargado en la app | No |
| Cargo por CFDI timbrado | Sí, conforme al plan | Sí, se registra el uso igualmente |
| Acción del botón "Generar CFDI" | Timbra inmediatamente | Dispara el disparador `cfdi-flow-mode` |
| Correo al cliente | CFDI timbrado adjunto (PDF+XML) | Confirmación "Datos Fiscales Enviados" (sin adjunto) |
| Cancelación | CFDI Express cancela ante el SAT | Tu PAC la realiza (CFDI Express marca cancelado) |

## Pasar de Modo Flow a Modo PAC (o viceversa)

Puedes cambiar de modo cuando quieras desde [Configuraciones](/docs/configuraciones) con el toggle "¿Ya tienes un PAC de facturación?":

- **Switch apagado = Modo PAC**: deberás cargar tu CSD y los CFDIs se timbrarán con nosotros.
- **Switch encendido = Modo Flow**: no se requiere CSD y los CFDIs se enviarán a tu PAC vía Flow.

<!-- TODO: captura — toggle "¿Ya tienes un PAC de facturación?" dentro de Configuraciones -->
![TODO: Toggle PAC/Flow en Configuraciones](TODO-screenshot)
<!-- ENDTODO -->

> Nota: cambiar de modo no afecta los CFDIs ya timbrados. Sólo aplica a las nuevas solicitudes de facturación.

Si requieres ayuda configurando la automatización de Flow para tu PAC específico, contáctanos a través de [Acromático Development](https://acromatico.dev).

<!-- TODO: grabar tutorial de Modo Flow (usar tu propio PAC) -->

### Video Tutorial

- Embed:
  <iframe width="100%" style="aspect-ratio:16/9;" src="https://www.youtube.com/embed/TODO_VIDEO_ID?si=TODO" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
- Link:
  https://www.youtube.com/watch?v=TODO_VIDEO_ID
<!-- ENDTODO -->