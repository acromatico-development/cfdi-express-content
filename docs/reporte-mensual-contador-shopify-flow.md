---
title: "Mandar el reporte mensual al contador con Shopify Flow"
description: "Genera el reporte de CFDIs (CSV y Excel) el día 1 o al terminar la facturación global y mándaselo por correo a tu contador. En Plus, Pro y Enterprise."
keywords: "reporte mensual contador, shopify flow generar reporte, report generated, reporte cfdi correo automático, facturación global reporte, CFDI Express"
---
# Mandar el reporte mensual al contador con Shopify Flow

URL de esta página: https://cfdi.express/docs/reporte-mensual-contador-shopify-flow

Desde la versión **3.31.0** de la app, la acción **Generar reporte** de Shopify Flow arma el mismo reporte de la sección [Reportes](/docs/reportes): un CSV y un Excel con los CFDIs y los complementos de pago del periodo. Cuando el archivo queda listo, el disparador **Report Generated** (`report-generated`) trae la liga del CSV para mandársela a tu contador. No tienes que entrar a la app.

Esta acción está en los planes **Plus**, **Pro** y **Enterprise**. En cualquier plan puedes seguir generando el reporte a mano desde **CFDI Express › Reportes**.

Hay dos recetas: el reporte del mes anterior, el día 1, y el reporte del periodo al terminar una corrida de [facturación global](/docs/facturacion-global-mensual). En ambas, la acción solo encola el reporte; el correo va en un segundo workflow que arranca con **Report Generated**. Ese segundo workflow sirve para las dos recetas.

## Antes de empezar

1. El plan de la tienda es Plus, Pro o Enterprise.
2. La app está en la **3.31.0** o una posterior.
3. Tienes Shopify Flow instalado. Lo básico de Flow con CFDI Express está en [Uso de Flow para automatizaciones](/docs/uso-shopify-flow-automatizaciones-cfdi-express).

## Qué hace la acción

**Generar reporte** (`cfdi-report-action`) pide un rango de fechas y, si quieres, un título. Genera un solo tipo de reporte, el mismo de la pantalla Reportes: CFDIs y complementos de pago del periodo, en **CSV** y en **Excel**. No es el archivo de una fila por orden que sale de la facturación global.

| Campo | Variable en Flow | Obligatorio | Formato |
| --- | --- | --- | --- |
| Fecha de inicio | `fecha_inicio` | Sí | Día de calendario `AAAA-MM-DD`, hora de México. El periodo empieza a las 00:00:00. |
| Fecha de fin | `fecha_fin` | Sí | Día de calendario `AAAA-MM-DD`, incluido completo hasta las 23:59:59, hora de México. |
| Título | `titulo` | No | Texto de hasta 120 caracteres. Si lo dejas vacío, queda `Reporte AAAA-MM-DD a AAAA-MM-DD`. |

Si una fecha no es un día válido (`AAAA-MM-DD`) o la fecha de fin es anterior a la de inicio, el paso falla con un 422 y Flow no lo reintenta. La acción responde enseguida con el `reporte_id` y el `estado` (`pending`): el archivo lo arma después un proceso en segundo plano.

En un plan que no es Plus, Pro o Enterprise, el paso también falla con un 422 y este mensaje: «La acción Generar reporte de Flow es de los planes Plus, Pro y Enterprise. En cualquier plan puedes generar el reporte desde CFDI Express › Reportes. Cambia de plan en CFDI Express › Configuraciones.» Si no se puede confirmar el plan, Flow recibe un error temporal y reintenta.

### Qué trae el disparador Report Generated

El nombre del disparador en la extensión es **Report Generated** (`report-generated`). Se dispara solo cuando el reporte termina bien. Si falla, este aviso no sale.

| Campo | Variable en Flow | Qué trae |
| --- | --- | --- |
| Inicio del periodo | `{{ startdate }}` | Inicio del periodo, en hora de México, con formato de fecha y hora en español (por ejemplo `1 oct 2026, 00:00:00`). |
| Fin del periodo | `{{ enddate }}` | Fin del periodo, en el mismo formato. |
| Liga de descarga | `{{ reporturl }}` | Liga pública al **CSV**. No trae la del Excel: ese archivo se descarga en **CFDI Express › Reportes**. La liga no lleva una fecha de caducidad. |
| Número de registros | `{{ recordcount }}` | Cuántas filas lleva el reporte (CFDIs y complementos del periodo; en una factura global, una fila por orden). |
| Título | `{{ title }}` | El título del reporte. |
| Id del reporte | `{{ reportid }}` | El mismo id que devolvió la acción como `reporte_id`. También lo ves en Reportes. |
| Origen | `{{ origen }}` | `flow`, si lo pidió la acción Generar reporte, o `manual`, si lo pediste en la pantalla Reportes. Un reporte de antes de la 3.31.0 llega como `manual`. |

## Receta 1: reporte del mes al contador el día 1

Son dos workflows.

### Workflow A: generar el reporte

1. En el admin de Shopify abre **Apps → Flow** y crea un workflow.
2. Como disparador, elige **Hora programada** (*Scheduled time*). Configúralo para que corra el **día 1 de cada mes**. La zona horaria, por defecto, es la de la tienda.
3. Agrega la acción **Generar reporte** de CFDI Express.
4. En **Fecha de inicio** pega:

```liquid
{{ scheduledAt | date_minus: "1 day" | date: "%Y-%m-01" }}
```

5. En **Fecha de fin** pega:

```liquid
{{ scheduledAt | date_minus: "1 day" | date: "%Y-%m-%d" }}
```

6. El título es opcional. Si lo dejas vacío, queda con las dos fechas del periodo.
7. Guarda y **activa** el workflow.

Ese Liquid es el que muestra la propia acción en la ayuda de los campos. `date_minus` es un filtro de Shopify Flow: resta un día al `scheduledAt` del disparador del día 1, y así obtienes el último día del mes anterior. `%Y-%m-01` es el primero de ese mes y `%Y-%m-%d` es ese último día, en el formato `AAAA-MM-DD` que pide la acción. Las fechas se interpretan en hora de México.

<!-- TODO: captura — workflow de Shopify Flow con el disparador Hora programada (día 1) y la acción Generar reporte -->
![TODO: Workflow para generar el reporte del mes anterior el día 1](TODO-screenshot)
<!-- ENDTODO -->

### Workflow B: mandar el correo

Este workflow también sirve para la receta 2.

1. Crea otro workflow.
2. Como disparador, busca CFDI Express y elige **Report Generated**.
3. Agrega una **condición**: `origen` es igual a `flow`. Así el correo solo sale cuando el reporte lo pidió esta acción, no cuando lo generas a mano desde la app. Si también quieres el correo de los reportes manuales, omite la condición.
4. En la rama que se cumple, agrega la acción **Enviar correo electrónico interno** (*Send internal email*).
5. En el campo del correo (*Email address*), escribe el de tu contador. Para varias personas, sepáralas con comas. Este campo no acepta variables.
6. En **Asunto** (*Subject*), escribe por ejemplo:

```liquid
Reporte de CFDIs: {{ title }}
```

7. En **Mensaje** (*Message*), pega la plantilla de la siguiente sección y ajústala a tu gusto.
8. Guarda y **activa** el workflow.

<!-- TODO: captura — workflow de Shopify Flow con el disparador Report Generated, la condición origen = flow y la acción Enviar correo electrónico interno -->
![TODO: Workflow para mandar el reporte al contador](TODO-screenshot)
<!-- ENDTODO -->

## Plantilla del mensaje

El campo **Mensaje** acepta HTML y Liquid. La liga descarga el CSV:

```liquid
<p>Hola, te comparto el reporte de CFDIs {{ title }}.</p>
<p>Periodo: {{ startdate }} al {{ enddate }}.</p>
<p>Registros: {{ recordcount }}.</p>
<p><a href="{{ reporturl }}">Descargar CSV</a></p>
```

El Excel del mismo reporte se descarga en **CFDI Express › Reportes**.

## Receta 2: reporte después de la facturación global

La factura global está en Plus, Pro y Enterprise, igual que esta acción. Cómo correrla está en [Facturación global mensual](/docs/facturacion-global-mensual). Cómo mandarle a tu contador los ZIP de esas facturas está en [Cómo mandarle a tu contador las facturas globales con Shopify Flow](/docs/enviar-facturas-globales-al-contador-shopify-flow).

Este reporte no sustituye esos ZIP: es el reporte de CFDIs y complementos de pago del mismo periodo de la corrida.

1. Crea un workflow.
2. Como disparador, busca CFDI Express y elige **Facturación global terminada**.
3. Agrega una **condición**: `estado` es igual a `completada`. Así el reporte solo se pide cuando la corrida terminó bien.
4. En la rama que se cumple, agrega la acción **Generar reporte**.
5. En **Fecha de inicio** usa `{{ fechainicio }}`. En **Fecha de fin**, `{{ fechafin }}`. Esos campos ya vienen en `AAAA-MM-DD`.
6. El título es opcional.
7. Guarda y **activa** el workflow.

El correo lo manda el mismo workflow B de la receta 1, con el disparador **Report Generated**. No hace falta uno nuevo.

<!-- TODO: captura — workflow de Shopify Flow con el disparador Facturación global terminada, la condición estado = completada y la acción Generar reporte -->
![TODO: Workflow para generar el reporte al terminar la facturación global](TODO-screenshot)
<!-- ENDTODO -->

## Preguntas frecuentes

### ¿Por qué son dos workflows?

La acción **Generar reporte** solo encola el trabajo y responde con `pending`. El CSV todavía no existe. Cuando el archivo está listo, arranca el disparador **Report Generated**, que es el punto de partida de otro workflow.

### ¿Qué pasa si estoy en el plan Básico?

La acción no corre. Flow muestra el mensaje de que es de Plus, Pro y Enterprise. En cualquier plan puedes generar el mismo reporte desde **CFDI Express › Reportes**.

### ¿El correo trae también el Excel?

No. `reporturl` es la liga del CSV. El Excel se descarga en la pantalla Reportes.

### ¿Y si el periodo no tiene CFDIs?

El reporte igual termina y **Report Generated** se dispara. El CSV lleva el encabezado y una fila que lo indica.

### ¿En qué planes está?

La acción **Generar reporte** está en Plus, Pro y Enterprise. La factura global, también. La pantalla Reportes está en todos los planes.

## Relacionados

- [Reportes de CFDIs y Complementos](/docs/reportes)
- [Uso de Flow para automatizaciones con CFDI Express](/docs/uso-shopify-flow-automatizaciones-cfdi-express)
- [Cómo mandarle a tu contador las facturas globales con Shopify Flow](/docs/enviar-facturas-globales-al-contador-shopify-flow)
- [Facturación global mensual](/docs/facturacion-global-mensual)
- [Notificaciones por correo](/docs/notificaciones-por-correo)
