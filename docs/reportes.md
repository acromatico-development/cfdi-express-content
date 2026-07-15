# Reportes de CFDIs y Complementos de Pago

CFDI Express genera reportes en **CSV** con todas tus facturas y complementos de pago en un rango de fechas, listo para tu contabilidad o importarlo a tu ERP. La generación corre de forma asíncrona en segundo plano, por lo que puedes cerrar la app mientras se procesa.

## Cómo generar un reporte

1. Entra a la app de CFDI Express → **Reportes** (en el menú de navegación).
2. Completa el formulario:
    - **Título** descriptivo del reporte (ej. "Ventas Julio 2025").
    - **Fecha inicial** y **Fecha final** del rango a reportar.
3. Haz clic en **Generar reporte**. El registro queda con estatus **Pendiente**.
4. Cuando termina, el estatus cambia a **Completado** y aparece el botón de **descarga CSV**. Si algo falla, queda **Fallido**.

## Columnas del CSV

Cada fila es un CFDI o complemento de pago. El reporte incluye columnas completas para cumplir los requerimientos contables del SAT:

- Datos del emisor y receptor (RFC, régimen, razón social).
- Tipo de comprobante, folio y UUID (y UUID de relación para complementos).
- Uso CFDI, método de pago, moneda, tipo de cambio, condiciones de pago.
- Subtotal, IVA trasladado, IEPS trasladado, impuestos retenidos, total.
- Información global (periodicidad, mes, año) para facturas al público en general.
- Estatus (Facturado/Cancelado/Flow), correo del receptor, URL de descarga y origen (source).

## Detrás de escena

- El reporte se construye en un **worker** en segundo plano (BullMQ) que consulta tus registros de CFDIs y complementos en el rango solicitado.
- Al terminar, el CSV se guarda y se dispara el disparador **`report-generated`** en Shopify Flow (con `startdate`, `enddate`, `reporturl`, `recordcount` y el título), para que puedas encadenar automatizaciones (avisar a contabilidad, subir a un drive, etc.). Revisa [Shopify Flow](/docs/uso-shopify-flow-automatizaciones-cfdi-express) para ver cómo usarlo.

## Métricas en el dashboard de Reportes

La página de Reportes también muestra un resumen de uso: total de CFDIs generados, total de Constancias de Situación Fiscal (CSF) recibidas, total de reportes creados y un cálculo de "tiempo ahorrado" (basado en ~10 min por CFDI y ~30 min por reporte).