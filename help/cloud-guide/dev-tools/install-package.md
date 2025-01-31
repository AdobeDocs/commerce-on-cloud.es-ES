---
title: Actualizar proyecto para utilizar ECE-Tools
description: Aprenda a actualizar su proyecto de infraestructura de Adobe Commerce en la nube para utilizar el paquete ECE-Tools y aprovechar las últimas correcciones y funciones.
feature: Cloud, Install
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '365'
ht-degree: 0%

---

# Actualizar proyecto para utilizar el paquete ECE-Tools

El Adobe ha desaprobado los paquetes `magento/magento-cloud-configuration` y `magento/ece-patches` en favor del paquete `ece-tools`, lo que simplifica muchos procesos de la nube. Si usa un proyecto de infraestructura de Adobe Commerce en la nube más antiguo que _no_ contiene el paquete `ece-tools`, debe realizar un único proceso de _actualización_ manual del proyecto.

>[!WARNING]
>
>Si el proyecto contiene el paquete `ece-tools`, puede omitir la siguiente actualización. Para comprobarlo, recupere la versión [!DNL Commerce] mediante el comando `php vendor/bin/ece-tools -V` en el directorio raíz del proyecto local.

Este proceso de actualización de proyecto requiere que actualice la restricción de versión `magento/magento-cloud-metapackage` en el archivo `composer.json` en el directorio raíz. Esta restricción permite actualizar Adobe Commerce en los metapaquetes de infraestructura en la nube (incluida la eliminación de paquetes obsoletos) sin actualizar la versión actual de Adobe Commerce.

{{upgrade-tip}}

## Eliminación de paquetes obsoletos

Antes de realizar una actualización para usar el paquete `ece-tools`, busque los siguientes paquetes obsoletos en el archivo `composer.lock`:

- `magento/magento-cloud-configuration`
- `magento/ece-patches`

## Actualización del metapaquete

Cada versión de Adobe Commerce requiere una restricción diferente en función de lo siguiente:

```
>=current_version <next_version
```

- Para `current_version`, especifique la versión de Adobe Commerce que desea instalar.
- Para `next_version`, especifique la siguiente versión del parche después del valor especificado en `current_version`.

Si desea instalar Adobe Commerce `2.3.5-p2`, establezca `current_version` en `2.3.5` y `next_version` en `2.3.6`. La restricción `">=2.3.5 <2.3.6"` instala el último paquete disponible para 2.3.5.

Siempre puede encontrar la restricción de metapackage más reciente en la plantilla [`magento-cloud` ](https://github.com/magento/magento-cloud/blob/master/composer.json).

El siguiente ejemplo coloca una restricción para el metapaquete de infraestructura en la nube de Adobe Commerce en cualquier versión superior o igual a la versión actual 2.4.7 e inferior a la siguiente versión 2.4.8:

```json
"require": {
    "magento/magento-cloud-metapackage": ">=2.4.7 <2.4.8"
},
```

## Actualizar el proyecto

Para actualizar el proyecto de modo que utilice el paquete `ece-tools`, debe actualizar las propiedades del metapaquete y los vínculos `.magento.app.yaml`, y realizar una actualización del Compositor.

**Para actualizar el proyecto y usar ece-tools**:

1. Actualizar la restricción de versión `magento/magento-cloud-metapackage` en el archivo `composer.json`.

   ```bash
   composer require "magento/magento-cloud-metapackage":">=2.4.7 <2.4.8" --no-update
   ```

1. Actualice el metapaquete.

   ```bash
   composer update magento/magento-cloud-metapackage
   ```

1. Modifique los comandos de enlace en el archivo `magento.app.yaml`.

   ```yaml
   hooks:
       # We run build hooks before your application has been packaged.
       build: |
           set -e
           php ./vendor/bin/ece-tools run scenario/build/generate.xml
           php ./vendor/bin/ece-tools run scenario/build/transfer.xml
       # We run deploy hook after your application has been deployed and started.
       deploy: |
           php ./vendor/bin/ece-tools run scenario/deploy.xml
       # We run post deploy hook to clean and warm the cache. Available with ECE-Tools 2002.0.10.
       post_deploy: |
           php ./vendor/bin/ece-tools run scenario/post-deploy.xml
   ```

1. Busque y elimine [paquetes obsoletos](#remove-deprecated-packages). Los paquetes obsoletos pueden impedir que la actualización se realice correctamente.

   ```bash
   composer remove magento/magento-cloud-configuration
   ```

   ```bash
   composer remove magento/ece-patches
   ```

1. Puede que sea necesario actualizar el paquete `ece-tools`.

   ```bash
   composer update magento/ece-tools
   ```

1. Añada y confirme los cambios de código. En este ejemplo, se actualizaron los siguientes archivos:

   ```
   .magento.app.yaml
   composer.json
   composer.lock
   ```

1. Inserte los cambios de código en el servidor remoto y combine esta rama con la rama `integration`.

   ```bash
   git push origin <branch-name>
   ```
