---
title: Arquitectura de nube para Commerce
description: Descubra cómo las arquitecturas de proyectos Starter y Pro se contrastan para Commerce en la infraestructura en la nube.
feature: Cloud, Iaas, Paas
topic: Architecture
recommendations: noDisplay
exl-id: 7c1e895d-0f88-4f11-919a-b3b5748ca5f0
source-git-commit: 5fc2082ca2aae8a1466821075c01ce756ba382cc
workflow-type: tm+mt
source-wordcount: '746'
ht-degree: 0%

---

# Arquitectura de nube para Commerce

Adobe Commerce en la infraestructura en la nube tiene un plan Starter y un plan Pro. Cada plan tiene una arquitectura única para dirigir el proceso de desarrollo e implementación de Adobe Commerce. Tanto el plan Starter como la arquitectura del plan Pro implementan bases de datos, servidores web y servidores de almacenamiento en caché en varios entornos para realizar pruebas de extremo a extremo y, al mismo tiempo, admiten la integración continua.

Para comparar, cada plan incluye las siguientes funciones de infraestructura y productos compatibles.

|          | Starter | Pro |
| -------- | --------------------| ------------------ |
| Funciones principales | <ul><li>[Todas las características de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-operations/release/features.html?lang=es)</li><li>Herramienta de incorporación de PayPal</li><li>[Informes de Commerce](https://business.adobe.com/es/products/magento/business-intelligence.html?_ga=2.85288604.442698376.1665067470-1322106587.1655147209)</li></ul> | <ul><li>[Todas las características de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-operations/release/features.html?lang=es)</li><li>Herramienta de incorporación de PayPal</li><li>[Informes de Commerce](https://business.adobe.com/es/products/magento/business-intelligence.html?_ga=2.85288604.442698376.1665067470-1322106587.1655147209)</li><li>[Módulo B2B](https://business.adobe.com/es/products/magento/b2b-ecommerce.html?_ga=2.105948422.442698376.1665067470-1322106587.1655147209)</li></ul> |
| Infraestructura e implementación | <ul><li>Herramientas de integración continua en la nube con usuarios ilimitados</li><li>Rápidamente, la red de distribución de contenido (CDN), la optimización de imágenes (IO) y la seguridad añadida con generosas asignaciones de ancho de banda. El servicio Web Application Firewall (WAF) solo está disponible en entornos de producción.</li><li>[New Relic](../monitor/new-relic-service.md) APM (supervisión del rendimiento) en 3 ramas: `master` y 2 de su elección<br>Entornos de producción, ensayo y desarrollo de Platform as a service (PaaS) optimizados para Adobe Commerce</li><li>Filtrado de salidas (cortafuegos de salida)</li></ul> | <ul><li>Herramientas de integración continua en la nube con usuarios ilimitados</li><li>Rápidamente, la red de distribución de contenido (CDN), la optimización de imágenes (IO) y la seguridad añadida con generosas asignaciones de ancho de banda. El servicio Web Application Firewall (WAF) solo está disponible en entornos de producción.</li><li>Infraestructura de [New Relic](../monitor/new-relic-service.md) en producción + APM (supervisión del rendimiento) en ensayo y producción. La directiva [Alertas administradas](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts) para Adobe Commerce implementa prácticas recomendadas de supervisión para notificarle de forma proactiva los problemas de infraestructura y aplicaciones que afectan el rendimiento del sitio.</li><li>Entornos [desarrollo de integración](pro-architecture.md#integration-environment) basados en la plataforma como servicio (PaaS) (2 entornos activos totales) optimizados para Adobe Commerce</li><li>Infraestructura como servicio (IaaS): infraestructura virtual dedicada para entornos de ensayo y producción</li></ul> |
| Infraestructura de alta disponibilidad | | [Arquitectura de alta disponibilidad](pro-architecture.md#redundant-hardware) con una configuración de tres servidores en la infraestructura como servicio (IaaS) subyacente para proporcionar confiabilidad y disponibilidad de nivel empresarial |
| Hardware dedicado | | Hardware aislado y dedicado en la infraestructura como servicio (IaaS) subyacente para proporcionar niveles aún más altos de fiabilidad y disponibilidad |
| asistencia por correo electrónico ininterrumpida | Monitorización y asistencia por correo electrónico ininterrumpidas para la aplicación principal y la infraestructura en la nube | Monitorización y asistencia por correo electrónico ininterrumpidas para la aplicación principal y la infraestructura en la nube |
| Un asesor técnico para clientes (CTA) | | Administración de cuentas técnicas dedicadas para el período de inicio, empezando por la suscripción hasta el inicio del sitio |
| Complementos\* | <ul><li>[Módulo B2B](https://business.adobe.com/es/products/magento/b2b-ecommerce.html)</li></ul> |

\* _Disponible por una tarifa adicional_

## Proyectos iniciales

La [arquitectura del plan inicial](starter-architecture.md) tiene cuatro entornos:

- **Integración**: el entorno de integración proporciona dos entornos comprobables. Cada entorno incluye una rama de Git activa, una base de datos, un servidor web, almacenamiento en caché, algunos servicios, variables de entorno y configuraciones.

- **Ensayo**: a medida que el código y las extensiones pasan las pruebas, puede combinar la rama `integration` con el entorno de ensayo, que se convertirá en el entorno de prueba previo a la producción. Incluye la rama activa `staging`, la base de datos, el servidor web, el almacenamiento en caché, los servicios de terceros, las variables de entorno, las configuraciones y los servicios, como Fastly y New Relic.

- **Producción**: cuando el código está listo y probado, todo el código se combina con `master` para su implementación en el sitio de producción activo. Este entorno incluye su rama `master` activa, base de datos, servidor web, almacenamiento en caché, servicios de terceros, variables de entorno y configuraciones.

- **Inactivo**: tiene un número ilimitado de ramas inactivas.

## Proyectos Pro

La [arquitectura de plan profesional](pro-architecture.md) tiene un `master` global con tres entornos:

- **Integración**: el entorno de integración proporciona un entorno de prueba que incluye una base de datos, un servidor web, almacenamiento en caché, algunos servicios, variables de entorno y configuraciones. Puede desarrollar, implementar y probar el código antes de combinarlo con el entorno de ensayo.

   - _Inactiva_: puede tener un número ilimitado de ramas inactivas basadas en el entorno `integration`, pero solo una rama activa (sin incluir `integration` ).

- **Ensayo**: el entorno de ensayo es para pruebas previas a la producción e incluye una base de datos, un servidor web, almacenamiento en caché, servicios de terceros, variables de entorno, configuraciones y servicios, como Fastly.

- **Producción**: el entorno de producción incluye una arquitectura de alta disponibilidad de tres nodos para los datos, los servicios, el almacenamiento en caché y el almacén. La producción es su entorno de tienda pública activo con variables de entorno, configuraciones y servicios de terceros.

## Software y servicios compatibles

Adobe Commerce en la infraestructura en la nube utiliza:

- Sistema operativo: Debian GNU/Linux
- Servidor web: Nginx
- Base de datos: MySQL (MariaDB)
- Red de entrega de contenido (CDN): Fastly CDN

Puede configurar los siguientes servicios:

- [PHP](../application/php-settings.md)
- [MySQL](../services/mysql.md)
- [Redis](../services/redis.md)
- [RabbitMQ](../services/rabbitmq.md)
- [ActiveMQ](../services/activemq.md)
- [Elasticsearch](../services/elasticsearch.md)
- [OpenSearch](../services/opensearch.md)

{{elasticsearch-support}}

>[!NOTE]
>
>Consulte [Requisitos del sistema](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html?lang=es) en la _Guía de instalación_ para ver las versiones recomendadas.

El módulo Fastly CDN se utiliza para los servicios de CDN y almacenamiento en caché en entornos de ensayo y producción. Consulte [Configurar servicios de Fastly](../cdn/fastly.md).

Para obtener información sobre cómo configurar las versiones de software que se utilizarán en la implementación, consulte los siguientes archivos de configuración de Adobe Commerce en la infraestructura en la nube:

- [Configuración de la aplicación (.magento.app.yaml)](../application/configure-app-yaml.md)
- [Configuración del entorno (.magento.env.yaml)](../environment/configure-env-yaml.md)
- [Configuración de rutas (routes.yaml)](../routes/routes-yaml.md)
- [Configuración de servicios (services.yaml)](../services/services-yaml.md)
