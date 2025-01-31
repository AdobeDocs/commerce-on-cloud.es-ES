---
title: Variables de entorno
description: Consulte una lista de variables de entorno específicas de Adobe Commerce en la infraestructura en la nube.
feature: Cloud, Build, Configuration, Deploy
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 0%

---

# Variables de entorno

Adobe Commerce en la infraestructura de la nube le permite asignar variables de entorno para anular las opciones de configuración. El paquete `ece-tools` establece valores en el archivo `env.php` en función de los valores de [variables de nube](variables-cloud.md), las variables establecidas en [!DNL Cloud Console] y el archivo de configuración `.magento.env.yaml`.

Las variables de entorno del archivo `.magento.env.yaml` personalizan el entorno de la nube al anular la configuración de Commerce existente. Si un valor predeterminado es `Not Set`, el paquete `ece-tools` toma la acción **NO** y usa el valor predeterminado [!DNL Commerce] o el valor de la configuración `MAGENTO_CLOUD_RELATIONSHIPS`. Si se establece el valor predeterminado, el paquete `ece-tools` actuará para establecerlo.

Los tipos de variables de entorno incluyen:

- [ADMIN](variables-admin.md): las variables anulan las variables de ADMIN del proyecto
- [MAGENTO_CLOUD](variables-cloud.md): variables específicas de la infraestructura en la nube
- Variables utilizadas en el archivo `.magento.env.yaml`:
   - [Global](variables-global.md): las variables afectan a las fases de compilación, implementación y posteriores a la implementación
   - [Build](variables-build.md): las variables controlan las acciones de compilación
   - [Implementar](variables-deploy.md): las variables controlan las acciones de implementación
   - [Después de la implementación](variables-post-deploy.md): las variables controlan las acciones después de la implementación

Las variables son _jerárquicas_, lo que significa que si una variable no se reemplaza, se hereda del entorno principal.

Puede establecer [variables ADMIN](variables-admin.md) desde [!DNL Cloud Console] o usar la CLI de Adobe Commerce. Puede administrar otras variables de entorno desde el archivo [`.magento.env.yaml`](configure-env-yaml.md) para administrar las acciones de compilación e implementación en todos los entornos (incluidos el ensayo y la producción de Pro) sin requerir un vale de soporte.

>[!TIP]
>
>Los archivos YAML distinguen entre mayúsculas y minúsculas y no permiten tabulaciones. Tenga cuidado de utilizar una sangría uniforme en todo el archivo `.magento.env.yaml`; de lo contrario, es posible que la configuración no funcione según lo esperado. Los ejemplos de esta documentación y del archivo de muestra utilizan la sangría _two-space_. Use [ece-tools validate command](configure-env-yaml.md#validate-configuration-file) para comprobar la configuración.
