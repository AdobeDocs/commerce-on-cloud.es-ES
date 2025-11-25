---
title: Cambios incompatibles con versiones anteriores
description: Obtenga información acerca de la compatibilidad con versiones anteriores al actualizar proyectos existentes en la nube.
feature: Cloud, Release Notes
recommendations: noDisplay, catalog
exl-id: 3f3c1036-bfd0-4c70-8309-6c5e442134cd
source-git-commit: de50fda78c28a57d76e5c0a4d5dac0f8d4d844a0
workflow-type: tm+mt
source-wordcount: '791'
ht-degree: 0%

---

# Cambios incompatibles con versiones anteriores

Es posible que los cambios incompatibles con versiones anteriores requieran que ajuste la configuración y los procesos de Cloud para los proyectos de Cloud existentes al actualizar a la última versión del paquete `ece-tools` u otros paquetes de Cloud Tools Suite para Commerce.

## Cambios en el paquete `ece-tools`

Alguna funcionalidad previamente incluida en el paquete `ece-tools` ahora se proporciona en paquetes separados. Estos paquetes son dependencias del compositor para `ece-tools`, que se instalan y actualizan automáticamente al instalar o actualizar ece-tools.

La nueva arquitectura no debería afectar a los procesos de instalación o actualización. Sin embargo, es posible que tenga que cambiar algunos procesos y sintaxis de comandos al trabajar con Adobe Commerce en el proyecto de infraestructura en la nube. Para obtener más información, revise la siguiente información sobre cambios incompatibles con versiones anteriores y las [notas de la versión de Cloud Tools Suite](cloud-tools-suite.md).

### Cambios en los requisitos de versión del servicio

Cambiamos el requisito de versión mínima de PHP de 7.0.x a 7.1.x para proyectos en la nube que usan `ece-tools` v2002.1.0 y posteriores. Si la configuración de su entorno especifica PHP 7.0, actualice la [configuración php](../application/php-settings.md) en el archivo `.magento.app.yaml`.

>[!WARNING]
>
>Debido al cambio en los requisitos de la versión de PHP, `ece-tools` 2002.1.0 solo admite Adobe Commerce en proyectos de infraestructura en la nube que ejecuten Adobe Commerce 2.1.15 o posterior. Si su proyecto usa una versión anterior, debe [actualizar](../development/commerce-version.md) antes de actualizar a `ece-tools` 2002.1.0.

### Cambios de configuración del entorno

La siguiente tabla proporciona información sobre las variables de entorno y otros archivos de configuración de entorno que se eliminaron o quedaron obsoletos en `ece-tools` v2002.1.0.

| Elemento | Sustitución |
| -------- | ----------- |
| `SCD_EXCLUDE_THEMES` variable | [`SCD_MATRIX`](../environment/variables-build.md#scd_matrix) |
| `STATIC_CONTENT_THREADS` variable | [`SCD_THREADS`](../environment/variables-build.md#scd_threads) |
| `DO_DEPLOY_STATIC_CONTENT` variable | [`SKIP_SCD`](../environment/variables-build.md#skip_scd) |
| `STATIC_CONTENT_SYMLINK` variable | Ninguna. Ahora, la compilación siempre crea un enlace simbólico al directorio de contenido estático `pub/static`. |
| `build_options.ini` archivo | Utilice el archivo [`.magento.env.yaml`](../application/configure-app-yaml.md) para configurar variables de entorno con el fin de administrar acciones de compilación e implementación en todos los entornos.<p>Si genera un entorno de nube que incluya el archivo `build_options.ini`, la compilación falla. |

### Cambios en el comando CLI

La siguiente tabla resume los cambios de comandos CLI en ECE-Tools v2002.1.0 que pueden requerir la actualización de comandos o secuencias de comandos.

| Comando | Sustitución |
|-------- | ----------- |
| `m2-ece-build` | `vendor/bin/ece-tools build` |
| `m2-ece-deploy` | `vendor/bin/ece-tools deploy` |
| `m2-ece-scd-dump` | `vendor/bin/ece-tools config:dump` |
| `vendor/bin/ece-tools patch` | `vendor/bin/ece-patches apply` |
| `vendor/bin/ece-tools docker:build` | `vendor/bin/ece-docker build:compose` |
| `vendor/bin/ece-tools docker:config:convert` | `vendor/bin/ece-docker  image:generate:php` |

En versiones anteriores de ECE-Tools, se podían utilizar los comandos `m2-ece-build` y `m2-ece-deploy` para configurar los vínculos de implementación en el archivo `.magento.app.yaml`. Cuando actualice a v2002.1.0, compruebe la configuración de `hooks` en el archivo `.magento.app.yaml` para ver si hay comandos obsoletos y reemplácelos si es necesario.

## Cambios en Parches de nube

- **Quitar parches descargados**-El paquete `magento/magento-cloud-patches` agrupa todos los parches disponibles en la página [descargas de software](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/commerce.html) y los aplica automáticamente al implementarlo en la nube. Para evitar conflictos de parches después de actualizar a ECE-Tools 2002.1.0 o posterior, elimine los parches suministrados por Adobe que haya descargado y agregado al proyecto manualmente.

- **Actualizando el comando para aplicar parches**- Hemos movido el comando para aplicar parches del directorio `vendor/bin/ece-tools` al directorio `vendor/bin/ece-patches`. Si utiliza este comando para aplicar parches manualmente, utilice la nueva ruta.

  > Aplicar parches manualmente

  ```bash
  php ./vendor/bin/ece-patches apply
  ```

## Cambios de Cloud Docker

- **El requisito mínimo de la versión de PHP ahora es PHP 7.1**. Si el host Cloud Docker para Commerce está ejecutando una versión anterior, actualice a PHP v7.1 o posterior.

- **Cambios en el comando de Cloud Docker para Commerce**-

   - **Actualización de los comandos de Cloud Docker para Commerce para las operaciones de generación de Docker**- Hemos movido los comandos de Cloud Docker para Commerce del directorio `vendor/bin/ece-tools` al directorio `vendor/bin/ece-docker`. Actualice los scripts y comandos para utilizar la nueva ruta.

     Después de actualizar a `ece-tools` 2002.1.0, use el comando siguiente para ver los comandos de `ece-docker` disponibles.

     ```bash
     php ./vendor/bin/ece-docker list
     ```

   - **Actualizando los comandos de Cloud Docker-Compose**- Cambiamos el nombre de la ruta de acceso al archivo de comandos de `./bin/docker` a `./bin/magento-docker`. Actualice los scripts y comandos para utilizar la nueva ruta.

   - **El contenedor Cron ya no se incluye en la configuración Docker predeterminada**-Ahora debe agregar la opción `--with-cron` al comando `ece-docker build:compose` para incluir el contenedor Cron en la configuración del entorno Docker. Consulte [Administrar trabajos cron](https://developer.adobe.com/commerce/cloud-tools/docker/configure/manage-cron-jobs) en la guía de _Cloud Docker para Commerce_.

     Los scripts que anteriormente generaban contenedores con trabajos cron ahora no tienen el contenedor cron.

   - **Al usar contenedores temporales**-En versiones anteriores, los contenedores creados por `bin/magento-docker` operaciones de comando no se quitaban, por lo que se podían usar para otras operaciones. Ahora, los comandos `magento-docker` quitan los contenedores que hayan creado una vez finalizado el comando.

     Si desea conservar un contenedor creado por una operación docker-compose, utilice el comando `docker-compose run` en lugar del comando `bin/magento-docker`.

   - **Ejecutar vínculos posteriores a la implementación**: El comando `cloud-deploy` ya no ejecuta los vínculos posteriores a la implementación. Utilice el nuevo comando `cloud-post-deploy` para ejecutar los vínculos posteriores a la implementación después de la implementación. Actualice los scripts para agregar el comando y ejecutar los vínculos posteriores a la implementación.

     ```shell
     bin/magento-docker ece-deploy
     bin/magento-docker ece-post-deploy
     ```

     Como alternativa, si utiliza los comandos de `docker-compose` directamente, ejecute el comando `docker-compose run deploy cloud-post-deploy` después del comando de implementación.

- **Actualizando la base de datos**: el contenedor de la base de datos ahora está almacenado en el volumen Docker persistente `magento-db`. Al actualizar el entorno de Docker, la base de datos ya no se elimina automáticamente. Si es necesario, utilice uno de los siguientes comandos para eliminarlo manualmente.

   - Quitar el contenedor `magento-db`:

     ```bash
     docker volume rm magento-db
     ```

   - Elimine todos los volúmenes asociados al cerrar los contenedores de Docker:

     ```bash
     docker-compose down -v
     ```

- **Anular la configuración de sincronización de archivos para archivos de archivado y copia de seguridad**: Los archivos de archivado y copia de seguridad con las siguientes extensiones ya no se sincronizan al usar docker-sync o mutagen: SQL, GZ, ZIP y BZ2. Puede anular la sincronización de archivos predeterminada para estos tipos de archivos cambiando el nombre del archivo para que termine con una extensión diferente. Por ejemplo: `synchronize-me.zip-backup`
