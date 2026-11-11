---
title: "Cómo mandarle a tu contador las facturas globales con Shopify Flow"
description: "Manda en automático a tu contador las facturas globales de cada corrida con Shopify Flow: UUID, serie y folio, total y liga al ZIP con el PDF y el XML."
keywords: "facturas globales contador, shopify flow factura global, facturación global terminada, enviar facturas al contador, correo automático facturas, CFDI Express"
---
# Cómo mandarle a tu contador las facturas globales con Shopify Flow

URL de esta página: https://cfdi.express/docs/enviar-facturas-globales-al-contador-shopify-flow

Desde la versión **3.30.0** de la app, el disparador **Facturación global terminada** de Shopify Flow trae la lista de facturas globales que se timbraron en la corrida. Con eso armas un workflow que, cada vez que termina una corrida, le manda a tu contador un correo con el UUID, la serie y folio, el total, el número de órdenes y la liga de descarga de cada factura. No tienes que entrar a la app.

La factura global está en los planes **Plus**, **Pro** y **Enterprise**. Cómo correrla, a mano o programada, está en [Facturación global mensual](/docs/facturacion-global-mensual).

## Antes de empezar

1. El plan de la tienda es Plus, Pro o Enterprise.
2. La app está en la **3.30.0** o una posterior.
3. Tienes Shopify Flow instalado. Lo básico de Flow con CFDI Express está en [Uso de Flow para automatizaciones](/docs/uso-shopify-flow-automatizaciones-cfdi-express).

Tus workflows actuales con este disparador siguen funcionando igual. Los datos de las facturas son un extra.

## Qué trae el disparador

**Facturación global terminada** se dispara una vez por corrida, la lances a mano desde la app o con Flow, cuando la corrida termina con éxito o con error. Estos son sus campos y la variable que usas en Flow:

| Campo | Variable en Flow | Qué trae |
| --- | --- | --- |
| Id de la corrida | `{{ corridaid }}` | El mismo id que devolvió la acción **Facturación global**. |
| Estado | `{{ estado }}` | `completada` o `error`. |
| Inicio del periodo | `{{ fechainicio }}` | Primer día del periodo, en formato AAAA-MM-DD. |
| Fin del periodo | `{{ fechafin }}` | Último día del periodo, en formato AAAA-MM-DD. |
| Facturas timbradas | `{{ cfdistimbrados }}` | Número de facturas globales timbradas en la corrida. Siempre trae el total. |
| Órdenes incluidas | `{{ ordenesincluidas }}` | Órdenes que entraron en las facturas timbradas. |
| Órdenes saltadas | `{{ ordenessaltadas }}` | Órdenes que no entraron, por ejemplo por moneda, por rechazo del SAT o porque ya estaban facturadas. |
| Error | `{{ error }}` | Mensaje de error. Vacío si la corrida se completó. |
| Lista de facturas | `facturas.cfdis` | Las facturas timbradas, en el orden en que se timbraron. |
| Lista completa | `{{ facturas.listaCompleta }}` | `true` si la lista trae todas las facturas; `false` si se recortó. |

Cada factura de `facturas.cfdis` trae:

| Campo | Variable dentro del ciclo | Qué trae |
| --- | --- | --- |
| UUID | `{{ cfdi.uuid }}` | Folio fiscal del SAT. |
| Serie y folio | `{{ cfdi.folio }}` | Serie y folio juntos, por ejemplo `FT-123`. |
| Total | `{{ cfdi.total }}` | Total de la factura, en su moneda (MXN o USD). |
| Órdenes | `{{ cfdi.ordenes }}` | Número de órdenes incluidas en esa factura. |
| Liga de descarga | `{{ cfdi.url }}` | Liga permanente a un ZIP con el PDF y el XML. |

`cfdi` es el nombre que le das a cada elemento en el ciclo `{% for cfdi in facturas.cfdis %}`. Puedes usar otro.

### Si la lista no trae todas las facturas

Shopify Flow tiene un límite de tamaño para cada aviso. Si una corrida timbra más facturas de las que caben, la lista trae todas las que entran y `facturas.listaCompleta` llega en `false`. El número total de facturas timbradas siempre viene en `cfdistimbrados`. Las que falten las descargas desde **Facturación global** en la app.

## Paso a paso

1. En el admin de Shopify abre **Apps → Flow** y crea un workflow.
2. Como disparador, busca CFDI Express y elige **Facturación global terminada**.
3. Agrega una **condición**: `estado` es igual a `completada`. Así el correo solo sale cuando la corrida terminó bien.
4. En la rama que se cumple, agrega la acción **Enviar correo electrónico interno** (*Send internal email*).
5. En el campo del correo (*Email address*), escribe el de tu contador. Para varias personas, sepáralas con comas. Este campo no acepta variables.
6. En **Asunto** (*Subject*), escribe por ejemplo:

```liquid
Facturas globales del {{ fechainicio }} al {{ fechafin }}
```

7. En **Mensaje** (*Message*), pega la plantilla de la siguiente sección y ajústala a tu gusto.
8. Guarda y **activa** el workflow.

Para revisar los nombres de las variables, usa la opción para agregar una variable (*Add a variable*) debajo de cada campo del editor de Flow.

<!-- TODO: captura — workflow de Shopify Flow con el disparador Facturación global terminada, la condición estado = completada y la acción Enviar correo electrónico interno -->
![TODO: Workflow para mandar las facturas globales al contador](TODO-screenshot)
<!-- ENDTODO -->

## Plantilla del mensaje

El campo **Mensaje** acepta HTML y Liquid. Esta plantilla recorre la lista de facturas, avisa si la lista se recortó y siempre trae el total de facturas timbradas:

```liquid
<p>Hola, te comparto las facturas globales del {{ fechainicio }} al {{ fechafin }}.</p>

<ul>
{% for cfdi in facturas.cfdis %}
  <li>
    <strong>{{ cfdi.folio }}</strong><br>
    UUID: {{ cfdi.uuid }}<br>
    Total: {{ cfdi.total }}<br>
    Órdenes: {{ cfdi.ordenes }}<br>
    <a href="{{ cfdi.url }}">Descargar PDF y XML (ZIP)</a>
  </li>
{% endfor %}
</ul>

<p>Facturas timbradas en la corrida: {{ cfdistimbrados }}. Órdenes incluidas: {{ ordenesincluidas }}.</p>

{% unless facturas.listaCompleta %}
<p>Esta lista no trae todas las facturas de la corrida. Te mando las que faltan por separado.</p>
{% endunless %}
```

Cada liga descarga un ZIP con el PDF y el XML de esa factura.

## Si la corrida falla

Cuando la corrida termina en `error`, la app ya le manda un correo de aviso al correo de contacto de la tienda. Si además quieres un aviso en Flow, agrega en la rama que no se cumple otra acción **Enviar correo electrónico interno** dirigida a ti, con el mensaje `{{ error }}`. El listado de correos de la app está en [Notificaciones por correo](/docs/notificaciones-por-correo).

## Preguntas frecuentes

### ¿El correo sale también cuando lanzo la corrida a mano?

Sí. El disparador se dispara al terminar cada corrida, la lances a mano desde la app o con la acción **Facturación global** de Flow.

### ¿Qué pasa si la corrida timbra muchas facturas?

La lista trae las que caben en el aviso de Flow y `facturas.listaCompleta` llega en `false`. El total siempre viene en `cfdistimbrados`. Las que falten las descargas desde la app.

### ¿Puedo mandarlas a Slack o a una hoja de cálculo?

Sí. Los mismos campos sirven en cualquier acción de Flow que acepte variables. Usa el mismo ciclo `{% for cfdi in facturas.cfdis %}`.

### ¿En qué planes está?

La factura global está en Plus, Pro y Enterprise. En el plan Básico no hay corridas de factura global, así que este disparador no se usa.

## Relacionados

- [Facturación global mensual](/docs/facturacion-global-mensual)
- [Uso de Flow para automatizaciones con CFDI Express](/docs/uso-shopify-flow-automatizaciones-cfdi-express)
- [Notificaciones por correo](/docs/notificaciones-por-correo)
