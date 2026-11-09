---
title: "Facturar anticipos en CFDI 4.0: los 3 comprobantes del SAT"
description: "Cómo facturar un anticipo en CFDI 4.0: factura del anticipo, factura por el total con relación 07 y nota de crédito con forma de pago 30."
image: "https://assets.acromatico.dev/marketing/diego/blog/hero-cfdi-facturas-4.0.png"
author: "Rafael González"
date: "2026-11-16"
keywords: "facturar anticipos, anticipo cfdi 4.0, cómo facturar un anticipo, tipo de relación 07, forma de pago 30, aplicación de anticipo, 84111506, ACT, nota de crédito anticipo, Apéndice 6 Anexo 20, CFDI Express"
---
# Facturar anticipos en CFDI 4.0: los 3 comprobantes que pide el SAT

Un cliente te paga antes de saber exactamente qué va a comprar o cuánto va a costar. Ese dinero es un **anticipo**, y el SAT no lo factura como una venta normal: pide **tres comprobantes** que se relacionan entre sí.

Este post es para contadores y dueños de negocio. Explica cuándo un pago es anticipo, qué lleva cada CFDI y los errores que más se repiten. Las reglas salen del **Apéndice 6** de la Guía de llenado del Anexo 20 («Procedimiento para la emisión de los CFDI en el caso de anticipos recibidos»), en su procedimiento **A**: facturación aplicando anticipo con CFDI de egreso.

## ¿Qué es un anticipo para el SAT?

**Respuesta corta:** es un pago que recibes cuando todavía no está definido el bien o servicio, su precio, o ninguno de los dos.

La Guía de llenado dice que solo hay anticipo cuando se recibe un pago en una operación donde:

- no se conoce o no se ha determinado el bien o servicio que se va a adquirir **o** su precio, o
- no se conoce o no se ha determinado **ni** el bien o servicio **ni** su precio.

Y aclara dos casos que **no** son anticipo:

- **Ya hay acuerdo sobre el bien o servicio y su precio**, aunque no sea por escrito, y el cliente paga una parte. Eso es una venta en parcialidades.
- **Un depósito en garantía**, como el que entrega un arrendatario para garantizar el pago de las rentas.

Si ya cerraste qué vendes y en cuánto, no es anticipo: factura la venta y registra los pagos que vayan llegando.

## Los 3 comprobantes del procedimiento A

1. **Factura del anticipo.** Se emite cuando recibes el dinero.
2. **Factura por el valor total de la operación.** Se emite cuando la venta se concreta y relaciona el anticipo con el tipo de relación **07**.
3. **CFDI de egreso (nota de crédito) por el anticipo aplicado.** Se emite después de la factura por el total, con forma de pago **30** y relación **07** a esa factura.

La Guía de llenado indica que la factura por el total y el egreso lleven, preferentemente, la misma fecha, y que primero se emita el ingreso y después el egreso.

### Campos clave de cada comprobante

| Campo | 1. Factura del anticipo | 2. Factura por el total | 3. Nota de crédito (egreso) |
|---|---|---|---|
| Tipo de comprobante | I (Ingreso) | I (Ingreso) | E (Egreso) |
| Método de pago | PUE | El que corresponda (PUE o PPD) | PUE |
| Forma de pago | La forma real del anticipo (transferencia, efectivo…) | La que corresponda al pago del remanente | 30 (Aplicación de anticipos) |
| CFDI relacionados | No lleva | Tipo de relación 07, UUID del anticipo | Tipo de relación 07, UUID de la factura por el total |
| Conceptos | Uno solo: 84111506, cantidad 1, unidad ACT | Los bienes o servicios reales, a precio completo | Uno solo: 84111506, cantidad 1, unidad ACT |
| Descripción | «Anticipo del bien o servicio» | La de cada bien o servicio | «Aplicación de anticipo» |
| Valor unitario | El anticipo antes de impuestos | El valor de cada bien o servicio | El anticipo aplicado antes de impuestos |

Si el remanente no se paga al emitir la factura por el total, la Guía de llenado pide un CFDI con complemento de pago (REP) por cada pago que recibas. Cómo emitir un REP que cubre varias facturas está en [este post](/blog/emitir-rep-varias-facturas-ppd).

### Ejemplo con números

El cliente te deja **$11,600** (IVA incluido) de anticipo por transferencia. Semanas después se concreta la venta por **$58,000** (IVA incluido).

1. **Factura del anticipo:** $11,600, PUE, forma 03 (transferencia), un concepto 84111506 / ACT de $10,000 más IVA.
2. **Factura por el total:** $58,000 con los productos reales, relación 07 al UUID de la factura del anticipo. Si el cliente paga los $46,400 restantes en ese momento, va con PUE y la forma real. Si los paga después, va con PPD y forma 99, y emites un REP por cada pago.
3. **Nota de crédito:** $11,600, forma 30, relación 07 al UUID de la factura por el total, un concepto 84111506 / ACT de $10,000 más IVA.

## Errores comunes al facturar anticipos

**Usar la relación 04 en lugar de la 07.** La 04 es sustitución de los CFDI previos: sirve para reemplazar una factura que se va a cancelar con motivo 01. Para aplicar un anticipo la relación es **07** (CFDI por aplicación de anticipo).

**Poner forma de pago 30 en la factura del anticipo.** El anticipo sí movió dinero, así que su factura lleva la forma real en que llegó. La forma **30** va en la nota de crédito que aplica el anticipo.

**Relacionar la nota de crédito con la factura del anticipo.** La nota con forma 30 se relaciona con la **factura por el valor total**, no con el anticipo. El anticipo lo relaciona la factura por el total.

Otros que también aparecen:

- Restar el anticipo en la factura por el total. Esa factura va por el valor completo de la operación; el anticipo se descuenta con la nota de crédito.
- Poner la clave 84111506 / ACT en la factura por el total. Esa clave solo va en la factura del anticipo y en la nota de crédito.
- Emitir la factura del anticipo como PPD. Siempre es PUE.

Si ya timbraste algo mal, el camino suele ser cancelar y volver a emitir. Los motivos de cancelación y el acuse que entrega el SAT están en [Acuse de cancelación](/blog/acuse-de-cancelacion).

## Cómo lo automatiza CFDI Express

La [API de CFDI Express](https://cfdi.express/api) timbra los tres comprobantes del procedimiento A y valida cada combinación **antes de timbrar**, para que no gastes un timbre en un CFDI que no sigue la guía:

- **Factura del anticipo** con `POST /v1/invoices`. La API rechaza más de un concepto, una cantidad distinta de 1, PPD o forma de pago 30.
- **Factura por el total** con `POST /v1/invoices` y `cfdiRelacionados` tipo 07. La API revisa que el UUID sea una factura de anticipo vigente del mismo emisor y del mismo receptor, y que ese anticipo no se haya aplicado antes en otra factura.
- **Nota de crédito** con `POST /v1/credit_notes`, tipo de relación 07 y forma 30. La API rechaza la nota si la relacionas con la factura del anticipo en lugar de la factura por el total, y no deja aplicar más de lo que se anticipó.

Las revisiones contra otros comprobantes aplican cuando el anticipo y la factura se timbraron en CFDI Express. Cada error explica qué mandar en su lugar. El mismo flujo está en el **servidor MCP**, así que un agente de IA puede emitir los tres comprobantes con `create_invoice` y `create_credit_note`.

Hoy la API cubre el procedimiento A. El procedimiento B (factura por el remanente, con el anticipo como descuento en cada concepto) no está soportado.

El paso a paso con los requests está en la guía [Cómo facturar anticipos con CFDI Express](/docs/facturar-anticipos). Las notas de crédito en general están en [Crear una nota de crédito](/docs/api/crear-nota-de-credito).

**[Crea tu llave de prueba en dash.cfdi.express](https://dash.cfdi.express)** y timbra los tres comprobantes en el ambiente de pruebas. ¿Prefieres verlo con tu caso? [Agenda una demo](https://cal.com/team/acromatico-development/cfdi-express) o escribe a [hola@cfdi.express](mailto:hola@cfdi.express).

## Preguntas frecuentes

**¿Cuándo un pago es anticipo y cuándo es una venta en parcialidades?**
Es anticipo cuando todavía no se conoce o no se ha determinado el bien o servicio, su precio, o ninguno de los dos. Si ya hay acuerdo sobre el bien o servicio y su precio, aunque no sea por escrito, y el cliente paga una parte, es una venta en parcialidades.

**¿Qué clave de producto lleva la factura de un anticipo?**
Un solo concepto con clave 84111506, cantidad 1, unidad ACT y descripción «Anticipo del bien o servicio». El valor unitario es el anticipo antes de impuestos.

**¿Con qué factura se relaciona la nota de crédito con forma de pago 30?**
Con la factura por el valor total de la operación, usando el tipo de relación 07. No se relaciona con la factura del anticipo.

**¿La factura por el total puede quedar pendiente de pago?**
Sí. Si el remanente se paga después, la factura por el total va con el método PPD y se emite un CFDI con complemento de pago por cada pago que recibas.

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {"@type": "Question", "name": "¿Cuándo un pago es anticipo y cuándo es una venta en parcialidades?", "acceptedAnswer": {"@type": "Answer", "text": "Es anticipo cuando todavía no se conoce o no se ha determinado el bien o servicio, su precio, o ninguno de los dos. Si ya hay acuerdo sobre el bien o servicio y su precio, aunque no sea por escrito, y el cliente paga una parte, es una venta en parcialidades."}},
    {"@type": "Question", "name": "¿Qué clave de producto lleva la factura de un anticipo?", "acceptedAnswer": {"@type": "Answer", "text": "Un solo concepto con clave 84111506, cantidad 1, unidad ACT y descripción «Anticipo del bien o servicio». El valor unitario es el anticipo antes de impuestos."}},
    {"@type": "Question", "name": "¿Con qué factura se relaciona la nota de crédito con forma de pago 30?", "acceptedAnswer": {"@type": "Answer", "text": "Con la factura por el valor total de la operación, usando el tipo de relación 07. No se relaciona con la factura del anticipo."}},
    {"@type": "Question", "name": "¿La factura por el total puede quedar pendiente de pago?", "acceptedAnswer": {"@type": "Answer", "text": "Sí. Si el remanente se paga después, la factura por el total va con el método PPD y se emite un CFDI con complemento de pago por cada pago que recibas."}}
  ]
}
</script>
