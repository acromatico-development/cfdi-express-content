# Plan de Grabación de Tutoriales en Video - CFDI Express

Este documento contiene el plan de videos a grabar, ordenados por **complejidad creciente** y **flujo lógico de funcionamiento**.

---

## Grupo 1: Configuración Inicial (Baja Complejidad)

### 1. Onboarding y Configuración Inicial ✅ EXISTENTE
- **Video actual**: `g7wiNKPqh8k`
- **Contenido**: Selección PAC/Flow, llenado de formulario inicial
- **Estado**: Revisar si necesita actualización por cambios de UI

### 2. Generación de CSD (Certificado Digital) ✅ EXISTENTE
- **Video actual**: `e4W7Q5a0ULs`
- **Contenido**: Cómo generar certificado de sello digital
- **Estado**: Revisar si necesita actualización

### 3. Dashboard y Lista de Órdenes - NUEVO
- **Contenido**:
  - Navegación inicial de la app
  - Filtrado por correo, orden, estatus
  - Entendiendo los badges de estatus (Pendiente/Facturado/Cancelado)
  - Entendiendo los estados (Activa/Cerrada, Pendiente/Parcial/Pagado)
- **Enlace al archivo**: `docs/ordenes-estatus.md` (falta video)

### 4. Configuraciones Completas - NUEVO
- **Contenido**:
  - Datos fiscales del emisor
  - Dirección fiscal completa
  - Carga de CSD (Modo PAC)
  - Toggle PAC/Flow
  - Códigos SAT predeterminados
  - Reglas de periodo de facturación
  - Toggles de notificación
  - Gestión de plan y límites
- **Enlace al archivo**: `docs/configuraciones.md` (falta video)

---

## Grupo 2: Conceptos Básicos de Facturación (Media Complejidad)

### 5. Facturación Manual desde Admin - ✅ EXISTENTE
- **Video actual**: `G2_Wl8bJqus`
- **Contenido**: Captura de datos fiscales, generación de CFDI
- **Estado**: Revisar si necesita actualización

### 6. Códigos SAT por Producto y IEPS - ⚠️ RE-GRAVAR (Video Viejo)
- **Video actual**: `DPETjTdVMJ0`
- **Contenido**: Asignar códigos SAT específicos por producto
- **Estado**: Video antiguo, considerar re-grabar para consistencia
- **Enlace al archivo**: `docs/codigos-sat-producto.md`

### 7. Carga Masiva CSV - ⚠️ VIDEO ANTERIOR CONFUSO
- **Video**: `https://videos.acromatico.dev/video/16661156-5e71-4e6c-bf72-f8d178147c16`
- **Contenido**: Subir CSV con códigos SAT, actualizar productos en lote
- **Estado**: Video grabado, revisar si debe migrar a YouTube embebido
- **Enlace al archivo**: `docs/carga-masiva-productos-csv.md`

### 8. Buscador de Códigos SAT - NUEVO
- **Contenido**:
  - Acceso a `cfdi.express/buscador-de-codigos-sat-productos-servicios`
  - Búsqueda por descripción en lenguaje natural
  - Interpretación de resultados (vigencia, IVA, IEPS, frontera, complementos)
  - Uso para asignar códigos por producto
- **Enlace al archivo**: `docs/buscador-de-codigos-sat.md`

---

## Grupo 3: Formulares de Facturación del Cliente (Media-Alta Complejidad)

### 9. Thank You Page (Checkout) - ✅ EXISTENTE
- **Video actual**: `iKweIA7HyHI`
- **Contenido**: Agregar bloque de facturación a Thank You Page
- **Estado**: Revisar si necesita actualización

### 10. Order Status Page - ⚠️ RE-GRAVAR
- **Contenido**: Agregar formulario de facturación a Order Status Page
- **Estado**: Actualmente compartido con video de Thank You Page
- **Enlace al archivo**: `docs/facturacion-pagina-estado-de-orden.md`
- **Recomendación**: Crear video enfocado exclusivamente en esta página

### 11. POS Integration - ✅ EXISTENTE
- **Video actual**: `UjpmQC0FTXA`
- **Contenido**: Integración con Shopify POS, configuración de apps del canal POS
- **Estado**: Video actualizado, verificar calidad

### 12. Theme App Block - NUEVO
- **Contenido**:
  - Navegación a Themes → Customize
  - Agregar bloque "Formulario de Facturación" a cualquier página
  - Configuración de settings (título, colores, textos)
  - Diferencia con extensiones de checkout
  - Búsqueda por orden# + total
- **Enlace al archivo**: `docs/formulario-de-facturacion-theme-block.md`

---

## Grupo 4: Funcionalidades Avanzadas de Facturación (Alta Complejidad)

### 13. Facturación al Público en General - ✅ EXISTENTE
- **Video actual**: `vDxG9X-TLJg`
- **Contenido**:
  - Facturación manual express
  - Automatización con Shopify Flow
  - Plantillas de automatización (fin de día, 3 días, 10 días, fin de mes)
- **Estado**: Revisar si necesita actualización

### 14. Facturación al Extranjero (RFC XEXX010101000) - NUEVO
- **Contenido**:
  - Cuando aplicar RFC XEXX010101000
  - Configuración desde admin (botón especial)
  - Checkboxes de IVA 0% por línea
  - Cálculo de IVA 0% vs 16% en mezcla
  - Validaciones y requisitos SAT
- **Enlace al archivo**: `docs/facturacion-al-extranjero.md`

### 15. Complementos de Pago (PPD) - NUEVO
- **Contenido**:
  - Generación de complemento con abono
  - Cálculo de saldo insoluto y parcialidad
  - Formulario de fecha, forma de pago, monto
  - Cancelación de complemento anterior a cancelación de CFDI
  - Campos guardados (UUID, folio, parcialidad, etc.)
- **Enlace al archivo**: `docs/complementos-de-pago.md`

### 16. Cancelación de CFDI y Acuse - NUEVO
- **Contenido**:
  - Cómo cancelar un CFDI timbrado
  - Motivos de cancelación SAT (01, 02, 03, 04)
  - Restricciones (no cancelar con complementos activos)
  - Descarga de acuse de cancelación
  - Re-facturación después de cancelar
  - Flujo de disparo `cfdi-deleted`
- **Enlace al archivo**: `docs/cancelacion-y-acuse.md`

### 17. Modo Flow (PAC Propio) - NUEVO
- **Contenido**:
  - Diferencia entre Modo PAC y Modo Flow
  - Cuándo usar Modo Flow
  - Configuración inicial sin CSD
  - Disparador `cfdi-flow-mode`
  - Qué envía y qué procesa tu propio PAC
  - Estatus "flow" vs "Facturado"
  - Cambio entre modos PAC/Flow
- **Enlace al archivo**: `docs/modo-flow-cfdi-express-para-usar-tu-propio-pac-de-facturacion.md`

---

## Grupo 5: Automatización y Flujo (Muy Alta Complejidad)

### 18. Shopify Flow - Disparadores y Acciones - NUEVO
- **Contenido**:
  - Introducción a Shopify Flow y CFDI Express
  - Disparadores: `cfdi-created`, `cfdi-deleted`, `cfdi-flow-mode`, `report-generated`
  - Acciones: "Crear CFDI", "Eliminar CFDI"
  - Creando workflow: trigger → query → condición → acción
- **Enlace al archivo**: `docs/uso-shopify-flow-automatizaciones-cfdi-express.md`

### 19. Shopify Flow - Automatización de Facturación - NUEVO
- **Contenido**:
  - Importar plantillas de automatización
  - Configurar query de órdenes pagadas y no facturadas
  - Condicionales para filtrar por estatus `$app.facturado`
  - Automatización recurrente (cron jobs)
  - Automatización por evento (orden creada, cancelación, etc.)
- **Enlace al archivo**: `docs/uso-shopify-flow-automatizaciones-cfdi-express.md`
- **Nota**: Considerar separar en 2 videos: (18) Conceptos base + (19) Automatizaciones prácticas

### 20. Reportes y Exportación CSV - NUEVO
- **Contenido**:
  - Generación de reporte (título, rango de fechas)
  - Procesamiento en segundo plano
  - Estados: Pendiente → Completado / Fallido
  - Columnas del CSV y datos disponibles
  - Descarga del archivo CSV
  - Disparador `report-generated` para automatizadas
  - Métricas en dashboard (CFDIs, CSFs, tiempo ahorrado)
- **Enlace al archivo**: `docs/reportes.md`

---

## Grupo 6: Funcionalidades Secundarias (Baja-Media Complejidad)

### 21. Notificaciones por Correo - NUEVO
- **Contenido**:
  - Correo "Tu CFDI para la orden X ha sido generada" (MODO PAC)
  - Correo "Datos Fiscales Enviados" (MODO FLOW)
  - Adjunto `factura.zip` con PDF + XML
  - Toggle `skipCfdiEmail`
  - Evento Klaviyo "CFDI Generated"
  - Correos que NO se envían automáticamente (cancelación)
- **Enlace al archivo**: `docs/notificaciones-por-correo.md`

### 22. Acción de Impresión Rápida - NUEVO
- **Contenido**:
  - Buscar acción "Imprimir CFDI" en página de orden
  - Consulta de metafield `$app.frediexpress.url_cfdi`
  - Servicio del PDF desde `/api/print-cfdi`
  - Mensaje de error si no hay CFDI
  - Diferencia con re-facturar desde editor
- **Enlace al archivo**: `docs/accion-de-impresion-admin.md`

### 23. Planes y Precios - NUEVO
- **Contenido**:
  - Resumen de planes (Standard, Plus, Pro, Enterprise)
  - Cómo se cobra (suscripción + cargo por uso)
  - CFDIs incluidos por plan
  - Cambio de plan (interfaz y confirmación Shopify)
  - Gestión de cargos por excedente y cancelaciones
- **Enlace al archivo**: `docs/planes-y-precios.md`

---

## Resumen Ejecutivo

| Prioridad | Cantidad | Videos |
|-----------|----------|--------|
| **Baja Complejidad** | 7 | Grupo 1-2 |
| **Media Complejidad** | 5 | Grupo 3 |
| **Alta Complejidad** | 4 | Grupo 4 |
| **Muy Alta Complejidad** | 2 | Grupo 5 |
| **Funciones Secundarias** | 3 | Grupo 6 |
| **TOTAL** | **21 videos** | |

### Videos Existentes (a revisar)
- ✅ **8 videos** tienen video incrustado actual
- ⚠️ **2-3 videos** necesitan re-grabación por videos antiguos o confusos
- ❌ **~13 videos** son completamente nuevos (sin video grabado)

### Próximos Pasos Inmediatos
1. Priorizar grupos por order de complejidad
2. Grabar 2-3 videos del Grupo 1 (Setup básico)
3. Luego avanzar al Grupo 2 (Conceptos básicos de facturación)
4. Continuar secuencialmente hacia mayor complejidad

---

## Notas Técnicas de Grabación

- **Formato**: 1080p, 16:9, ~2-3 minutos cada uno
- **Estándares**:
  - Mostrar zoom in en zonas de interés
  - Highlight con mouse movement
  - Incluir subtítulos en español
  - Logo CFDI Express en esquina (si aplica)
- **Herramientas sugeridas**:
  - OBS Studio / ScreenFlow
  - Adobe Premiere / DaVinci Resolve
  - CapCut (para subtítulos automáticos)

---

## Enlaces de Referencia

- **Plataforma**: YouTube (todos los videos embeddables)
- **Assets**: `https://videos.acromatico.dev/` (videos existentes)
- **Assets**: `https://cdn.shopify.com/s/files/1/0804/5540/1763/files/` (capturas de UI)

---

*Documento actualizado: 2026-07-20*
