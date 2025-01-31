---
title: Establecer caché para archivos estáticos
description: Obtenga información sobre cómo establecer las opciones de almacenamiento en caché en el archivo de configuración de la aplicación  [!DNL Commerce] .
feature: Cloud, Configuration, Cache, SCD
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '113'
ht-degree: 0%

---

# Establecer caché para archivos estáticos

El TTL de caché (tiempo de vida) para los archivos estáticos y multimedia se establece en el archivo de configuración `.magento.app.yaml` mediante la clave `expires`.

>[!NOTE]
>
>Antes de actualizar el entorno de producción, es importante probar los cambios en el entorno de ensayo. [Envíe un ticket de soporte de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para obtener ayuda con la actualización de la configuración en estos entornos.

1. Especifique el tiempo TTL (en segundos) en la propiedad [`web` ](web-property.md) del archivo `.magento.app.yaml`. Puede agregar la clave `expires` en `locations` o en `"/media"` y `"/static"`.

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
