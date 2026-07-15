# Buscador de Códigos del SAT (productos y servicios)

CFDI Express cuenta con un buscador público de **códigos de producto y servicio del SAT** que te ayuda a encontrar el código correcto para tus productos usando lenguaje natural, sin necesidad de navegar el catálogo manual del SAT.

## Acceso

El buscador está disponible públicamente en:

> **[cfdi.express/buscador-de-codigos-sat-productos-servicios](https://cfdi.express/buscador-de-codigos-sat-productos-servicios)**

No requiere iniciar sesión ni tener el app instalado; lo puedes usar para ti o compartirlo con tu contador o clientes.

## Cómo funciona

1. Escribe una descripción del producto o servicio en lenguaje natural (por ejemplo, "venta de zapatos de piel" o "servicios de consultoría en marketing").
2. El buscador usa un modelo RAG (búsqueda semántica) sobre el catálogo del SAT y te regresa los códigos que mejor coinciden.

<!-- TODO: captura — interfaz del buscador con un query y resultados de códigos SAT -->
![TODO: Interfaz del buscador de códigos SAT](TODO-screenshot)
3. Cada resultado te lleva a una **página de detalle del código** (`/codigos-sat/{código}`) que muestra:
    - Fechas de vigencia del código
    - Si lleva IVA e IEPS
    - Si aplica cuota al fronterizo
    - Complemento relacionado
    - PalabrasSimilares para refinar la búsqueda

<!-- TODO: captura — página de detalle de un código SAT (vigencia, IVA/IEPS, fronterizo, complemento, similares) -->
![TODO: Página de detalle de un código SAT](TODO-screenshot)

## Cuándo usarlo

- Al configurar tus [códigos del SAT por producto](/docs/codigos-sat-producto) y no saber qué código asignar.
- Para verificar la vigencia o los impuestos de un código antes de usarlo.
- Para compartir con clientes que necesiten identificar el código que los facturará correctamente.

> Si tus productos son genéricos, puedes omitir la configuración por producto y usar los [códigos predeterminados](/docs/configuraciones). El buscador es útil cuando necesitas precisión por producto.