---
title: Configurar la implementación de aplicaciones
description: Obtenga información sobre cómo configurar las propiedades del archivo de configuración de la aplicación que controlan la forma en que la aplicación [!DNL Commerce] se genera e implementa en el entorno de nube.
feature: Cloud, Configuration, Build, Deploy
exl-id: 47dcb13f-8873-495d-956f-08a5e04844d9
TQID: 'https://experienceleague.adobe.com/LCTu-HeJCO1pp9trlZ7h74CxSQtyBr5SDmYP9DgCtkU'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: 5a951749-fac9-5bc7-9a98-ebe4ff066437
    internal-label: Cloud
  - id: bcd8874c-7b93-5596-bdaa-22660e84df14
    internal-label: Deploy
subfeature_v2:
  - id: adedf3b3-e153-47a3-ae73-b5d65067b544
    internal-label: Build system
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
source-git-commit: e6e0bd8e116b2f0b93557b6aeb2aac7cbb8e1d8a
workflow-type: tm+mt
source-wordcount: '191'
ht-degree: 0%
---
# Configurar la implementación de aplicaciones

El archivo `.magento.app.yaml` controla la forma en que se genera e implementa la aplicación. Aunque Adobe Commerce en la infraestructura de la nube admite varias aplicaciones por proyecto, normalmente un proyecto tiene una sola aplicación con el archivo `.magento.app.yaml` en la raíz del repositorio.

`.magento.app.yaml` tiene muchos valores predeterminados, vea [un archivo de muestra `.magento.app.yaml`](https://github.com/magento/magento-cloud/blob/master/.magento.app.yaml). Revise siempre `.magento.app.yaml` para la versión instalada. Este archivo puede diferir según la Adobe Commerce en las versiones de infraestructura en la nube.

Utilice el archivo `.magento.app.yaml` para definir los siguientes valores de configuración:

- [Propiedades](properties.md): defina los valores de propiedad para la instancia de aplicación.
- [Variables property](variables-property.md): revise las variables de entorno necesarias para la versión de la aplicación [!DNL Commerce].
- [Configuración de PHP](php-settings.md): configure las opciones de PHP en tiempo de ejecución.
- [Establecer caché para archivos estáticos](set-cache.md): establezca el TTL de caché para los archivos estáticos y multimedia.

>[!NOTE]
>
>El archivo `.magento.app.yaml` se administra localmente o en el repositorio de Git. La configuración solo se lee con el propósito de implementar y generar el proceso, y se elimina después de completar la implementación, por lo que no la encontrará en el servidor.

