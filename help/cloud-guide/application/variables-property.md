---
title: Propiedad Variables
description: Utilice la propiedad variables para personalizar las opciones de configuración del almacén para la aplicación  [!DNL Commerce] .
feature: Cloud, Configuration
exl-id: 9909d4a1-d9c8-492b-a5cc-cfbf17b5b7a8
TQID: https://experienceleague.adobe.com/bNdcqNxAAE1E1Qe2C-y1LWpFX-8Kzuwc9rJYEnbRz0U
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: d863fc70609dcc66d21eb95e709db80e29114714
workflow-type: tm+mt
source-wordcount: 85
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

