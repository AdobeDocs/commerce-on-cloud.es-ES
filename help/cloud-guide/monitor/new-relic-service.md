---
title: servicio de New Relic
description: Obtenga información acerca del servicio New Relic disponible con su proyecto de Adobe Commerce en la nube.
feature: Cloud, Observability
last-substantial-update: 2023-09-06T00:00:00Z
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 0%

---

# Información general del servicio New Relic

Todos los proyectos de Adobe Commerce en la infraestructura en la nube incluyen acceso al servicio New Relic para supervisar el rendimiento e investigar los eventos de la aplicación [!DNL Commerce] y la infraestructura en la nube.

Las siguientes funciones de New Relic están disponibles para su uso con los entornos de producción y ensayo:

- [New Relic APM](#new-relic-apm) (Pro y Starter)
- [Infraestructura de New Relic](#new-relic-infrastructure) (solo Pro)
- [Administración de registros de New Relic](#new-relic-log-management) (solo Pro)

>[!INFO]
>
>Otras funciones de New Relic no están disponibles en proyectos de Adobe Commerce.

## APM NEW RELIC

[New Relic for application performance management (APM)](https://docs.newrelic.com/introduction-apm/) es un producto de análisis de software que le ayuda a analizar y mejorar las interacciones entre las aplicaciones. New Relic APM está disponible para todos los proyectos de infraestructura en la nube de Adobe Commerce y proporciona las siguientes funciones:

- **Céntrese en transacciones específicas**: marque y supervise de forma activa las acciones clave del cliente en su sitio, como agregar al carro de compras, retirar o procesar un pago.
- **Supervisión de consultas de base de datos**: busque y supervise consultas de base de datos que afecten al rendimiento.
- **Mapa de aplicaciones**: vea todas las dependencias de aplicaciones dentro del sitio, las extensiones y los servicios externos.
- **[!DNL Apdex]puntuaciones**: evalúe el rendimiento y cree alertas que identifiquen problemas y le notifiquen cuando ocurran, como el rendimiento del sitio afectado por una venta flash o un evento web. Ver [Puntuación Apdex](https://docs.newrelic.com/docs/apm/new-relic-apm/apdex/apdex-measure-user-satisfaction/).
- **Alertas administradas para Adobe Commerce**: utilice esta directiva de alertas de New Relic para supervisar el rendimiento de la infraestructura y la aplicación en función de las prácticas recomendadas del sector. Ver [Supervisar el rendimiento con las alertas administradas para la directiva de alertas de Adobe Commerce](investigate-performance.md/#monitor-performance-with-managed-alerts).
- **Seguimiento de implementaciones**: supervise los eventos de implementación y analice el impacto de la implementación en el rendimiento general. Consulte [Implementaciones de seguimiento](track-deployments.md).

Su proyecto de infraestructura de Adobe Commerce en la nube incluye el software para el servicio APM de New Relic junto con una clave de licencia. No es necesario adquirir ni instalar ningún software adicional.

## Infraestructura de New Relic

Los proyectos profesionales incluyen el servicio [Infraestructura de New Relic (NRI)](https://docs.newrelic.com/docs/infrastructure/infrastructure-monitoring/get-started/get-started-infrastructure-monitoring/), que se conecta automáticamente con los datos de la aplicación y el análisis de rendimiento para proporcionar supervisión dinámica del servidor. Este servicio está disponible en entornos de ensayo y producción profesional.

## New Relic Log Management

Todos los proyectos de infraestructura en la nube incluyen [administración de registros de New Relic](log-management.md). El servicio está preconfigurado para agregar todos los datos de registro de los entornos de ensayo y producción y mostrarlos en un panel de administración de registros centralizado.
