---
title: Información general sobre archivos de configuración
description: Obtenga información sobre la configuración del entorno de infraestructura en la nube para admitir la implementación y administración de su tienda de Adobe Commerce personalizada.
feature: Cloud, Configuration, Services, Iaas, Paas
exl-id: 305380b0-1920-4037-a1db-80e72c6af333
TQID: 'https://experienceleague.adobe.com/mFjzrTN6R7LC3e9ADnzzulcWAwun4k-g3aCjc9Bo3gQ'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: 5a951749-fac9-5bc7-9a98-ebe4ff066437
    internal-label: Cloud
  - id: da76473c-f99b-5ad0-9b14-896aed473f8a
    internal-label: Services
  - id: 00451af3-7b97-5414-9992-3a6c269e413f
    internal-label: Paas
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
    internal-label: Architecture
subfeature_v2:
  - id: df5e974b-6742-4873-a687-a6bedaafdaa2
    internal-label: IaaS
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
source-git-commit: e6e0bd8e116b2f0b93557b6aeb2aac7cbb8e1d8a
workflow-type: tm+mt
source-wordcount: '286'
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
| [Servicio](../services/services-yaml.md) | `.magento/services.yaml` | Define los servicios que utiliza Adobe Commerce por nombre y versión. Por ejemplo, este archivo puede incluir versiones de MariaDB, extensiones PHP, Redis o Valkey, RabbitMQ y Elasticsearch o OpenSearch. Debe abrir un ticket de asistencia para insertar estos cambios en los entornos de ensayo y producción de planificación profesional. |
| [Configuración de PHP](../application/php-settings.md#configure-php) | `php.ini` | Archivo opcional que se puede agregar al proyecto. La configuración contenida en este archivo se anexa a la que mantiene la infraestructura en la nube. |

{style="table-layout:auto"}

## Actualizaciones de configuración para entornos Pro

Para los entornos de ensayo y producción de Adobe Commerce en la infraestructura en la nube Pro, puede actualizar muchas opciones de configuración en su entorno de desarrollo local y confirmar los cambios para aplicarlos a estos entornos. Sin embargo, debe [enviar un ticket de soporte de Adobe Commerce](https://experienceleague.adobe.com/es/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket) para actualizar las siguientes opciones de configuración:

- Instale o actualice los servicios en el archivo `.magento/services.yaml`.
- Cambie la configuración de las propiedades `mounts` y `disk` en el archivo `.magento.app.yaml`.

{{pro-self-service-warning}}
