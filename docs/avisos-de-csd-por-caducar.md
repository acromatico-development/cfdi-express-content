---
title: "Avisos de CSD por caducar: banner y correos de la app de Shopify"
description: "CFDI Express te avisa antes de que venza tu Certificado de Sello Digital (CSD): un banner en la app y correos 30, 15, 7, 3 y 1 días antes, y un recordatorio diario durante dos semanas si ya venció."
keywords: "CSD por vencer, CSD vencido, certificado de sello digital vencido, renovar CSD, CertiSAT Web, aviso vencimiento CSD, no puedo timbrar facturas, CFDI Express, Shopify"
---
# Avisos de CSD por caducar

URL de esta página: https://cfdi.express/docs/avisos-de-csd-por-caducar

Tu **Certificado de Sello Digital (CSD)** tiene fecha de vencimiento. El día que vence, CFDI Express ya no puede timbrar facturas, notas de crédito ni complementos de pago de tu tienda (tampoco desde Shopify Flow) hasta que subas uno vigente.

Para que no te tome por sorpresa, la app te avisa de dos formas:

- Un **banner** en la app, desde 30 días antes del vencimiento.
- **Correos** antes y después del vencimiento: al [correo del responsable fiscal](/docs/correo-del-responsable-fiscal), si lo guardaste, y al correo de tu tienda en Shopify.

Aplica a las tiendas en [Modo PAC](/docs/modo-flow-cfdi-express-para-usar-tu-propio-pac-de-facturacion), que son las que tienen un CSD cargado. En Modo Flow no hay CSD, así que no hay banner ni correos de vigencia.

## El banner en la app

Cuando a tu CSD le quedan **30 días o menos**, la app muestra un banner arriba de todas las pantallas. No se puede cerrar: se quita cuando subes un CSD vigente al que le quedan más de 30 días.

**Si tu CSD está por vencer** (banner de advertencia):

> **Tu CSD vence en 12 días (18 de mayo de 2027)**
>
> Cuando venza, CFDI Express no podrá timbrar facturas, notas de crédito ni complementos de pago. Tramita un CSD nuevo en el portal del SAT y súbelo en Configuraciones → Datos fiscales.

Los últimos días el encabezado dice «Tu CSD vence mañana» o «Tu CSD vence hoy», con la fecha.

**Si tu CSD ya venció** (banner crítico, en rojo):

> **Tu CSD venció el 18 de mayo de 2027**
>
> CFDI Express no puede timbrar facturas, notas de crédito ni complementos de pago (tampoco desde Shopify Flow) hasta que subas un Certificado de Sello Digital vigente en Configuraciones → Datos fiscales.

Los dos banners tienen el botón **Actualizar CSD**, que te lleva a [Configuraciones](/docs/configuraciones).

El banner no sigue el calendario de los correos: está visible todos los días mientras falten 30 días o menos, o mientras el CSD esté vencido.

## Los correos

### Quién los recibe

Estos avisos son, sobre todo, para tu contador o quien renueva el CSD. Llegan así:

- **Si guardaste el [correo del responsable fiscal](/docs/correo-del-responsable-fiscal)**: llegan a ese correo y al **correo de tu tienda en Shopify** (el que tienes en los datos de la tienda en tu admin de Shopify).
- **Si no lo guardaste**: llegan solo al correo de tu tienda en Shopify.
- **Si los dos son el mismo correo** (aunque cambien mayúsculas o espacios), llega una sola vez.

Revisa que el correo de tu tienda sea un buzón que alguien lea. El banner no cambia: lo ve cualquiera que abra la app en el admin.

### Cuándo llegan

La app revisa la vigencia de tu CSD todos los días a las **9:00 de la mañana, hora de la Ciudad de México**. Los días se cuentan como días de calendario en esa zona horaria.

**Antes del vencimiento:** un correo a los **30, 15, 7, 3 y 1 días** antes de la fecha en que vence tu CSD. Un correo por cada uno.

Si tu tienda entra a esa ventana con menos de 30 días (por ejemplo, a 12 días del vencimiento), te llega en ese momento el siguiente aviso que toca (en el ejemplo, el de 7 días) y de ahí sigue el calendario normal. No se reenvían los avisos que ya pasaron.

**Desde el día en que vence:** un correo **al día durante 15 días**, del día del vencimiento al día 14. Después ya no se mandan más.

Nunca recibes más de un correo al día por el mismo CSD.

El día del vencimiento cuenta como el primero de esos 15. Si tu CSD vence en la tarde, el correo de esa mañana dice que **vence hoy**. Desde que pasa la hora exacta del vencimiento, los correos dicen que **ya venció** y que ya no puedes timbrar.

### Qué dice el correo

El asunto cambia según el momento:

| Momento | Asunto |
|---|---|
| 30, 15, 7 y 3 días antes | Tu CSD vence en N días (fecha) |
| 1 día antes | Tu CSD vence mañana (fecha) |
| El día del vencimiento, antes de la hora | Tu CSD vence hoy (fecha) |
| Ya vencido | Tu CSD venció el (fecha): ya no puedes timbrar facturas |

Dentro del correo viene:

- La tienda, la razón social y el RFC (si los tienes capturados).
- El número de certificado.
- La fecha y hora de vencimiento, en hora de la Ciudad de México.
- Los pasos para actualizarlo (los mismos de la sección siguiente) y el botón **Actualizar mi CSD**, que abre Configuraciones de la app.

Los correos después del vencimiento te recuerdan que te avisaremos cada día durante dos semanas o hasta que subas un CSD vigente. El del día 14 te dice que es el último recordatorio.

## Cómo actualizar tu CSD

1. Tramita un CSD nuevo en el portal del SAT, en **CertiSAT Web**, con tu e.firma. El SAT te entrega el certificado (**.cer**) y la llave (**.key**) con la contraseña que elijas.
2. En tu admin de Shopify abre **CFDI Express → Configuraciones → Datos fiscales**.
3. Sube el **.cer**, el **.key** y la contraseña, y guarda.

El detalle de esa sección está en [Configuraciones](/docs/configuraciones). Si nunca has tramitado un CSD, en [Onboarding](/docs/onboarding) hay un video con el paso a paso.

Te recomendamos hacerlo con el primer aviso. El trámite en el SAT puede tardar, y no hay forma de timbrar con un CSD vencido.

## Qué pasa cuando subes un CSD nuevo

- Los avisos del CSD anterior se apagan.
- Si el nuevo está vigente y le quedan más de 30 días, el banner desaparece.
- El calendario de avisos empieza de cero con la fecha de vencimiento del nuevo CSD.

## Preguntas frecuentes

### ¿Puedo seguir facturando con el CSD vencido?

No. Mientras no subas uno vigente, CFDI Express no timbra facturas, notas de crédito ni complementos de pago, tampoco desde Shopify Flow.

### ¿Puedo cambiar a quién le llegan los avisos?

Siempre llegan al correo de tu tienda en Shopify; si cambias ese correo en tu admin de Shopify, los siguientes avisos llegan al nuevo. Además, puedes agregar, cambiar o borrar el [correo del responsable fiscal](/docs/correo-del-responsable-fiscal) en **Configuraciones → Datos fiscales**.

### ¿Le llegan a mi contador?

Sí, si guardaste su correo como [correo del responsable fiscal](/docs/correo-del-responsable-fiscal). Los avisos llegan a ese correo y al correo de tu tienda en Shopify. Si no lo has guardado, siguen llegando solo al correo de la tienda: agrégalo en **Configuraciones → Datos fiscales**.

### Ya subí mi CSD nuevo y sigo viendo el banner

El banner se quita cuando el CSD cargado está vigente y le quedan más de 30 días. Revisa en [Configuraciones](/docs/configuraciones) que guardaste el certificado nuevo y no el anterior.

### Uso Modo Flow, ¿me llegan estos avisos?

No. En Modo Flow no cargas CSD en CFDI Express: la vigencia de tu certificado la controla tu propio PAC.
