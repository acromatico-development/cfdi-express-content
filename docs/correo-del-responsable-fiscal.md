---
title: "Correo del responsable fiscal: copia de los avisos de tu tienda para tu contador"
description: "Agrega en CFDI Express el correo de tu contador o de tu equipo de facturación para que reciba los avisos de la tienda: vencimiento de tu CSD, errores y cancelaciones de la factura global, actualización masiva de productos y cambios en tu facturación."
keywords: "correo del contador, responsable fiscal, avisos de la tienda, copia de correos, CSD por vencer, aviso vencimiento CSD, factura global errores, actualización masiva de productos, CFDI Express, Shopify"
---
# Correo del responsable fiscal

URL de esta página: https://cfdi.express/docs/correo-del-responsable-fiscal

Muchas veces quien instala CFDI Express no es quien lleva la facturación. Con el **correo del responsable fiscal**, tu contador o tu equipo de facturación recibe una copia de los avisos de la tienda sin que tengas que reenviárselos.

Es un campo opcional de la app de Shopify. Funciona igual en Modo PAC y en [Modo Flow](/docs/modo-flow-cfdi-express-para-usar-tu-propio-pac-de-facturacion).

## Dónde se configura

El campo está junto a los datos fiscales:

- En el [onboarding](/docs/onboarding), al dar de alta tu tienda. No es obligatorio: puedes terminar el alta sin llenarlo.
- En [Configuraciones](/docs/configuraciones), en la misma sección de datos fiscales. Ahí lo agregas después, lo cambias o lo borras.

La etiqueta del campo es **Correo del contador o equipo responsable de CFDI Express (opcional)**, con este texto de ayuda:

> Le enviaremos los avisos de la tienda (vencimiento de tu CSD, errores de la factura global, actualización masiva de productos y cambios en tu facturación), además de al correo de la tienda. Puede ser un alias, por ejemplo facturacion@tuempresa.com. Déjalo vacío si no lo necesitas.

Para guardarlo:

1. Escribe **un solo correo**. No acepta una lista.
2. Guarda los datos fiscales.

Si el correo está mal escrito, la app no lo guarda y muestra: «El correo no tiene un formato válido (ej: contador@tuempresa.com)». Si lo dejas vacío, se guarda sin responsable.

Te recomendamos usar un alias del equipo (por ejemplo `facturacion@tuempresa.com`) en lugar del correo personal de una persona. Así los avisos no se pierden si cambia tu contador o alguien deja el equipo.

## Qué avisos recibe

Cuando el campo tiene un correo, ese correo recibe copia de estos avisos de la tienda:

| Aviso | Cuándo llega | A quién le llegaba ya |
|---|---|---|
| [CSD por caducar o vencido](/docs/avisos-de-csd-por-caducar) (solo Modo PAC) | Antes y después de que venza tu Certificado de Sello Digital. La cadencia y el contenido están en la guía de CSD. Este aviso es, sobre todo, para tu contador. | Al correo de la tienda en Shopify. |
| Error de la [factura global](/docs/facturacion-global-mensual) | Cuando una corrida de facturación global termina con error. | Al correo de contacto de la tienda en Shopify o, si no hay, al correo de la tienda. |
| Cancelación de [factura global](/docs/facturacion-global-mensual#cancelar-una-factura-global) | Cuando termina la cancelación de una factura global o de una corrida, con el resumen. | Igual que el anterior. |
| [Actualización masiva de productos](/docs/carga-masiva-productos-csv) | Cuando termina la carga masiva por CSV. Asunto: «CFDI Express - Actualización masiva de productos completada». | Al correo de contacto de la tienda en Shopify. |
| Cambio de proveedor de timbrado | Cuando el equipo de CFDI Express cambia el proveedor con el que se timbran las facturas de tu tienda. Es un correo de bienvenida que te explica el cambio. | Al correo de la tienda en Shopify. |

El responsable **se suma** a quien ya recibía cada aviso; no lo sustituye. Si es el mismo correo, el aviso llega una sola vez.

Si dejas el campo vacío, todo sigue igual: cada aviso llega solo a quien le llegaba antes.

## Qué no recibe

- **Los correos a tus clientes.** El CFDI, el complemento de pago, la nota de crédito y la confirmación de datos fiscales en Modo Flow siguen llegando solo al cliente, y si el cliente responde, la respuesta va al contacto de tu tienda en Shopify, como siempre. El detalle está en [Notificaciones por correo](/docs/notificaciones-por-correo).

## Preguntas frecuentes

### ¿Es obligatorio?

No. Puedes terminar el onboarding y usar la app sin llenarlo.

### ¿Puedo poner varios correos?

No, es un solo correo. Si varias personas necesitan los avisos, usa un alias o una lista de distribución de tu dominio que les llegue a todos.

### ¿Mi contador recibe las facturas de mis clientes?

No. Solo recibe los avisos de la tienda de la tabla de arriba, incluidos los del vencimiento de tu CSD. Para mandarle las facturas globales, usa [Shopify Flow](/docs/enviar-facturas-globales-al-contador-shopify-flow).

### ¿Cómo dejo de enviarle avisos?

Borra el correo en [Configuraciones](/docs/configuraciones) y guarda. Los avisos vuelven a llegar solo al correo de siempre.

### ¿Está en el dashboard o en la API?

No. El campo existe solo en la app de Shopify.
