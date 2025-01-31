---
title: Información general sobre archivos de configuración
description: Obtenga información sobre la configuración del entorno de infraestructura en la nube para admitir la implementación y administración de su tienda de Adobe Commerce personalizada.
feature: Cloud, Configuration, Services, Iaas, Paas
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 0%

---

# Información general sobre archivos de configuración

Los entornos de Adobe Commerce en la infraestructura en la nube incluyen contenedores con aplicaciones, servicios y una base de datos para proporcionar un sistema completo para el código base y los archivos de la aplicación de Adobe Commerce.

Puede configurar la configuración de la aplicación, las rutas, las acciones de generación e implementación y las notificaciones para que sean compatibles con los entornos de proyecto mediante los siguientes archivos de configuración:

| Configuración | Nombre de archivo | Descripción |
| ------------- | -------- | ----------- |
| [Aplicación](../application/configure-app-yaml.md) | `.magento.app.yaml` | Define cómo generar e implementar Adobe Commerce, incluidos servicios, vínculos y trabajos cron. |
| [Entorno](configure-env-yaml.md) | `.magento.env.yaml` | Centraliza la administración de acciones de compilación e implementación en todos sus entornos, incluidos el ensayo y la producción profesionales, mediante variables de entorno. |
| [Rutas](../routes/routes-yaml.md) | `.magento/routes.yaml` | Configure el almacenamiento en caché, las redirecciones y las inclusiones del lado del servidor. |
| [Servicio](../services/services-yaml.md) | `.magento/services.yaml` | Define los servicios que utiliza Adobe Commerce por nombre y versión. Por ejemplo, este archivo puede incluir versiones de MariaDB, extensiones PHP, Redis, RabbitMQ y Elasticsearch o OpenSearch. Debe abrir un ticket de asistencia para insertar estos cambios en los entornos de ensayo y producción de planificación profesional. |
| [Configuración de PHP](../application/php-settings.md#configure-php) | `php.ini` | Archivo opcional que se puede agregar al proyecto. La configuración contenida en este archivo se anexa a la que mantiene la infraestructura en la nube. |

{style="table-layout:auto"}

## Actualizaciones de configuración para entornos Pro

Para los entornos de ensayo y producción de Adobe Commerce en la infraestructura en la nube Pro, puede actualizar muchas opciones de configuración en su entorno de desarrollo local y confirmar los cambios para aplicarlos a estos entornos. Sin embargo, debe [enviar un ticket de soporte de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para actualizar las siguientes opciones de configuración:

- Instale o actualice los servicios en el archivo `.magento/services.yaml`.
- Cambie la configuración de las propiedades `mounts` y `disk` en el archivo `.magento.app.yaml`.

{{pro-self-service-warning}}
