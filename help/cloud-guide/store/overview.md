---
title: Información general sobre las opciones de tienda y la administración de configuración
description: Personalice su tienda Adobe Commerce en la infraestructura en la nube.
feature: Cloud, Configuration, Services
exl-id: e653172f-7370-4761-b2ce-3a420b33b948
TQID: https://experienceleague.adobe.com/iseYcfjh61-4ArUf9rKBGKFVuOz1dbS1xVq-FYiCxsw
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 193
ht-degree: 0%

---

# Información general sobre las opciones de tienda y la administración de configuración

Existen muchas formas de personalizar la tienda, como añadir una temática personalizada, instalar una extensión o aplicar una configuración específica en entornos de infraestructura en la nube. Puede configurar las opciones de servicios específicos directamente en los entornos de ensayo y producción. Puede configurar varios sitios web y tiendas. La configuración de Almacenamiento le ayuda a configurar estas opciones en su estación de trabajo local e implementar configuraciones específicas en entornos.

Para acceder a tu tienda, usa el comando `magento-cloud url` y responde a las indicaciones. O puede encontrar la dirección URL en [!DNL Cloud Console] en **Acceso al sitio**.

## Configurar las opciones de tienda

Las opciones de tienda son las siguientes:

* [Módulo de empresa a empresa (B2B)](b2b-module.md)
* [Temas personalizados](custom-theme.md)
* [Extensiones](extensions.md)
* [Varios sitios](multiple-sites.md)
* [Servicios de pago](paypal.md)

## Configuración de servicios e integraciones

Hay [archivos de configuración](../environment/overview.md) específicos que administran ciertos comportamientos de implementación en entornos remotos. Puede revisar estos temas por separado:

* [Implementación de aplicaciones](../application/configure-app-yaml.md)
* [Generar e implementar acciones del entorno](../environment/configure-env-yaml.md)
* [Rutas de solicitud entrantes](../routes/routes-yaml.md)
* [Servicios compatibles](../services/services-yaml.md)

## Administración de configuración

Después de configurar las opciones, los servicios y las integraciones de la tienda, utilice la administración de la configuración para implementar estas configuraciones en todos los entornos de forma coherente y con un tiempo de inactividad mínimo. Consulte [Administración de configuración](store-settings.md).
