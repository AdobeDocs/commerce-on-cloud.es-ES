---
title: Explicación de la aplicación
description: Obtenga información sobre cómo funciona Adobe Commerce Traffic Insights, cómo gestionarlo con filtros, cómo se miden sus datos, y sus limitaciones y rendimiento de datos.
feature: Cloud, Observability
role: Admin
source-git-commit: 09318645dd341a74d72237f4d4162d1d26d03651
workflow-type: tm+mt
source-wordcount: '949'
ht-degree: 0%

---

# Explicación de la aplicación

La aplicación [!DNL Adobe Commerce Traffic Insights] visualiza los registros de acceso sin procesar de la red de distribución de contenido (CDN) en una imagen del tráfico perimetral de una tienda. Los gráficos se agrupan en las siguientes pestañas:

- **Ancho de banda**: Cómo se distribuye el ancho de banda del tráfico entre dominios, tipos de contenido y recursos y proyectos de Cloud a lo largo del tiempo.
- **Rendimiento de caché de página completa**: la eficacia con la que HTML de tienda dinámica para páginas de detalles de producto (PDP), páginas de listas de productos (PLP) y páginas del sistema de administración de contenido (CMS) se almacena en caché en el perímetro de.
- **Análisis de actividad y solicitudes de bots**: tráfico desglosado por agentes de bots conocidos, geolocalización, IP/subredes, URL y señales de Fastly Next-Gen Web Application Firewall (WAF).

Una cuarta ficha de **Documentación** en la aplicación contiene notas conceptuales y el [manual de investigación](investigation-playbook.md).

## ¿Para quién es esta guía?

- **Operadores de sitio e Ingeniería de confiabilidad del sitio (SRE)** que investigan el uso excesivo del ancho de banda de la CDN, los picos de tráfico o la carga de origen.
- **Desarrolladores** que afinan la cobertura de caché de página completa (FPC) y la proporción de visitas o implementan reglas de lenguaje de configuración de barniz rápido (VCL).
- **Administradores e ingenieros de seguridad** que identifican y mitigan bots, raspadores y tráfico automatizado malintencionado no deseados.

Se da por hecho que está familiarizado con [!DNL Adobe Commerce on Cloud Infrastructure], conceptos de Fastly CDN y navegación básica de New Relic.

## Cómo funciona

Seleccione una cuenta y un intervalo de tiempo en los controles de plataforma de la parte superior de la página. Un **ID de proyecto** opcional puede reducir aún más los gráficos a proyectos específicos de la nube. En una configuración de cuenta maestra o asociación, poder ver una cuenta en la lista desplegable no significa que pueda consultarla. Si un gráfico informa de un error de permiso, cambie a una cuenta a la que tenga acceso New Relic Query Language (NRQL).

Sigue aplicando filtros para convertir una visión general en una investigación centrada. Haga clic en un valor de una columna de faceta, como un bot, una dirección IP, una subred, un país o un tipo de contenido, para agregar un [filtro global](https://docs.newrelic.com/docs/query-your-data/explore-query-data/dashboards/filter-new-relic-one-dashboards-facets/#example-use). Los filtros activos aparecen en la parte superior de la cuadrícula y se aplican a todos los widgets de cada pestaña simultáneamente. Para ampliar el ámbito, quite un filtro.

**Tutorial** - Piense en un escenario en el que el *Ancho de banda total* es una tendencia por encima de la asignación de contrato y desea saber quién lo conduce:

1. Abra la pestaña **Análisis de solicitudes y actividad de bots** y lea **Estructura del ancho de banda** para ver cuánto tráfico está automatizado en comparación con el orgánico.
1. Si los bots parecen tener más tráfico, abra **Bots conocidos por ancho de banda** y haga clic en el bot con el nombre más pesado, por ejemplo, un raspador. Esto agrega un nuevo filtro, lo que significa que ahora cada widget tiene ámbitos para ese bot.
1. Lea **Detalles de impacto de bots conocidos** para ver su tasa de solicitudes, combinación de estados y tasa de visitas de FPC.
1. Para ver dónde se origina el bot, marque **Ancho de banda por país**. Para ver lo que está obteniendo el bot, consulte **URL por ancho de banda**.
1. Si el tráfico se concentra en una red, haga clic en **Estadísticas por subredes IP** para confirmar que un actor gira entre direcciones en un solo bloque.
1. Ahora tiene el quién, el qué y el dónde necesarios para escribir una mitigación dirigida. Continúe con el manual de [investigación](investigation-playbook.md) para obtener más información sobre cómo proceder.

El mismo método de filtrado funciona desde cualquier faceta inicial: un país sospechoso, una sola IP, un tipo de contenido o un segmento de ruta de URL.

## Cómo se miden los datos

Entender algunas opciones de medición hace que los números sean más fáciles de confiar e interpretar.

- **Ancho de banda (BW)** es el total de bytes que CDN proporcionó a las solicitudes coincidentes, contando **tanto los encabezados de respuesta como el cuerpo**. Es la métrica de coste de titulares la que cuenta contra la asignación del contrato.
- **Solicitudes (Req.)** es el número de solicitudes distintas, sin embargo, con Fastly [blindaje](https://www.fastly.com/documentation/guides/concepts/shielding/) habilitado, una sola solicitud se registra **dos veces**, una vez en cada una de las siguientes:
  - Escudo interno [Punto de presencia (POP)](https://www.fastly.com/documentation/guides/getting-started/concepts/using-fastlys-global-pop-network/)
  - POP DE EDGE
    Esto sucede a menos que la respuesta provenga directamente de la caché POP local o que el propio escudo actúe como POP para la ubicación del remitente. Para evitar el recuento doble de estos `HIT,MISS` y `MISS,MISS` casos, las consultas de la aplicación se agregan con [`uniqueCount`](https://docs.newrelic.com/docs/nrql/nrql-syntax-clauses-functions/#func-uniqueCount) sobre el campo `request_id`. Esto devuelve una aproximación cercana de **approximation** con un margen de error esperado de **~5%**, no un recuento exacto.
- **Los segmentos de red de CDN** se han comprimido de forma diferente. La respuesta entregada al cliente está comprimida, pero el tráfico de escudo a POP está [no comprimido](https://www.fastly.com/documentation/guides/concepts/compression/#compression-at-the-edge) para conservar la compatibilidad con [Edge Side Includes (ESI)](https://www.fastly.com/documentation/reference/vcl/statements/esi/). Por lo tanto, una proporción baja de visitas de caché infla el segmento interno más que el segmento orientado al cliente, ya que el contenido no almacenado en caché debe extraerse repetidamente a través del escudo a un tamaño completo y sin comprimir. Esta compresión es la razón por la que el widget **Ancho de banda del segmento de red CDN** y la proporción de visitas de FPC son dos vistas del mismo costo subyacente.

## Limitaciones de datos y rendimiento

- **Retención de 30 días**: los registros de CDN de Fastly se conservan en New Relic durante **30 días** por el plan de suscripción. Cualquier ventana que elija debe estar comprendida en los últimos 30 días. Para un ancho de banda de **total** a largo plazo, use la integración directa de Fastly en el panel [!DNL Adobe Commerce admin], **Panel > Fastly > Ancho de banda > Total**, pero considere que informa por ID de servicio, por lo que los datos deben recopilarse por entorno y agregarse para compararlos con la asignación del contrato.
- **Límite de consulta de 60 segundos** - Cada NRQL de gráfico tiene un límite de ejecución de [60 segundos](https://docs.newrelic.com/docs/nrql/using-nrql/rate-limits-nrql-queries/#query-duration). Para cuentas de mucho tráfico, un widget puede agotar el tiempo de espera mientras explora demasiados registros. Si esto sucede, reduzca el intervalo de tiempo y vuelva a cargar los gráficos. Puede expandirlo de nuevo para pestañas más claras.
