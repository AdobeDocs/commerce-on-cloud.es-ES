---
title: Mensajes de error para el paquete ECE-Tools
description: Consulte la lista de códigos de error y mensajes que pueden producirse durante los procesos de creación, implementación y posterior implementación de la infraestructura en la nube de Adobe Commerce.
recommendations: noDisplay
role: Developer
exl-id: e240c268-b171-44e9-9fa4-f0e91b0d9899
source-git-commit: 350cfea06f036f0787b330e6e40c5af46a30f5ad
workflow-type: tm+mt
source-wordcount: '243'
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
