---
title: Paquete Cloud Docker
description: Consulte la lista de las mejoras más recientes del paquete Cloud Docker.
feature: Cloud, Docker, Release Notes
recommendations: noDisplay, catalog
last-substantial-update: 2025-04-03T00:00:00Z
exl-id: 95cf4f30-6bce-4bac-8e11-cfe53cac2c70
source-git-commit: 3d5c84890f48a26938b42783b591b876fd2a2fd1
workflow-type: tm+mt
source-wordcount: '3710'
ht-degree: 0%

---

# Paquete Cloud Docker

El paquete [`magento/magento-cloud-docker`](https://github.com/magento/magento-cloud-docker) proporciona funcionalidad e imágenes de Docker para implementar Adobe Commerce en un entorno de nube local. En estas notas de la versión se describen las mejoras más recientes realizadas en este paquete, que es un componente de [Cloud Tools Suite para Commerce](cloud-tools-suite.md).

El paquete `magento/magento-cloud-docker` usa la siguiente secuencia de versiones: `<major>.<minor>.<patch>`

Las notas de la versión incluyen:

- ![nuevo icono](../../assets/new.svg) Nuevas características
- ![icono de corrección](../../assets/fix.svg) Correcciones y mejoras

<!--Add release notes below-->

## Versión 1.4.2 {#latest}

Fecha de publicación: 3 de abril de 2025

- ![nuevo icono](../../assets/new.svg) **PHP 8.4**—Se agregaron `php-cli` 8.4 y `php-fpm` imágenes 8.4.


## Versión 1.4.1

Fecha de publicación: 6 de febrero de 2025

- ![nuevo icono](../../assets/new.svg) **PHP 8.4**—Se agregó compatibilidad con PHP 8.4.


## Versión 1.4.0

Fecha de la versión: 7 de octubre de 2024

- ![Icono de corrección](../../assets/fix.svg) **Código refactorizado**—Se ha eliminado la compatibilidad con versiones antiguas de PHP (7.4, 7.3, 7.2) y con bibliotecas e imágenes relacionadas.

## Versión 1.3.7

Fecha de publicación: 8 de abril de 2024

- ![nuevo icono](../../assets/new.svg) **PHP** — Se agregó compatibilidad con imágenes de PHP 8.3 y PHP 8.3.
- ![nuevo icono](../../assets/new.svg) **Nginx** — Imagen añadida nginx v. 1.24.
- ![nuevo icono](../../assets/new.svg) **Opensearch** - Imagen agregada en OpenSearch v. 2.12, 1.3.
- ![nuevo icono](../../assets/new.svg) **Compositor** - Versión del compositor actualizada a 2.2.23.

## Versión 1.3.6

Fecha de la versión: 31 de julio de 2023

- ![nuevo icono](../../assets/new.svg) **Se ha agregado una nueva versión del servicio**—OpenSearch 2.5.
- ![nuevo icono](../../assets/new.svg) **Habilitar la caché del Compositor**: ahora puede ampliar la configuración de Docker para habilitar la caché de borrado del Compositor al iniciar el contenedor de Docker. Consulte [Ampliar la configuración de Docker](https://developer.adobe.com/commerce/cloud-tools/docker/configure/extend-docker-configuration/) en la guía de _Cloud Docker para Commerce_.

## Versión 1.3.5

Fecha de la versión: 10 de marzo de 2023

- ![nuevo icono](../../assets/new.svg) **ionCube**—Se ha añadido la extensión ionCube para la imagen de PHP 8.1.
- ![nuevo icono](../../assets/new.svg) **Se agregaron nuevas versiones del servicio**—OpenSearch 2.3 y 2.4, PHP 8.2, Varnish 7.1.1.
- ![nuevo icono](../../assets/new.svg) **Soporte mejorado para PHP 8.2**—Se corrigieron problemas de compatibilidad con ciertas versiones de PHP 8.2.x para admitir Commerce 2.4.6.
- ![Icono de correcciones](../../assets/fix.svg) **Problema del compositor**: se han corregido problemas que se producían después de actualizar la versión del compositor en los contenedores de Docker.

## Versión 1.3.4

Fecha de la versión: 27 de octubre de 2022

- ![nuevo icono](../../assets/new.svg) **Se agregaron nuevas imágenes de barniz**—Se agregaron imágenes para Varnish 6.5, 7.0 y 7.1.<!-- MCLOUD-7879 -->

## Versión 1.3.3

Fecha de la versión: 13 de septiembre de 2022

- ![nuevo icono](../../assets/new.svg) **compatibilidad con Apple M1 (ARM64)**—Se han agregado cambios a las imágenes Docker para habilitar la compatibilidad con la arquitectura Apple M1 (ARM64).<!-- MCLOUD-7989-2 MCLOUD-7989 -->
- ![Icono de corrección](../../assets/fix.svg) **Mailhog**: se ha corregido un problema por el que el servicio Mailhog no detectaba correos electrónicos en el modo de desarrollador.<!-- MCLOUD-8643 -->
- ![icono de corrección](../../assets/fix.svg) **init-docker.sh**: se ha corregido el validador de versiones de servicio en el script `init-docker.sh`.<!-- MCLOUD-8765 -->

## Versión 1.3.2

Fecha de la versión: 31 de marzo de 2022

- ![nuevo icono](../../assets/new.svg) **Se agregó la imagen de Elasticsearch 7.10**<!-- MCLOUD-8548 -->

## Versión 1.3.1

Fecha de la versión: 10 de marzo de 2022

- ![nuevo icono](../../assets/new.svg) **Soporte PHP 8.1**—Se agregó soporte para PHP 8.1.
- ![nuevo icono](../../assets/new.svg) **OpenSearch**—Se han agregado imágenes de las versiones 1.1 y 1.2 de OpenSearch.
- ![nuevo icono](../../assets/new.svg) **Compositor 2.1**—Establece compositor 2.1.x de forma predeterminada en imágenes PHP 8.x.
- ![nuevo icono](../../assets/new.svg) **mejoras en las imágenes de PHP**—

   - Se agregaron imágenes de PHP 8.1
   - Se ha actualizado xDebug versión 3.1.2
   - Se ha actualizado xmlrpc 1.0.0RC3

- ![Icono de correcciones](../../assets/fix.svg) **Mejoras de Elasticsearch y OpenSearch**: mejoras en los archivos Docker de Elasticsearch y OpenSearch; se eliminó la imagen de Elasticsearch 5.2.
- ![Icono de corrección](../../assets/fix.svg) **Extensión de sodio**—Habilitó la extensión `sodium` de forma predeterminada en todas las imágenes PHP.
- ![Icono de correcciones](../../assets/fix.svg) **Volumen de caché del Compositor**: ruta fija para que el volumen de caché del Compositor tenga paquetes del Compositor en caché.
- ![icono de corrección](../../assets/fix.svg) **Limitación de memoria en nginx**—Se ha corregido la limitación de memoria en la imagen NGINX.

## Versión 1.3.0

Fecha de la versión: 25 de octubre de 2021

- ![Icono de corrección](../../assets/fix.svg) **Mejora el flujo de trabajo en modo de desarrollador**—Anteriormente, necesitaba especificar el modo en los pasos de compilación e implementación. Ahora, la opción `--mode` en el paso `build` determina el modo en el paso posterior `deploy`. Ya no es necesario configurar el modo después de la implementación. Ver [modo de desarrollador](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/developer-mode/).<!-- ACMP-1086 -->
- ![icono de corrección](../../assets/fix.svg) **Mejoras para el sistema de archivos de solo lectura**—<!-- ACMP-1106 -->
   - Se ha corregido un problema que iniciaba un contenedor de PHP para la configuración de correo.
   - Puede utilizar variables de entorno en archivos INI.
   - Asegúrese de que los puntos de entrada de PHP no necesitan permiso de escritura.
- ![Icono de corrección](../../assets/fix.svg) **Actualizar nodo**: actualice la versión del nodo agrupado; al instalar el nodo en imágenes PHP-CLI, ahora utiliza la versión actual de LTS.<!-- ACMP-1539 -->
- ![icono de corrección](../../assets/fix.svg) **Actualizar Symfony**: se han actualizado las dependencias de configuración de Symfony para que sean compatibles con Adobe Commerce 2.4.4.<!-- ACMP-1533 -->

## Versión 1.2.4

Fecha de la versión: 29 de julio de 2021

- ![nuevo icono](../../assets/new.svg) **Nuevo contenedor `Zookeeper`**—Se agregó un [contenedor de Zookeeper](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#zookeeper-container) para administrar la configuración del proveedor de bloqueos para los proyectos que no se implementan en Adobe Commerce en la infraestructura de la nube.<!--MCLOUD-8000-->

- ![nuevo icono](../../assets/new.svg) **Se ha agregado compatibilidad con Composer 2.0.**—Se ha agregado la versión 2.0 del Compositor al archivo de configuración del Compositor para admitir las actualizaciones desde Composer 1.0, que está llegando al final de su vida útil.<!--MCLOUD-8003-->

## Versión 1.2.3

Fecha de publicación: 14 de junio de 2021

- ![nuevo icono](../../assets/new.svg) **Se agregó PHP 8.0**—Se actualizó PHP a la versión 8.0, lo que le permite aprovechar todas las nuevas características y optimizaciones que incluye PHP 8.0.<!--MCLOUD-7941-->
- ![nuevo icono](../../assets/new.svg) **Actualizado a Varnish 6.6 y Elasticsearch 7.11.2**—Los siguientes vínculos proporcionan información de la versión sobre [Varnish Cache 6.6](https://varnish-cache.org/releases/rel6.6.0.html#rel6-6-0) y Elasticsearch 7.11.2.<!--MCLOUD-7921-->
- ![nuevo icono](../../assets/new.svg) **Se agregó la extensión `ioncube` para la imagen de PHP 7.4**—La extensión `ioncube` se ha vuelto a agregar a la imagen de PHP 7.4 después de haber sido inicialmente excluida de la actualización de PHP 7.3 a PHP 7.4. *[Enviado por mattskr](https://github.com/magento/magento-cloud-docker/pull/314).*<!--PR #314-->
- ![nuevo icono](../../assets/new.svg) **Se agregó una opción de sincronización de archivos:`manual-native`**—La opción de sincronización de archivos `manual-native` proporciona control manual sobre la sincronización, que proporciona el mejor rendimiento para los entornos de macOS y Windows. Obtenga información sobre el uso de la opción `manual-native` en [modo de desarrollador](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/developer-mode/) y [Sincronización de datos en un entorno de desarrollador de Docker](https://developer.adobe.com/commerce/cloud-tools/docker/setup/synchronize-data/#file-synchronization-options).<!--MCLOUD-7977-->
- ![nuevo icono](../../assets/new.svg) **Eliminó eliminaciones de volumen de `up` y `down` comandos**—La opción `--volume` se eliminó de los comandos `bin/magento-docker up` y `bin/magento-docker down`, reemplazada por el nuevo comando `bin/magento-docker init` con una advertencia de pérdida de datos. Este cambio ayuda a evitar la pérdida accidental de datos. *[Enviado por joeshelton-wagento](https://github.com/magento/magento-cloud-docker/pull/319).*<!--PR #319-->
- ![Icono de corrección](../../assets/fix.svg) **Se ha actualizado el valor `CN` del certificado generado**—Se ha eliminado el valor `CN` codificado del archivo Dockerfile. Este valor creó un error de certificado (`NET::ERR_CERT_INVALID`) que hizo que se ignorara la opción `--host` para el comando `ece-docker build:compose`.<!--MCLOUD-7934-->

## Versión 1.2.2

Fecha de publicación: 20 de abril de 2021

- ![nuevo icono](../../assets/new.svg) **Se ha actualizado `host.docker.internal` para que sea independiente de la plataforma**. Ahora puede crear los mismos scripts de composición de Docker para Ubuntu, Windows y macOS. El uso de Xdebug en Ubuntu ya no requiere una variable de entorno independiente. [Corrección enviada por Igor Vitol](https://github.com/magento/magento-cloud-docker/pull/299).<!--Issue #298-->
- ![nuevo icono](../../assets/new.svg) **Se ha actualizado init-docker.sh**—Se ha agregado el objeto `mounts` a la variable de entorno `MAGENTO_CLOUD_APPLICATION`. [Corrección enviada por Chiranjeevi](https://github.com/magento/magento-cloud-docker/pull/299).<!--Issue #299-->
- ![nuevo icono](../../assets/new.svg) **Se ha actualizado init-docker.sh**—Se ha actualizado el script `init-docker.sh` con las versiones PHP 7.4 y Cloud Docker 1.2.1. [Corrección enviada por Adarsh Manickam](https://github.com/magento/magento-cloud-docker/pull/300).<!--Issue #300-->
- ![nuevo icono](../../assets/new.svg) **Sodio habilitado de forma predeterminada**: se habilitó la extensión PHP `sodium` de forma predeterminada en las imágenes Docker de PHP.<!--MCLOUD-7548-->
- ![nuevo icono](../../assets/new.svg) **`custom-registry`opción**—Se ha agregado una opción `--custom-registry` al comando `php ./vendor/bin/ece-docker build:compose` para usar su propio registro de imágenes.<!--MCLOUD-7476-->

  ```bash
  ./vendor/bin/ece-docker build:compose --custom-registry=my-registry.example.com
  ```

- ![nuevo icono](../../assets/new.svg) **Se eliminaron las versiones antiguas de Elasticsearch**—Se eliminaron las versiones 1.7 y 2.4 de Elasticsearch de las imágenes de Elasticsearch.<!--MCLOUD-7504-->
- ![nuevo icono](../../assets/new.svg) **Certificados NGINX de generación automática**—Se eliminaron los certificados existentes de la imagen NGINX. Los certificados NGINX ahora se generan automáticamente con cada nueva implementación para mejorar la seguridad.<!--MCLOUD-7396-->
- ![icono de corrección](../../assets/fix.svg) **Habilitado`opcache.validate_timestamps`**: se habilitó la configuración de PHP `opcache.validate_timestamps` de forma predeterminada en el modo de desarrollador. Al habilitar esta configuración, se corrigió el problema en el cual los cambios en el sistema de archivos no se reconocían en Docker.<!--MCLOUD-7466-->
- ![Icono de corrección](../../assets/fix.svg) **Se ha corregido`build:custom:compose`**: se ha corregido el comando `build:custom:compose` para generar un error cuando los archivos no se pueden sobrescribir durante el proceso de generación. Si se produce un error se evitan situaciones en las que `docker-compose up` podría estar usando los archivos incorrectos.<!--MCLOUD-7457-->
- ![Icono de corrección](../../assets/fix.svg) **Se ha corregido la opción `--sync_engine="native"`**—Se ha corregido el problema por el que en el modo de producción (`--mode="production"`), la opción `--sync_engine="native"` no creaba ninguna entrada para las carpetas locales en el archivo `docker.composer.yml`.<!--MCLOUD-7254-->
- ![Icono de corrección](../../assets/fix.svg) **Se corrigieron errores de validación de la versión del servicio**: se agregaron versiones del servicio para [!DNL RabbitMQ], Elasticsearch y otros servicios a la propiedad `type` en la variable `MAGENTO_CLOUD_RELATIONSHIP`. Al agregar estas versiones a la variable `relationships` se corrigieron los errores de validación que se produjeron durante la fase de implementación.<!--MCLOUD-7572-->

## Versión 1.2.1

Fecha de la versión: 21 de diciembre de 2020

- ![nuevo icono](../../assets/new.svg) **opciones de comando NGINX**: se han agregado opciones de comando de compilación para cambiar el número de NGINX `worker_processes` y NGINX `worker_connections` para TLS y servicios web. El parámetro `worker_process` conserva la capacidad de establecer el valor en `auto`. Ejemplos: <!--MCLOUD-7259-->

  ```bash
  ./vendor/bin/ece-docker build:compose --nginx-worker-processes=2
  ./vendor/bin/ece-docker build:compose --nginx-worker-connections=2048
  ```

- ![nuevo icono](../../assets/new.svg) **opción de comando TLS**—Se ha agregado la opción de comando de compilación para crear una configuración sin el servicio TLS. Ejemplo: <!--MCLOUD-7259-->

  ```bash
  ./vendor/bin/ece-docker build:compose --no-tls
  ```

- ![nuevo icono](../../assets/new.svg) **consumo de memoria NGINX**: se ha reducido la memoria consumida por el proceso NGINX para TLS y servicios web.<!--MCLOUD-7259-->

- ![nuevo icono](../../assets/new.svg) **Blackfire**: la extensión Blackfire PHP está deshabilitada de forma predeterminada en la imagen Cloud Docker.

- ![Icono de correcciones](../../assets/fix.svg) **Contenedor de PHP-FPM**—Se corrigió la comprobación del estado del contenedor de PHP-FPM al cambiar `WEB_PORT` de `80` a `8080`.<!--MCLOUD-7232-->

- ![icono de corrección](../../assets/fix.svg) **Nomenclatura de volumen no válida**—Se ha corregido un error con una nomenclatura de volumen no válida en el modo de desarrollador.<!--MCLOUD-7442-->

- ![icono de corrección](../../assets/fix.svg) **Puerto NGINX de subida**: se ha actualizado la imagen Docker NGINX 1.19 para que utilice el puerto 8080 y así evitar un bucle infinito. [Corrección enviada por Adarsh Manickam](https://github.com/magento/magento-cloud-docker/pull/296).<!--Issue 295-->

## Versión 1.2.0

Fecha de la versión: 9 de noviembre de 2020

- ![nuevo icono](../../assets/new.svg) **Actualizaciones de contenedor—**

   - ![nuevo icono](../../assets/new.svg) **contenedor PHP-FPM**—Se agregó compatibilidad con la extensión gnupg de PHP. [Corrección enviada por G Arvind desde Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/210).<!--MCLOUD-5981-->

   - ![Icono de corrección](../../assets/fix.svg) **Contenedor de base de datos**: se corrigió la comprobación de estado del contenedor de base de datos agregando la contraseña de base de datos necesaria al comando de comprobación de estado.<!--MCLOUD-7122-->

   - ![nuevo icono](../../assets/new.svg) **contenedor de Elasticsearch**

      - Se agregó compatibilidad con Elasticsearch 7.9 para la compatibilidad con próximas versiones de Adobe Commerce.<!--MCLOUD-7190-->

      - **Configuración del complemento de Elasticsearch**: Se agregó compatibilidad para usar la información de configuración del complemento de Elasticsearch del archivo `services.yaml` a fin de generar el archivo `docker-compose.yaml` para un entorno de Cloud Docker para Commerce. Ver [complementos de Elasticsearch](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#elasticsearch-plugins).<!--MCLOUD-2789-->

      - **Compatibilidad con complementos de Elasticsearch**—Se agregó compatibilidad con los siguientes complementos de Elasticsearch: `analysis-icu`, `analysis-phonetic`, `analysis-stempel` y `analysis-nori`. Los complementos `analysis-icu` y `analysis-phonetic` están instalados de forma predeterminada. Puede agregar o quitar los complementos `analysis-stempel` y `analysis-nori` según sea necesario.<!--MCLOUD-2789-->

   - ![nuevo icono](../../assets/new.svg) **contenedor CLI**

      - **Ejecutar comandos dentro de contenedores de Docker PHP**—Ahora puede usar la CLI de Cloud Docker para ejecutar comandos dentro de contenedores de PHP en su entorno de Docker sin tener que instalar PHP en el host. Por ejemplo, el comando siguiente genera la configuración: `./bin/magento-docker php 7.3 vendor/bin/ece-docker build:compose`. Consulte [CLI de Cloud Docker](https://developer.adobe.com/commerce/cloud-tools/docker/quick-reference/#cloud-docker-cli). [Corrección enviada por G Arvind desde Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/209).<!--MCLOUD-5982-->

      - Se ha agregado el cliente OpenSSH a los contenedores CLI de PHP. Ahora, puede usar el reenvío de ssh-agent para Composer si el archivo `composer.json` contiene repositorios de Git privados que requieren que un cliente ssh use comandos de Composer.<!--MCLOUD-6008-->

   - ![Icono de corrección](../../assets/fix.svg) **Contenedor TLS**: Ahora, el [contenedor TLS](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#tls-container) se basa en la imagen Docker `https://hub.docker.com/r/magento/magento-cloud-docker-nginx` en lugar de en la imagen CentOS. Este cambio corrige los problemas que ocasionaban errores al enviar solicitudes HTTPS entre contenedores en el entorno Cloud Docker.<!--MCLOUD-6469-->

   - ![nuevo icono](../../assets/new.svg) **Contenedor de prueba**: se ha agregado un contenedor de prueba para la prueba de aplicaciones y se ha agregado la opción `--with-test` al comando `build:compose` del Docker para crear el contenedor únicamente cuando se realice la prueba en el entorno del Docker. Ver [prueba de aplicación](https://developer.adobe.com/commerce/cloud-tools/docker/test/application-testing/).<!--MCLOUD-6394-->

   - ![nuevo icono](../../assets/new.svg) **contenedor FPM-XDEBUG**

      - ![nuevo icono](../../assets/new.svg) **Configurar Xdebug en Linux**—Se ha agregado la opción `--set-docker-host` al comando `ece-docker build:compose` para configurar el valor `host.docker.internal` en el contenedor Xdebug. Esta opción es necesaria para utilizar Xdebug en sistemas Linux. Ver [Configurar Xdebug para Docker](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/).<!--MCLOUD-6430-->

      - ![icono de corrección](../../assets/fix.svg) Se ha corregido la configuración de la variable Xdebug para que Docker ENTRYPOINT resuelva `uninitialized "with_xdebug" variable` errores en los registros. [Corrección enviada por Florent Olivaud](https://github.com/magento/magento-cloud-docker/pull/218)<!--MCLOUD-6043-->

- ![nuevo icono](../../assets/new.svg) **cambios en la configuración de Docker**

   - **Configuración de MailHog**: Ahora puede usar las siguientes opciones de comando de `ece-docker build:compose` para deshabilitar MailHog y especificar puertos: `--no-mailhog`, `--mailhog-http-port` y `--mailhog-smtp-port`. Ver [Configurar correo electrónico](https://developer.adobe.com/commerce/cloud-tools/docker/configure/#set-up-email).<!--MCLOUD-6898, MCLOUD-6660-->

   - Para Cloud Docker para Commerce 1.2.0 y posterior, Adobe ahora proporciona imágenes de Docker para cada versión del parche y el generador de configuración de Docker crea la configuración de Docker con una versión de parche especificada en lugar de utilizar la última. Anteriormente, el generador de configuración de Docker creaba la configuración con la última versión de parche que podría romper Cloud Docker para entornos de Commerce creados con una versión anterior.<!--MCLOUD-7093-->

   - **Especificar imágenes y versiones personalizadas en la configuración personalizada de Cloud Docker**: se ha actualizado el comando `build:custom:compose` con opciones para especificar imágenes y versiones personalizadas al generar un archivo de configuración de composición personalizado de Docker (`docker-compose.yaml`). Ver [Crear una configuración de composición personalizada de Docker](https://developer.adobe.com/commerce/cloud-tools/docker/configure/custom-docker-compose/). <!--MCLOUD-7089-->

   - Se ha actualizado la configuración del host Docker para exponer el puerto 443 y habilitar el acceso a Adobe Commerce (`https://magento2.docker`) desde todos los contenedores CLI. Puede cambiar el puerto predeterminado agregando la opción `--tls-port` al generar el archivo de configuración de Docker.<!--MCLOUD-6806-->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que hacía que la compilación de Cloud Docker para Commerce fallara si el archivo `app/etc/env.php` existe.<!--MCLOUD-6732-->

- ![icono de corrección](../../assets/fix.svg) Se ha actualizado la configuración de compilación para reemplazar los volúmenes con nombre por volúmenes normales a fin de evitar problemas al implementar Cloud Docker para Commerce en Linux o el subsistema de Windows para Linux (WSL2).<!--MCLOUD-6732-->

- ![Icono de correcciones](../../assets/fix.svg) Se ha actualizado Cloud Docker para que las pruebas funcionales de Commerce admitan Composer 2.0.<!--MCLOUD-7183-->

## Versión 1.1.2

Fecha de la versión: 9 de septiembre de 2020

- ![nuevo icono](../../assets/new.svg) agregó compatibilidad con Elasticsearch 7.7<!--MCLOUD-6219-->

## Versión 1.1.1

Fecha de lanzamiento: 5 de agosto de 2020

- ![Icono de corrección](../../assets/fix.svg) **Configuración de correo electrónico actualizada**: se ha actualizado la configuración predeterminada de Cloud Docker para Commerce para que admita el servicio MailHog en lugar de usar SendMail. Ver [Configurar correo electrónico](https://developer.adobe.com/commerce/cloud-tools/docker/configure/#set-up-email).<!--MCLOUD-5624-->

- ![icono de corrección](../../assets/fix.svg) Restauró la biblioteca PS en la configuración del entorno de Cloud Docker para corregir `ps:  command not found` errores.<!--MCLOUD-6621-->

- ![Icono de corrección](../../assets/fix.svg) Se ha actualizado la configuración predeterminada de Cloud Docker para Commerce a fin de quitar el montaje automático de los volúmenes MariaDB y el punto de entrada de la base de datos para corregir `Cannot create container for service db` errores que pueden producirse al iniciar el entorno Cloud Docker.

  Ahora puede configurar el entorno Cloud Docker para montar los directorios de la base de datos agregando las siguientes opciones al comando `ece-docker build:compose`: `--with-entry-point` y `with-mariadb-conf`. Ver [opciones de configuración del servicio](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#service-configuration-options).<!--MCLOUD-6424-->

- ![nuevo icono](../../assets/new.svg) **actualizaciones del comando CLI**

| Acción | Comando |
| ------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| Agregar un punto de entrada al contenedor de base de datos para restaurar la base de datos desde la copia de seguridad | `./vendor/bin/ece-docker build:compose --db --with-entrypoint` |
| Agregar un volumen de configuración de MariaDB | `./vendor/bin/ece-docker build:compose --db --mariadb-conf` |

## Versión 1.1.0

Fecha de publicación: 25 de junio de 2020

- ![nuevo icono](../../assets/new.svg) **Se ha agregado compatibilidad con la solución de rendimiento de bases de datos divididas**. Ahora puede configurar e implementar un almacén mediante la solución de rendimiento de bases de datos divididas en el entorno Cloud Docker.<!--MCLOUD-3740-->

- ![nuevo icono](../../assets/new.svg) **Soporte para la implementación de Adobe Commerce y Magento Open Source**—Ahora puede usar Cloud Docker para Commerce para implementar un entorno de desarrollo local para proyectos que no estén alojados en Adobe Commerce en la infraestructura en la nube.<!--MCLOUD-5667-->

- ![nuevo icono](../../assets/new.svg) **compatibilidad con Blackfire.io**—Se ha agregado compatibilidad para usar la extensión [Blackfire.io](https://developer.adobe.com/commerce/cloud-tools/docker/test/blackfire/) para las pruebas de rendimiento automatizadas. [Corrección enviada por Adarsh Manickam desde Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/202)<!--MCLOUD-5857-->

- ![nuevo icono](../../assets/new.svg) **Actualizaciones de contenedor**

   - **Varnish**: Ahora Varnish es la memoria caché predeterminada al implementar Adobe Commerce en un entorno de Cloud Docker mediante una versión compatible de la plantilla de aplicaciones de Cloud. Ver [contenedor de barniz](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#varnish-container).<!--MCLOUD-2634-->

   - Se agregó la opción `--no-varnish` para omitir la instalación del servicio Varnish al generar el archivo de configuración de Cloud Docker.<!--MCLOUD-2634-->

   - ![nuevo icono](../../assets/new.svg) **Base de datos**

      - Se ha añadido la compatibilidad con la base de datos MySQL. Ahora puede configurar el entorno de Cloud Docker con MariaDB o MySQL. Ver [opciones de configuración del servicio](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#service-configuration-options).<!--MCLOUD-5691-->

      - Se ha añadido la capacidad de definir la configuración de incremento y desplazamiento para la replicación de bases de datos al generar el archivo de composición Docker. Ver [contenedores de servicio](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#service-containers).<!--MCLOUD-5735-->

   - ![nuevo icono](../../assets/new.svg) **PHP-FPM**

      - Se agregó compatibilidad con PHP 7.4. [Corrección enviada por Mohanela Murugan de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/198)<!--MCLOUD-198-->

      - Se ha agregado la capacidad de copiar un archivo de `php.ini` en el directorio raíz del proyecto al entorno de Cloud Docker y aplicar la configuración de PHP personalizada a los contenedores de PHP-FPM y CLI. Ver [Personalizar la configuración de PHP](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#customize-php-settings). [Corrección enviada por Mathew Beane de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/130).<!--MCLOUD-6012-->

      - Se ha añadido una comprobación de estado del contenedor. [Corrección enviada por Visanth Sampath desde Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/188).<!--MCLOUD-5752-->

   - ![Icono de corrección](../../assets/fix.svg) **Node.js**: se ha actualizado la versión predeterminada de Node.js de la versión 8 a la versión 10 para mejorar la seguridad. La versión 8 de Node.js está obsoleta y ya no se actualiza con correcciones de errores ni parches de seguridad. [Corrección enviada por Mohan Elamurugan de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/183).<!--MCLOUD-5586-->

   - ![nuevo icono](../../assets/new.svg) **Elasticsearch**

      - Se agregó compatibilidad con Elasticsearch 6.8, 7.2, 7.5 y 7.6.<!--MCLOUD-4050, MCLOUD-5855,MCLOUD-5860-->

      - Se ha agregado la capacidad de personalizar la [configuración del contenedor de Elasticsearch](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#elasticsearch-container) al generar el archivo de configuración de composición Docker.<!--MCLOUD-3059-->

      - Se ha agregado la opción `--no-es` a las opciones de configuración del servicio para generar el archivo de configuración Docker Compose. Utilice esta opción para omitir la instalación del contenedor de Elasticsearch y utilice la búsqueda MySQL en su lugar. Esta opción solo es compatible con las versiones 2.3.5 y anteriores de Adobe Commerce.<!--MCLOUD-3766-->

   - ![nuevo icono](../../assets/new.svg) **contenedor FPM-XDEBUG**—Se ha agregado una opción de configuración de servicio para instalar y configurar Xdebug para depurar PHP en su entorno Cloud Docker. Ver [Configurar Xdebug](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/).<!--MCLOUD-4098-->

- ![nuevo icono](../../assets/new.svg) **cambios en la configuración de Docker**

   - Se agregaron comprobaciones de estado para los contenedores de servicio PHP-FPM, Redis, Elasticsearch y MySQL Docker.<!--MCLOUD-3335 and MCLOUD-5856-->

   - Se cambió el modo de sincronización de archivos predeterminado a `native` en el modo de desarrollador.<!--MCLOUD-3890 -->

   - Se agregó información de versión a la imagen genérica del contenedor del servicio Docker al generar el archivo `docker-compose.yml`.<!--MCLOUD-3878-->

   - Se ha mejorado la capacidad de gestionar respuestas de gran tamaño desde el contenedor PHP-FPM ascendente mediante el aumento del valor `fastcgi_buffers` para el servidor Nginx.<!--MCLOUD-5980-->

   - Se mejoró el rendimiento de la sincronización de archivos mutagen al agregar una segunda sesión de sincronización para sincronizar archivos en el directorio `vendor`. Este cambio evita que el mutágeno se bloquee durante el proceso de sincronización de archivos. [Corrección enviada por Mathew Beane de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/127).<!--MCLOUD-6010-->

   - ![nuevo icono](../../assets/new.svg) **actualizaciones del comando CLI**

| Acción | Comando |
| -------- | --------------- |
| Borrar caché de Redis | `bin/magento-docker flush-redis` |
| Borrar caché de barniz | `bin/magento-docker flush-varnish` |
| Omitir instalación predeterminada de barniz | `.vendor/bin/ece-docker build:compose --no-varnish`<!--MCLOUD-2634--> |
| [Personalizar opciones de Elasticsearch](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#elasticsearch-container) | `.vendor/bin/ece-docker build:compose --es-env-var`<!--MCLOUD-3059--> |
| [Quitar la configuración de Elasticsearch](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#elasticsearch-container) | `.vendor/bin/ece-docker build:compose --no-es`<!--MCLOUD-3766--> |
| Configurar el contenedor de base de datos con MySQL versión 5.6 o 5.7 | `./vendor/bin/ece-docker build:compose --db <mysql-version-number> --db-image mysql`<!--MCLOUD-5691--> |
| Especificar una URL base personalizada | `./vendor/bin/ece-docker build:compose --host=<hostname> --port=<port-number>`<!--MCLOUD-3063--> |
| [Agregar contenedor para la configuración de Xdebug](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/) | `.vendor/bin/ece-docker build:compose --mode developer --sync-engine native --with-xdebug`<!--MCLOUD-4098--> |

- ![icono de corrección](../../assets/fix.svg) Se ha corregido la configuración de la sincronización de archivos mutagen para evitar que mutagen cree sesiones antiguas. [Corrección enviada por Mathew Beane de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/127).<!--MCLOUD-6010-->

- ![Icono de corrección](../../assets/fix.svg) Se ha corregido un problema de configuración que causaba errores de sintaxis en el registro de composición de Docker al iniciar el contenedor de PHP-FPM. [Corrección enviada por Mathew Beane de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/129)<!--MCLOUD-3958-->

- ![icono de corrección](../../assets/fix.svg) Se corrigieron errores de conflictos de volumen que a veces se producían al utilizar varios entornos Docker. [Corrección enviada por G Arvind desde Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/168).

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que provocaba que fallara el comando `ece-docker build:compose` si la configuración incluía Blackfire.io. [Corrección enviada por G Arvind desde Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/199). <!--MCLOUD-5797-->

- ![Icono de corrección](../../assets/fix.svg) Se ha actualizado la configuración de la imagen de la CLI de PHP para evitar errores de memoria insuficiente que se produjeron al instalar varios paquetes mediante Cloud Docker para Commerce. [Corrección enviada por Mohan Elamurugan de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/197).*<!--MCLOUD-5818-->

- ![Icono de corrección](../../assets/fix.svg) Se agregó compatibilidad con varios usuarios de MySQL en el entorno Cloud Docker. En versiones anteriores, la operación `build:compose` fallaba si el archivo `magento.app.yaml` especificaba varios usuarios de la base de datos. [Corrección enviada por G Arvind desde Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/181).<!--MCLOUD-5670-->

- ![Icono de corrección](../../assets/fix.svg) eliminó a `rsyslog` de los contenedores de Cloud Docker para Commerce PHP para resolver problemas de compatibilidad que causaban notificaciones de advertencia durante la implementación. Cloud Docker no utiliza la utilidad rsyslog.<!--MCLOUD-6173-->

## Versión 1.0.0

Fecha de la versión: 5 de febrero de 2020

- ![nuevo icono](../../assets/new.svg) **Se creó un paquete separado para entregar`Cloud Docker for Commerce`**—Se movió el código fuente para entregar Cloud Docker para Commerce desde el repositorio `ece-tools` al [nuevo repositorio `magento-cloud-docker`](https://github.com/magento/magento-cloud-docker) para mantener la calidad del código y proporcionar versiones independientes. El nuevo paquete es una dependencia para ECE-Tools v2002.1.0 y posterior.

  Al actualizar ece-tools, también se actualiza el paquete `magento/magento-cloud-docker` a la versión 1.0.0. Si ha utilizado Cloud Docker para Commerce con una versión anterior de `ece-tools` (2002.0.x), revise las [incompatibilidades con versiones anteriores](backward-incompatible-changes.md) y actualice el proyecto como scripts, comandos y procesos según sea necesario.

- ![nuevo icono](../../assets/new.svg) **Se ha agregado el control de versiones a las imágenes de Docker**. Ahora debe actualizar el paquete `magento/magento-cloud-docker` para obtener las imágenes actualizadas.<!--MAGECLOUD-4737-->

- ![nuevo icono](../../assets/new.svg) **Actualizaciones de contenedor**—

   - ![nuevo icono](../../assets/new.svg) **contenedor PHP-FPM**—

      - ![nuevo icono](../../assets/new.svg) **Se agregó compatibilidad con Node.js**: Se actualizó la imagen PHP-FPM para admitir el nodo, npm y las capacidades grunt-cli dentro del contenedor PHP.<!--MAGECLOUD-3953-->

      - ![nuevo icono](../../assets/new.svg) **Se agregó compatibilidad con [ionCube](https://www.ioncube.com/)**—Se actualizó la configuración predeterminada de Docker para admitir ionCube en el entorno de desarrollo local de Docker.<!--MAGECLOUD-4354-->

   - ![nuevo icono](../../assets/new.svg) **contenedor web**—

      - ![nuevo icono](../../assets/new.svg) **Personalizar la configuración de NGINX**—Se ha agregado la capacidad de montar un archivo `nginx.conf` personalizado en el entorno Cloud Docker para Commerce. Ver [contenedor web](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#web-container).<!--MAGECLOUD-4204-->

      - ![nuevo icono](../../assets/new.svg) **Certificados NGINX generados automáticamente**—El archivo de configuración de Docker ahora incluye la configuración para generar automáticamente certificados NGINX para el contenedor web.<!--MAGECLOUD-4258-->

   - ![nuevo icono](../../assets/new.svg) **Nuevo contenedor de Selenium**—Se agregó un [contenedor de Selenium](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#selenium-container) para admitir las pruebas de aplicaciones de Adobe Commerce mediante el Marco de prueba funcional de Magento (MFTF).<!--MAGECLOUD-4040-->

   - ![nuevo icono](../../assets/new.svg) **[!DNL RabbitMQ]compatibilidad con la versión**—Se ha actualizado la configuración del contenedor [!DNL RabbitMQ] para admitir [!DNL RabbitMQ] versión 3.8.<!--MAGECLOUD-4674-->

   - ![icono de corrección](../../assets/fix.svg) **Contenedor de base de datos persistente**: el volumen de base de datos `magento-db: /var/lib/mysql` ahora persiste después de detener y quitar la configuración de Docker y se restaura al reiniciar la configuración de Docker. Ahora debe eliminar manualmente el volumen de la base de datos. Ver [contenedores de base de datos].<!--MAGECLOUD-3978-->

   - ![nuevo icono](../../assets/new.svg) **contenedor TLS**—

      - ![nuevo icono](../../assets/new.svg) **Se ha actualizado la imagen base del contenedor para que utilice la imagen oficial**—La imagen del contenedor [Cloud TLS](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#tls-container) ahora se basa en la imagen oficial `debian:jessie` del Docker.—<!--MAGECLOUD-4163-->

      - ![nuevo icono](../../assets/new.svg) **Se ha agregado compatibilidad con el [proxy de terminación TLS en libras]**—El [archivo de configuración de libras](https://github.com/magento/magento-cloud-docker/blob/1.0/images/tls/) agrega las siguientes variables ENV para personalizar la configuración de Docker para el contenedor TLS:

         - **`TimeOut`**: establece el valor de tiempo de espera de Tiempo en primer byte (TTFB). El valor predeterminado es 300 segundos.

         - **`RewriteLocation`**: determina si el proxy de libras reescribe la ubicación en la dirección URL de la solicitud de forma predeterminada. El valor predeterminado es `0` para evitar que la reescritura interrumpa las redirecciones a sitios web externos como un sitio de SSO externo. [Corrección enviada por Sorin Sugar](https://github.com/magento/magento-cloud-docker/pull/37)<!--MAGECLOUD-4061-->

      - ![nuevo icono](../../assets/new.svg): se ha aumentado el valor de tiempo de espera en la configuración del contenedor TLS de 15 a 300 segundos. [Corrección enviada por Mathew Beane de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/78)<!--MAGECLOUD-4460-->

   - ![nuevo icono](../../assets/new.svg) **contenedor de barniz**—

      - ![nuevo icono](../../assets/new.svg) **Se ha actualizado la imagen base del contenedor para que utilice la imagen oficial**—El [contenedor de barniz de nube](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#varnish-container) se basa ahora en la imagen Docker `centos` oficial.<!--MAGECLOUD-4163-->

      - ![nuevo icono](../../assets/new.svg) **Se mejoró la configuración de tiempo de espera predeterminada**- Se agregó la configuración `.first_byte_timeout` y `.between_bytes_timeout` al contenedor de Varnish. Ambos valores de tiempo de espera tienen un valor predeterminado de `300s` (5 minutos). [Corrección enviada por Mathew Beane de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/78)<!--MAGECLOUD-4460-->

      - ![icono de corrección](../../assets/fix.svg) **Omitir barniz durante sesiones de Xdebug**—Se ha actualizado la configuración del contenedor de Varnish para que devuelva `pass` en las solicitudes recibidas cuando Xdebug está habilitado. En versiones anteriores, no se podía utilizar Xdebug si el entorno Docker incluía Varnish. [Corrección enviada por Mathew Beane de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/111).<!--MAGECLOUD-4873-->

- ![nuevo icono](../../assets/new.svg) **cambios en la configuración de Docker**—

   - ![nuevo icono](../../assets/new.svg) **Administrar montajes y volúmenes para su proyecto**—Se agregó la capacidad de administrar montajes y volúmenes al iniciar un entorno Docker para el desarrollo local. Ver [compartir datos de proyecto].<!--MAGECLOUD-3248-->

   - ![nuevo icono](../../assets/new.svg) **Compatibilidad con el modo de puente de red**—Se agregó compatibilidad con el modo de puente de red para habilitar conexiones entre contenedores Docker a través de la red local.<!--MAGECLOUD-4165-->

   - ![nuevo icono](../../assets/new.svg) **Contenedor de Cron deshabilitado de forma predeterminada**: para mejorar el rendimiento, el contenedor de Cron ya no está configurado de forma predeterminada al generar el entorno de Docker. Puede utilizar la opción `--with-cron` en el comando de generación de Docker para agregar un contenedor de Cron a su entorno. Ver [Administración de trabajos cron](https://developer.adobe.com/commerce/cloud-tools/docker/configure/manage-cron-jobs/).<!--MAGECLOUD-5181-->

   - ![nuevo icono](../../assets/new.svg) **Detener la sincronización de archivos de copia de seguridad grandes**—Volcados de base de datos agregados y archivos de archivo—ZIP, SQL, GZ y BZ2—a la lista de exclusión en los archivos `dist/docker-sync.yml` y `dist/mutagen.sh`. La sincronización de archivos grandes (>1 GB) puede causar un período de inactividad y los archivos de copia de seguridad no suelen requerir sincronización, ya que se pueden regenerar.<!--MAGECLOUD-3979-->

- ![nuevo icono](../../assets/new.svg) **Cambios de comando**—

   - ![icono de corrección](../../assets/fix.svg) Cambió el nombre del archivo `./bin/docker` a `./bin/magento-docker` para corregir un problema que hacía que algunos entornos Docker se dañaran porque el archivo `./bin/docker` sobrescribe los archivos binarios Docker existentes. Este es un [cambio incompatible con versiones anteriores](backward-incompatible-changes.md) que requiere actualizaciones de los scripts y comandos.<!-- MAGECLOUD-4038 -->

   - ![nuevo icono](../../assets/new.svg) **Se agregó una opción de configuración de servicio para exponer el puerto de base de datos al host**—Utilice la opción `--expose-db-port= [Fix submitted by Adarsh Manickam from Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/101).<PORT>` para exponer el puerto de base de datos al host al generar el archivo `docker-compose.yml`: `bin/ece-docker build:compose --expose-db-port=<PORT>`<!--MAGECLOUD-4454-->

   - ![nuevo icono](../../assets/new.svg) **Nuevo comando posterior a la implementación**—Anteriormente, los vínculos posteriores a la implementación definidos en el archivo `.magento.app.yaml` se ejecutaban automáticamente después de implementar Adobe Commerce en un contenedor de Cloud Docker mediante el comando `cloud-deploy`. Ahora debe emitir un comando `cloud-post-deploy` independiente para ejecutar los vínculos posteriores a la implementación después de implementar. Vea las instrucciones de inicio actualizadas para el modo [desarrollador](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/developer-mode/) y [producción](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/production-mode/).<!--MAGECLOUD-3996-->

   - ![nuevo icono](../../assets/new.svg) agregó la opción `--rm` a `./bin/magento-docker` comandos para generar e implementar contenedores. Quita el contenedor una vez completada la tarea.<!--MAGECLOUD-4205-->

   - ![nuevo icono](../../assets/new.svg) **Actualizaciones del comando `build:compose`**—

      - ![nuevo icono](../../assets/new.svg) agregó la opción `--sync-engine="native"` al comando `docker-build` para deshabilitar la sincronización de archivos cuando genere el archivo de configuración Docker Compose en modo de desarrollador. Utilice esta opción cuando desarrolle en sistemas Linux, que no requieren sincronización de archivos para el desarrollo local de Docker. Ver [Sincronización de datos en el entorno de Docker](https://developer.adobe.com/commerce/cloud-tools/docker/setup/synchronize-data/).<!--MCLOUD-3231, MCLOUD-3890-->

   - ![nuevo icono](../../assets/new.svg) cambió la configuración predeterminada de sincronización de archivos de `docker-sync` a `native`. [Corrección enviada por Mathew Beane de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/124).<!--MAGECLOUD-5066-->

- ![nuevo icono](../../assets/new.svg) **Mejoras de validación**—

   - ![nuevo icono](../../assets/new.svg) Se agregó la validación al proceso de implementación para los entornos de desarrollo locales de Docker a fin de verificar que la configuración del entorno de nube incluye la clave de cifrado necesaria para descifrar la base de datos. Ahora, recibirá un mensaje de error en el registro si la configuración del entorno no especifica un valor para la clave de cifrado.<!--MAGECLOUD-4423-->

   - ![nuevo icono](../../assets/new.svg) agregó una comprobación de estado del contenedor al servicio Elasticsearch para asegurarse de que el servicio está listo antes de continuar con el procesamiento de la compilación e implementación. Si la comprobación de estado devuelve un error, el contenedor se reinicia automáticamente.<!--MAGECLOUD-4456-->
