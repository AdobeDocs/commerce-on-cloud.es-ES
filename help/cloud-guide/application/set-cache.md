---
title: Establecer caché para archivos estáticos
description: Obtenga información sobre cómo establecer las opciones de almacenamiento en caché en el archivo de configuración de la aplicación  [!DNL Commerce] .
feature: Cloud, Configuration, Cache, SCD
exl-id: 0f577974-85d7-4972-8f03-856aa6accaae
TQID: https://experienceleague.adobe.com/ZA0WRB9p4Gpi7kjWxNPS5uCSfaTmrkrqxc4SgC9y9og
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: d863fc70609dcc66d21eb95e709db80e29114714
workflow-type: tm+mt
source-wordcount: 131
ht-degree: 0%

---

# Establecer caché para archivos estáticos

El TTL de caché (tiempo de vida) para los archivos estáticos y multimedia se establece en el archivo de configuración `.magento.app.yaml` mediante la clave `expires`.

>[!NOTE]
>
>Antes de actualizar el entorno de producción, es importante probar los cambios en el entorno de ensayo. [Envíe un ticket de soporte de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para obtener ayuda con la actualización de la configuración en estos entornos.

1. Especifique el tiempo TTL (en segundos) en la propiedad [`web` &#x200B;](web-property.md) del archivo `.magento.app.yaml`. Puede agregar la clave `expires` en `locations` o en `"/media"` y `"/static"`.

   Para evitar que la caché caduque, use el par clave-valor `expires: -1`. Consulte el siguiente ejemplo:

   ```yaml
   # The configuration of app when it is exposed to the web.
   web:
     locations:
       "/media":
         ...
         expires: -1
   
       "/static":
         ...
         expires: -1
   ```

1. Agregue, confirme e inserte los cambios de código.

   ```bash
   git add -A && git commit -m "Set cache TTL for static files" && git push origin <branch-name>
   ```

