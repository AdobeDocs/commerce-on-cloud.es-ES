---
title: Actualizar la versión de Commerce
description: Obtenga información sobre cómo actualizar la versión de Adobe Commerce en el proyecto de infraestructura en la nube.
feature: Cloud, Upgrade
exl-id: 0cc070cf-ab25-4269-b18c-b2680b895c17
source-git-commit: 1cea1cdebf3aba2a1b43f305a61ca6b55e3b9d08
workflow-type: tm+mt
source-wordcount: '1547'
ht-degree: 0%

---

# Actualizar la versión de Commerce

Puede actualizar el código base de Adobe Commerce a una versión más reciente. Antes de actualizar el proyecto, revise los [requisitos del sistema](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) en la guía de _Instalación_ para conocer los requisitos de la versión de software más reciente.

Según la configuración del proyecto, las tareas de actualización pueden incluir las siguientes:

- Actualice los servicios, como MariaDB (MySQL), OpenSearch, RabbitMQ y Redis, para comprobar la compatibilidad con las nuevas versiones de Adobe Commerce.
- Convertir un archivo de administración de configuración anterior.
- Actualice el archivo `.magento.app.yaml` con la nueva configuración para los vínculos y las variables de entorno.
- Actualice las extensiones de terceros a la última versión compatible.
- Actualizar el archivo `.gitignore`.

{{upgrade-tip}}

{{pro-update-service}}

## Actualizar desde versiones anteriores

Si está iniciando una actualización desde una versión de Commerce anterior a la 2.1, algunas restricciones en la base de código de Adobe Commerce pueden afectar su capacidad de _actualizar_ a una versión específica de ECE-Tools o de _actualizar_ a la siguiente versión de Commerce compatible. Utilice la siguiente tabla para determinar la mejor ruta:

| Versión actual | Ruta de actualización |
| ----------------- | ------------ |
| 2.1.3 y anteriores | Actualice Adobe Commerce a la versión 2.1.4 o posterior antes de continuar. A continuación, realice una [actualización única para instalar ECE-Tools](../dev-tools/install-package.md). |
| 2.1.4 - 2.1.14 | [Actualizar paquete ECE-Tools](../dev-tools/update-package.md).<br>Consulte las notas de la versión para [2002.0.9](../release-notes/cloud-release-archive.md#v200209) y versiones posteriores de 2002.0.x. |
| 2.1.15 - 2.1.16 | [Actualizar paquete ECE-Tools](../dev-tools/update-package.md).<br>Vea las notas de la versión para[2002.0.9](../release-notes/cloud-release-archive.md#v200209) y posteriores. |
| 2.2.x y posterior | [Actualizar paquete ECE-Tools](../dev-tools/update-package.md).<br>Vea las notas de la versión para[2002.0.8](../release-notes/cloud-release-archive.md#v200208) y posteriores. |

{style="table-layout:auto"}

{{ece-tools-package}}

## Administración de configuración

Las versiones anteriores de Adobe Commerce, como 2.1.4 o posterior a 2.2.x o posterior, utilizaban un archivo `config.local.php` para la administración de la configuración. La versión 2.2.0 y posteriores de Adobe Commerce usan el archivo `config.php`, que funciona exactamente igual que el archivo `config.local.php`, pero tiene diferentes opciones de configuración que incluyen una lista de los módulos habilitados y opciones de configuración adicionales.

Al actualizar desde una versión anterior, debe migrar el archivo `config.local.php` para utilizar el archivo `config.php` más reciente. Siga estos pasos para realizar una copia de seguridad del archivo de configuración y crear uno.

**Para crear un archivo `config.php` temporal**:

1. Cree una copia de `config.local.php` archivo y asígnele el nombre `config.php`.

1. Agregue este archivo a la carpeta `app/etc` de su proyecto.

1. Añada y confirme el archivo en su rama.

1. Inserte el archivo en la rama de integración.

1. Continúe con el proceso de actualización.

>[!WARNING]
>
>Después de la actualización, puede quitar el archivo `config.php` y generar un nuevo archivo completo. Solo puede eliminar este archivo para reemplazarlo esta vez. Después de generar un nuevo archivo completo de `config.php`, no puede eliminar el archivo para generar uno nuevo. Consulte [Administración de configuración e implementación de canalización](../store/store-settings.md).

### Verificar las dependencias del compositor de Zend Framework

Al actualizar a **2.3.x o posterior desde 2.2.x**, compruebe que las dependencias de Zend Framework se hayan agregado a la propiedad `autoload` del archivo `composer.json` para admitir Laminas. Este complemento admite nuevos requisitos para el marco Zend, que ha migrado al proyecto Laminas. Consulte [Migración de Zend Framework al proyecto Laminas](https://community.magento.com/t5/Magento-DevBlog/Migration-of-Zend-Framework-to-the-Laminas-Project/ba-p/443251) en _Magento DevBlog_.

**Para comprobar la configuración de `auto-load:psr-4`**:

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. Consulte la rama de integración.

1. Abra el archivo `composer.json` en un editor de texto.

1. Consulte la sección `autoload:psr-4` para ver la implementación de Zend plugin manager para la dependencia de los controladores.

   ```json
    "autoload": {
       "psr-4": {
          "Magento\\Framework\\": "lib/internal/Magento/Framework/",
          "Magento\\Setup\\": "setup/src/Magento/Setup/",
          "Magento\\": "app/code/Magento/",
          "Zend\\Mvc\\Controller\\": "setup/src/Zend/Mvc/Controller/"
       },
   }
   ```

1. Si falta la dependencia Zend, actualice el archivo `composer.json`:

   - Agregue la línea siguiente a la sección `autoload:psr-4`.

     ```json
     "Zend\\Mvc\\Controller\\": "setup/src/Zend/Mvc/Controller/"
     ```

   - Actualice las dependencias del proyecto.

     ```bash
     composer update
     ```

   - Agregar, confirmar y enviar cambios de código.

     ```bash
     git add -A
     ```

     ```bash
     git commit -m "Add Zend plugin manager implementation for controllers dependency for Laminas support"
     ```

     ```bash
     git push origin <branch-name>
     ```

   - Combine los cambios en el entorno de ensayo y, a continuación, en Producción.

## Archivos de configuración

Antes de actualizar la aplicación, debe actualizar los archivos de configuración del proyecto para tener en cuenta los cambios en los valores de configuración predeterminados de Adobe Commerce en la infraestructura en la nube o en la aplicación. Los valores predeterminados más recientes se encuentran en el [repositorio de GitHub de Magento en la nube](https://github.com/magento/magento-cloud).

### .magento.app.yaml

Revise siempre los valores contenidos en el archivo [.magento.app.yaml](../application/configure-app-yaml.md) para la versión instalada, ya que controla la forma en que la aplicación se genera e implementa en la infraestructura de la nube. El siguiente ejemplo es para la versión 2.4.8 y utiliza Composer 2.8.4. La propiedad `build: flavor:` no se usa para Composer 2.x; consulte [Instalación y uso de Composer 2](../application/properties.md#installing-and-using-composer-2).

**Para actualizar el archivo `.magento.app.yaml`**:

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. Abra y edite el archivo `magento.app.yaml`.

1. Actualice las opciones de PHP.

   ```yaml
   type: php:8.4
   
   build:
       flavor: none
   dependencies:
       php:
           composer/composer: '2.8.4'
   ```

1. Modifique los comandos `build` y `deploy` de la propiedad `hooks`.

   ```yaml
   hooks:
       # We run build hooks before your application has been packaged.
       build: |
           set -e
           composer install
           php ./vendor/bin/ece-tools run scenario/build/generate.xml
           php ./vendor/bin/ece-tools run scenario/build/transfer.xml
       # We run deploy hook after your application has been deployed and started.
       deploy: |
           php ./vendor/bin/ece-tools run scenario/deploy.xml
       # We run post deploy hook to clean and warm the cache. Available with ECE-Tools 2002.0.10.
       post_deploy: |
           php ./vendor/bin/ece-tools run scenario/post-deploy.xml
   ```

1. Agregue las siguientes variables de entorno al final del archivo.

   Para Adobe Commerce 2.2.x a 2.3.x-

   ```yaml
   variables:
       env:
           CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
           CONFIG__STORES__DEFAULT__PAYMENT__BRAINTREE__CHANNEL: 'Magento_Enterprise_Cloud_BT'
           CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
   ```

   Para Adobe Commerce 2.4.x-

   ```yaml
   variables:
       env:
           CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
           CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
   ```

1. Guarde el archivo. No confirme ni inserte cambios en el entorno remoto aún.

1. Continúe con el proceso de actualización.

### composer.json

Antes de actualizar, compruebe siempre que las dependencias del archivo `composer.json` sean compatibles con la versión de Adobe Commerce.

**Para actualizar el archivo `composer.json` para la versión 2.4.4 y posterior de Adobe Commerce**:

1. Agregar los(as) siguientes `allow-plugins` a la sección `config`:

   ```json
   "config": {
      "allow-plugins": {
         "dealerdirect/phpcodesniffer-composer-installer": true,
         "laminas/laminas-dependency-plugin": true,
         "magento/*": true
      }
   },
   ```

1. Agregue el siguiente complemento a la sección `require`:

   ```json
   "require": {
       "magento/composer-root-update-plugin": "^2.0.3"
   },
   ```

1. Agregue el siguiente componente a la sección `extra:component_paths`:

   ```json
   "extra": {
      "component_paths": {
         "tinymce/tinymce": "lib/web/tiny_mce_5"
      },
   },
   ```

1. Guarde el archivo. Aún no confirme ni inserte cambios en la rama.

1. Continúe con el proceso de actualización.

## Copia de seguridad del proyecto

Se recomienda crear una copia de seguridad del proyecto antes de una actualización. Siga estos pasos para realizar una copia de seguridad de los entornos de integración, ensayo y producción.

**Para hacer una copia de seguridad de la base de datos y el código del entorno de integración**:

1. Cree una copia de seguridad local de la base de datos remota.

   ```bash
   magento-cloud db:dump
   ```

   >[!NOTE]
   >
   >El comando `magento-cloud db:dump` ejecuta el comando [mysqldump](https://dev.mysql.com/doc/refman/8.0/en/mysqldump.html) con el indicador `--single-transaction`, lo que le permite hacer una copia de seguridad de la base de datos sin bloquear las tablas.

1. Haga una copia de seguridad del código y los medios.

   ```bash
   php bin/magento setup:backup --code [--media]
   ```

   Opcionalmente, puede omitir `[--media]` si tiene un gran número de archivos estáticos que ya están en el control de código fuente.

**Para hacer una copia de seguridad de la base de datos del entorno de ensayo o producción antes de implementar**:

1. Utilice SSH para iniciar sesión en el entorno remoto.

1. Crear un [volcado de base de datos](../storage/database-dump.md). Para elegir un directorio de destino para el volcado de la base de datos, utilice la opción `--dump-directory`.

   ```bash
   vendor/bin/ece-tools db-dump
   ```

   La operación de volcado crea un archivo de `dump-<timestamp>.sql.gz` en el directorio del proyecto remoto. Consulte [Copia de seguridad de la base de datos](../storage/database-dump.md).

## Actualización de aplicación

Revise la información de [versiones de servicio](../services/services-yaml.md#service-versions) para conocer los requisitos de la versión de software más reciente antes de actualizar la aplicación.

**Para actualizar la versión de la aplicación**:

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. Establezca la versión de actualización con la [sintaxis de restricción de versión](overview.md#cloud-metapackage).

   ```bash
   composer require "magento/magento-cloud-metapackage":">=CURRENT_VERSION <NEXT_VERSION" --no-update
   ```

   >[!NOTE]
   >
   >Debe usar la sintaxis de restricción de versión para actualizar correctamente el paquete `ece-tools`. Puede encontrar la restricción de versión en el archivo `composer.json` para la versión de la [plantilla de aplicación](https://github.com/magento/magento-cloud/blob/master/composer.json) que está utilizando para la actualización.

1. Actualice el proyecto.

   ```bash
   composer update
   ```

1. Revise los parches que se aplican actualmente:

   - Si hay parches instalados en el directorio `m2-hotfixes`, [envíe un ticket de soporte de Adobe Commerce](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#support-case) y trabaje con el soporte técnico de Adobe Commerce para comprobar qué parches se pueden seguir aplicando a la nueva versión. Quite los parches no aplicables del directorio `m2-hotfixes`.

   - Si hay [Parches de calidad] aplicados en el archivo `.magento.env.yaml`, compruebe si aún se pueden aplicar a la nueva versión. Quite los parches no aplicables de la sección `QUALITY_PATCHES` del archivo `.magento.env.yaml`.

   **Método 1**: [Compruebe las versiones aplicables en las notas de la versión de parches de calidad](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/quality-patches-tool/release-notes)

   **Método 2**: [Ver parches y estado disponibles](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/upgrade/apply-patches#view-available-patches-and-status)

   **Método 3**: [Buscar parches](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html?lang=en)


1. Agregar, confirmar y enviar cambios de código.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Upgrade"
   ```

   ```bash
   git push origin <branch-name>
   ```

   `git add -A` es necesario para agregar todos los archivos modificados al control de código fuente debido a la forma en que Composer calcula los paquetes base. Tanto `composer install` como `composer update` calculan las referencias de los archivos del paquete base (`magento/magento2-base` y `magento/magento2-ee-base`) en la raíz del paquete.

   Los archivos a los que Composer calcula las referencias pertenecen a la nueva versión de Adobe Commerce, para sobrescribir la versión obsoleta de esos mismos archivos. En la actualidad, el cálculo de referencias está deshabilitado en Adobe Commerce, por lo que debe agregar los archivos para calcular referencias al control de código fuente.

1. Espere a que se complete la implementación.

1. Compruebe la actualización en el entorno de integración, ensayo o producción utilizando SSH para iniciar sesión y comprobar la versión.

   ```bash
   php bin/magento --version
   ```

### Crear un archivo config.php

Como se menciona en [Administración de configuración](#configuration-management), después de actualizar, debe crear un archivo `config.php` actualizado. Complete los cambios de configuración adicionales mediante el Administrador en su entorno de integración.

**Para crear un archivo de configuración específico del sistema**:

1. Desde el terminal, utilice un comando SSH para generar el archivo `/app/etc/config.php` para el entorno.

   ```bash
   ssh <SSH-URL> "<Command>"
   ```

   Por ejemplo, para Pro, para ejecutar `scd-dump` en la rama `integration`:

   ```bash
   ssh <project-id-integration>@ssh.us.magentosite.cloud "php vendor/bin/ece-tools config:dump"
   ```

1. Transfiera el archivo `config.php` a las estaciones de trabajo locales mediante `rsync` o `scp`. Solo puede agregar este archivo a la rama localmente.

   ```bash
   rsync <SSH-URL>:app/etc/config.php ./app/etc/config.php
   ```

1. Agregar, confirmar y enviar cambios de código.

   ```bash
   git add app/etc/config.php && git commit -m "Add system-specific configuration" && git push origin master
   ```

   Esto genera un archivo `/app/etc/config.php` actualizado con una lista de módulos y opciones de configuración.

>[!WARNING]
>
>Para una actualización, se elimina el archivo `config.php`. Una vez agregado este archivo a su código, **no** debe eliminarlo. Si debe quitar o editar la configuración, edite el archivo manualmente.

### Actualización de extensiones

Revise las páginas de módulos y extensiones de terceros en Marketplace u otros sitios de la empresa y compruebe la compatibilidad con Adobe Commerce y Adobe Commerce en la infraestructura en la nube. Si debe actualizar cualquier extensión y módulo de terceros, Adobe recomienda trabajar en una nueva rama de integración con las extensiones deshabilitadas.

**Para verificar y actualizar sus extensiones**:

1. Cree una rama en la estación de trabajo local.

1. Deshabilite las extensiones según sea necesario.

1. Cuando esté disponible, descargue las actualizaciones de extensión.

1. Instale la actualización según lo documentado por la documentación de terceros.

1. Habilite y pruebe la extensión de.

1. Agregue, confirme e inserte los cambios de código en el control remoto.

1. Envíe a y pruebe en su entorno de integración.

1. Insertar en el entorno de ensayo para realizar pruebas en un entorno de preproducción.

Adobe recomienda encarecidamente actualizar el entorno de producción _antes de_, incluidas las extensiones actualizadas en el proceso de lanzamiento del sitio.

>[!NOTE]
>
>Al actualizar la versión de la aplicación, el proceso de actualización se actualiza automáticamente a la última versión del [módulo CDN de Fastly](../cdn/fastly.md#fastly-cdn-module-for-magento-2).

## Solución de problemas de actualización

Si la actualización falla, recibe un mensaje de error en el explorador que indica que no puede acceder a la tienda o al panel Administración:

```
There has been an error processing your request
Exception printing is disabled by default for security reasons.
  Error log record number: <error-number>
```

**Para resolver el error**:

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. Utilice SSH para iniciar sesión en el entorno remoto.

   ```bash
   magento-cloud ssh
   ```

1. Abra el archivo `./app/var/report/<error number>`.

1. [Examine los registros](../test/log-locations.md) y determine el origen del problema.

1. Agregar, confirmar y enviar cambios de código.

   ```bash
   git add -A && git commit -m "Fixed deployment failure" && git push origin <branch-name>
   ```
