---
title: Propiedad Variables
description: Utilice la propiedad variables para personalizar las opciones de configuración del almacén para la aplicación  [!DNL Commerce] .
feature: Cloud, Configuration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '72'
ht-degree: 0%

---

# Propiedad Variables

Puede utilizar variables de entorno basadas en aplicaciones para personalizar las configuraciones de tienda. Estas variables utilizan una sintaxis específica. Consulte [Anular la configuración](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/paths/override-config-settings.html) en la _Guía de configuración_.

Las siguientes variables de entorno incluidas en el archivo `.magento.app.yaml` son necesarias para versiones específicas de la aplicación [!DNL Commerce].

Necesario para Adobe Commerce 2.2.x a 2.3.x:

```yaml
variables:
    env:
        CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
        CONFIG__STORES__DEFAULT__PAYMENT__BRAINTREE__CHANNEL: 'Magento_Enterprise_Cloud_BT'
        CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
```

Para Adobe Commerce 2.4.x, configure las siguientes variables:

```yaml
variables:
    env:
        CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
        CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
```
