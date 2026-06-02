---
title: Mensajes de error para el paquete ECE-Tools
description: Consulte la lista de códigos de error y mensajes que pueden producirse durante los procesos de creación, implementación y posterior implementación de la infraestructura en la nube de Adobe Commerce.
recommendations: noDisplay
role: Developer
exl-id: e240c268-b171-44e9-9fa4-f0e91b0d9899
TQID: https://experienceleague.adobe.com/FpWmyt0POi3RgKQOf6ihQQVV6GazxRByu-4HdPIhTPY
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7id: f42e0a1a-0d79-488d-a83f-f2c30672b137
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 253
ht-degree: 0%

---

# Mensajes de error para ECE-Tools

Esta referencia de mensaje de error proporciona información para solucionar errores que pueden producirse durante los procesos de creación, implementación y posterior a la implementación de Adobe Commerce en la infraestructura en la nube.

Todos los mensajes de error críticos y de advertencia que se producen durante la implementación se escriben en los archivos `var/log/cloud.log` y `/var/log/cloud.error.log`. El archivo de registro de errores de la nube solo contiene errores de la implementación más reciente. Un archivo vacío indica una implementación correcta sin errores.

En el archivo `cloud.error.log`, cada entrada tiene el formato de una cadena JSON para facilitar el análisis:

```json
{"errorCode":1006,"stage":"build","step":"validate-config","suggestion":"No stores/website/locales found in config.php\n  To speed up the deploy process do the following:\n  1. Using SSH, log in to your Magento Cloud account\n  2. Run \"php ./vendor/bin/ece-tools config:dump\"\n  3. Using SCP, copy the app/etc/config.php file to your local repository\n  4. Add, commit, and push your changes to the app/etc/config.php file","title":"The configured state is not ideal","type":"warning"}
```

Los mensajes de error se clasifican por una de las etapas de implementación: generación, implementación y posterior a la implementación. Cada sección proporciona una lista de errores asociados con la siguiente información para cada error:

- **Código de error**: El identificador asignado por Adobe Commerce para el mensaje de error
- **Fase**: indica si el error se produjo durante la fase de compilación, implementación o posterior a la implementación
- **Paso**: indica el paso en el escenario de implementación que puede devolver el error. Si la columna _Step_ está en blanco, se trata de un error general que se puede devolver mediante varios pasos o durante las operaciones de preprocesamiento. Consulte [Implementación basada en escenarios](../deploy/scenario-based.md) para obtener más información sobre los pasos de compilación, implementación y posterior a la implementación.
- **Sugerencia**: Proporciona instrucciones para solucionar y resolver el error
- **Título (descripción del error)**: una descripción que resume la causa del error
- **Tipo**: indica si el error es un error crítico o una advertencia

{{$include /help/_includes/automated/ece-tools-error-codes.md}}

<!-- Last updated from includes: 2025-05-28 21:01:41 -->
