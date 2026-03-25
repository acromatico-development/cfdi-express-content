# Facturación al Público en General (Manual y Automática)

Para tener una facturación ordenada y de acuerdo a lo que el SAT requiere, es importante que todas las órdenes o compras que se hayan hecho en nuestro negocio queden facturadas al final del año fiscal. Para poder lograr esto, los negocios que facturan órdenes / ventas a sus clientes deben definir ciertos periodos de tiempo en los que las órdenes / ventas que no se hayan facturado anteriormente se facturen al público en general, ya sea en un CFDI global o facturando cada orden / venta al público en general.

### Video Tutorial

Si prefieres ver un video tutorial puedes consultarlo aquí:

<iframe width="100%" height="315" src="https://www.youtube.com/embed/vDxG9X-TLJg?si=OYVLPnMLJaufAiuN" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Botón de Facturación al Público en General Express

En CFDI Express, contamos con un botón de facturación al público, en general express. Este botón se encuentra en la pantalla de facturación de la aplicación de CFDI Express dentro del Admin de Shopify. A continuación se muestra una captura de pantalla de dicho botón:

![Botón de Facturación al Público en General Express](https://videos.acromatico.dev/api/images/assets/4dd6caa7-781a-42a1-8b9e-b91c72673761.png?w=1500)

Este botón, lo único que hace es prellenar el formulario con el RFC, razón social, código postal, uso del CFDI y correo necesarios para poder generar una factura al público en general. Uno debe manualmente seleccionar el método de pago y proceder a generar la factura para que dicha factura sea timbrada al público en general.

## Automatización Recurrente para Facturación al Público en General global

Gran parte de la mentalidad en CFDI Express es aprovechar muchas de las herramientas que Shopify nos da para facilitar el comercio y el operar tu negocio. CFDI Express busca integrarse tanto con Shopify al punto de que no notes que es una herramienta externa a Shopify y se sienta como parte nativa de la plataforma.

Shopify tiene, dentro de sus herramientas, una aplicación que se llama Shopify Flow, la cual permite generar flujos de trabajo automatizados a partir de disparadores y acciones. CFDI Express cuenta con disparadores y acciones que se pueden utilizar en flujos de trabajo automatizados dentro de Shopify Flow. Una de las acciones que CFDI Express tiene es la acción de timbrado de CFDI. Esto permite que los usuarios de Flow en sus automatizaciones pueda definir puntos en los que las facturas CFDI se timbrarán automáticamente.

También, uno de los triggers más utilizados en Flow es el trigger de recurrencia o el cron job. Este trigger permite disparar flujos automatizados de trabajo de manera recurrente en ciertos periodos de tiempo.

Combinando el trigger de recurrencia con la acción de facturación automática y agregando ciertas condicionales podemos generar una automatización que cada cierto tiempo revise las órdenes que no están timbradas y las timbre al público en general. Sabemos que no es cualquier cosa diseñar estas automatizaciones incluso aunque Flow es una herramienta muy amigable. Por lo mismo hemos prediseñado una plantilla que puede servir como base para generar dicha automatización, descargarla y utilizarla como base para generar la automatización:

## Plantillas de Shopify Flow

- [Plantilla de auto facturación al final del día](https://assets.acromatico.dev/assets/5d6eebfc-9304-4860-8017-2145b2150e43.flow)
- [Plantilla de auto facturación despues de 3 días](https://assets.acromatico.dev/assets/4b95bd41-4ba3-451d-babd-40b632821148.flow)
- [Plantilla de auto facturación despues de 10 días](https://assets.acromatico.dev/assets/a4e4ea57-6d0a-42ad-be1e-b4eeec33d7dc.flow)
- [Plantilla de auto facturación al final del mes](https://assets.acromatico.dev/assets/3ef1789f-05c1-489e-9b12-a0acbfe437f6.flow)

> Esta plantilla está diseñada para facturar automáticamente las órdenes pagadas y no facturadas que se generaron durante el día, hace más de 3 días, hace más de 10 días y al final del mes. Si quieres expandir el tiempo de espera deberás editar los parámetros del query de búsqueda de órdenes, es muy sencillo y puedes hacerlo en el editor de Flow. Igualmente si requieres una automatización más avanzada o que se integre con otras apps o acciones de Shopify Flow, no dudes en contactarnos a través de [Acromático Development](https://acromatico.dev).
