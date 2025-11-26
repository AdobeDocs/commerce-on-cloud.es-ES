---
title: Información general de desarrollo
description: Prepárese para el desarrollo local con un proyecto de Adobe Commerce en la nube.
role: Developer
feature: Cloud, Install
topic: Development
last-substantial-update: 2024-02-06T00:00:00Z
exl-id: 14fb0b41-1c3a-4abc-8726-cea16ab00ba8
source-git-commit: 0d84d29c470a098c7238b6ca7cc9538463dda695
workflow-type: tm+mt
source-wordcount: '560'
ht-degree: 0%

---

# Información general de desarrollo

Los entornos remotos de Adobe Commerce en la infraestructura de la nube son de **solo lectura**, e incluyen todos los entornos de inicio y todos los entornos de integración, ensayo y producción de Pro. En un entorno de desarrollo local, puede escribir y probar el código antes de insertarlo en un entorno de integración para realizar más pruebas e implementaciones en Ensayo y producción.

Antes de preparar el área de trabajo local, asegúrese de que dispone de sus [credenciales](../../get-started/prepare-workspace.md). El desarrollo local requiere la instalación de PHP y Composer a menos que decida usar [Cloud Docker para Commerce](#docker-environment).

## Paquetes necesarios

Adobe Commerce en la infraestructura en la nube usa Composer para administrar las dependencias y actualizaciones de los proyectos. Para el desarrollo local, debe instalar las versiones de PHP y Composer compatibles con su proyecto en la nube. Por ejemplo, si está usando la plantilla de nube [!DNL Commerce] 2.4.8, puede ver que el archivo de configuración [`.magento.app.yaml`](https://github.com/magento/magento-cloud/blob/2.4.8/.magento.app.yaml) usa **PHP 8.4** y **Composer 2.8.4**.

Composer instala las bibliotecas y dependencias necesarias para su proyecto en el directorio `vendor`. Los siguientes ficheros de composición requeridos se encuentran en el directorio raíz del proyecto:

- `composer.json`: utilice el archivo `composer.json` para administrar las instalaciones y actualizaciones de productos.
- `composer.lock`: el archivo `composer.lock` almacena un conjunto de dependencias de versión exactas que cumplen las restricciones de versión de cada requisito para cada paquete en el árbol de dependencias del proyecto.

**Comandos comunes:**

| Comando | Descripción |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| `composer update` | Actualizaciones a las últimas versiones de las dependencias reflejadas en el archivo `composer.json`. Esto actualiza el archivo `composer.lock`. |
| `composer install` | Lee el archivo `composer.lock` para descargar dependencias. Se recomienda mantener una copia actualizada de `composer.lock` en el repositorio del proyecto. |

{style="table-layout:auto"}

Una vez que agregue, confirme e inserte el código actualizado, el proceso de implementación ejecutará automáticamente el comando `composer install` durante la [fase de compilación](../deploy/process.md#build-phase-build-phase).

### Metapaquete de nube

Adobe Commerce en la infraestructura en la nube usa un metapaquete que requiere `magento/product-enterprise-edition`. Para obtener las últimas actualizaciones de la última versión de Commerce, utilice la siguiente sintaxis de restricción:

```text
>=current_version <next_version
```

Por ejemplo, para usar la última versión de Adobe Commerce 2.4.9, establezca `2.4.8` como la versión &quot;actual&quot; y `2.4.9` como la &quot;siguiente&quot; versión en el archivo `composer.json`:

```text
"magento/magento-cloud-metapackage": ">=2.4.8 <2.4.9"
```

Los paquetes principales de este metapaquete son los siguientes:

- **supplier/magento/ece-tools**: el paquete `ece-tools` es compatible con la versión 2.1.4 y posterior de Adobe Commerce para proporcionar un completo conjunto de características que puede utilizar para administrar su proyecto de infraestructura en la nube de Adobe Commerce. Contiene scripts y comandos de infraestructura de Adobe Commerce en la nube diseñados para ayudarle a administrar su código y generar e implementar automáticamente sus proyectos. Consulte la descripción general del paquete [`ece-tools`](../dev-tools/package-overview.md).
- **proveedor/magento/product-enterprise-edition**: este metapaquete requiere componentes de aplicación, incluidos módulos, marcos, temas y mucho más.
- **proveedor/fastly2/magento2**: este módulo administra la CDN y los servicios de Fastly para los entornos de ensayo profesional, producción y producción inicial. Ver [Servicios rápidos](/help/cloud-guide/cdn/fastly.md#fastly-cdn-module-for-magento-2).
- **proveedor/magento/módulo-paypal-on-boarding**: este módulo proporciona un pago de puerta de enlace de pago mediante conexión a tu cuenta de PayPal. Ver [Herramienta de incorporación a PayPal](../store/paypal.md).
- **proveedor/aem/rum**: este módulo administra la herramienta de recopilación de datos [Telemetría operativa](../monitor/operational-telemetry.md).

>[!TIP]
>
>Consulte [Paquetes de nube para Adobe Commerce](/help/cloud-guide/release-notes/cloud-packages.md) en las _Notas de la versión de Commerce_ para obtener una lista de dependencias y licencias de terceros.

## Entorno Docker

Puede utilizar la herramienta Cloud Docker para Commerce para emular Adobe Commerce en entornos de producción y desarrollo de infraestructura en la nube para el desarrollo local. Cloud Docker para Commerce no requiere que PHP y Composer se instalen localmente.

- [Desarrollo local con Cloud Docker](https://developer.adobe.com/commerce/cloud-tools/docker/setup/) en el sitio de Adobe Developer
- [Arquitectura de Docker y comandos comunes](../dev-tools/cloud-docker.md)
- [Notas de la versión de Cloud Docker](../release-notes/cloud-docker.md)

>[!TIP]
>
>Para obtener información sobre cómo usar los servicios de alojamiento basados en Git con Adobe Commerce en la infraestructura en la nube, consulte [Integraciones](../integrations/overview.md).
