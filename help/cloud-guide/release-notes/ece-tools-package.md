---
title: Notas de la versión de ECE-Tools
description: Vea una lista de las mejoras más recientes del paquete ECE-Tools.
recommendations: noDisplay, catalog
last-substantial-update: 2025-08-07T00:00:00Z
exl-id: 3cbfe698-d75d-4a16-877a-52c214595344
source-git-commit: e39b348b02b4edfbe091420c54a20b320387a4b6
workflow-type: tm+mt
source-wordcount: '3286'
ht-degree: 0%

---

# Notas de la versión de ECE-Tools

El paquete [ece-tools](https://github.com/magento/ece-tools) es un conjunto de scripts y herramientas diseñadas para administrar e implementar proyectos en la nube. En estas notas de la versión se describen las mejoras más recientes realizadas en este paquete, que forma parte de [Cloud Tools Suite for Commerce](cloud-tools-suite.md).

>[!NOTE]
>
>Consulte [Actualizar ECE-Tools](../dev-tools/update-package.md) para obtener información sobre cómo actualizar a la última versión del paquete `ece-tools`.

El paquete `ece-tools` utiliza la siguiente secuencia de versiones de la versión: `200<major>.<minor>.<patch>`

Las notas de la versión incluyen:

- ![nuevo icono](../../assets/new.svg) Nuevas características
- ![icono de corrección](../../assets/fix.svg) Correcciones y mejoras

<!--Add release notes below-->

## v2002.2.8 {#latest}

Fecha de la versión: 8 de octubre de 2025

- Se ha agregado compatibilidad con ![nuevo icono](../../assets/new.svg) **ActiveMQ** para ActiveMQ.<!-- MCLOUD-13770 -->
- ![nuevo icono](../../assets/new.svg) **ActiveMQ** agregó pruebas funcionales.<!-- MCLOUD-13813 -->


## v2002.2.7

Fecha de lanzamiento: 7 de agosto de 2025

- ![icono de corrección](../../assets/fix.svg) **Correcciones de PHP 8.4**: compatibilidad de tipos agregada.<!-- MCLOUD-13965 -->
- ![icono de corrección](../../assets/fix.svg) **Validador de fin de vida útil**: fechas de servicio de fin de vida útil (EOL) actualizadas.<!-- MCLOUD-13929 -->
- ![nuevo icono](../../assets/new.svg) **Valkey** agregó pruebas funcionales de PHP 8.2 y PHP 8.3.<!-- MCLOUD-13610 -->
- ![icono de corrección](../../assets/fix.svg) **Validador de Valkey**: se ha corregido el mensaje de advertencia de las herramientas ECE.<!-- MCLOUD-13896 -->
- ![icono de corrección](../../assets/fix.svg) **herramientas ECE**: se agregaron mejoras en las pruebas unitarias.<!-- MCLOUD-13838 -->
- ![nuevo icono](../../assets/new.svg) **Validador para servicios**: nuevas versiones añadidas compatibles con Opensearch, MariaDB y PHP.<!-- MCLOUD-13923 -->
- ![nuevo icono](../../assets/new.svg) **Opensearch3** agregó compatibilidad con Opensearch3.<!-- MCLOUD-13763 -->
- ![icono de corrección](../../assets/fix.svg) **Compatibilidad con Opensearch para 2.4.4-p7/p12**: se ha actualizado el script de validación.<!-- MCLOUD-13945 -->
- ![nuevo icono](../../assets/new.svg) **Opensearch3 prueba** pruebas funcionales agregadas.<!-- MCLOUD-13769 -->

## v2002.2.6

Fecha de publicación: 3 de junio de 2025

- ![Icono de corrección](../../assets/fix.svg) **Se ha mejorado la compatibilidad con las bibliotecas de terceros de 2.4.8** y se han actualizado para mejorar la compatibilidad con 2.4.8<!-- MCLOUD-13707 -->

## v2002.2.5

Fecha de la versión: 27 de mayo de 2025

- ![nuevo icono](../../assets/new.svg) **Compatibilidad con Valkey extendida**-Compatibilidad con Valkey extendida en Adobe Commerce.<!-- MCLOUD-13595 -->
- ![Icono de corrección](../../assets/fix.svg) **Validador actualizado de RabbitMQ**: validador actualizado para RabbitMQ.<!-- MCLOUD-13589 -->
- ![icono de corrección](../../assets/fix.svg) **Se ha actualizado el validador de MariaDB**-Se ha actualizado el validador de ece-tools para MariaDB 10.11.<!-- MCLOUD-13593 -->
- ![Icono de corrección](../../assets/fix.svg) **Compatibilidad con Opensearch2 ampliada**: Opensearch2 compatible con las últimas versiones de la versión 2.4.4.<!-- MCLOUD-13710 -->

## v2002.2.4

Fecha de publicación: 24 de abril de 2025

- ![Icono de corrección](../../assets/fix.svg) **Opensearch2 para 2.4.4/2.4.5**—Se ha corregido un problema relacionado con la compatibilidad con `opensearch2` en las versiones de Adobe Commerce 2.4.4/2.4.5.<!-- MCLOUD-13607 -->

## v2002.2.3

Fecha de publicación: 9 de abril de 2025

- ![Icono de correcciones](../../assets/fix.svg) **Corrección de Valkey** Se ha corregido un problema con la configuración personalizada de Valkey.<!-- MCLOUD-13569 -->
- ![Icono de correcciones](../../assets/fix.svg) **Validador de correcciones**: validador fijo para RabbitMQ 4.0.<!-- MCLOUD-13560 -->

## v2002.2.2

Fecha de publicación: 7 de abril de 2025

## v2002.2.2

Fecha de publicación: 7 de abril de 2025

- ![nuevo icono](../../assets/new.svg) **Valkey**—Se agregó compatibilidad con un nuevo servicio (Valkey), que reemplaza a Redis.<!-- MCLOUD-13455 -->
- ![Icono de corrección](../../assets/fix.svg) **Opensearch2 para 2.4.4/2.4.5**—Se ha agregado compatibilidad con `opensearch2` en las versiones de Adobe Commerce 2.4.4/2.4.5.<!-- MCLOUD-13493 -->

## v2002.2.1

Fecha de publicación: 6 de febrero de 2024

- ![nuevo icono](../../assets/new.svg) **PHP 8.4**—Se agregó compatibilidad con PHP 8.4.<!-- MCLOUD-13145 -->
- ![Icono de corrección](../../assets/fix.svg) **Validador para Opensearch**: se ha corregido el validador que generaba un mensaje engañoso acerca de la versión incorrecta del servicio.<!-- MCLOUD-13184 -->

## v2002.2.0

Fecha de la versión: 7 de octubre de 2024

- ![nuevo icono](../../assets/new.svg) **MariaDB 11.4**: se ha agregado compatibilidad con MariaDB 11.4.
- ![Icono de corrección](../../assets/fix.svg) **Código refactorizado**: se ha eliminado la compatibilidad con las versiones anteriores de PHP 7.4, 7.3, 7.2 y las bibliotecas relacionadas.<!-- MCLOUD-9278 -->
- ![Icono de corrección](../../assets/fix.svg) **Se ha actualizado la versión de monólogo**. Se ha agregado compatibilidad con monólogo 3.6.<!-- MCLOUD-12855 -->
- ![Icono de corrección](../../assets/fix.svg) **Validador para RabbitMQ, MariaDB y PHP**: se ha corregido el validador que generaba un mensaje engañoso acerca de la versión incorrecta del servicio.

## v2002.1.19

Fecha de la versión: 21 de mayo de 2024

- ![nuevo icono](../../assets/new.svg) **Lua**—Se agregó la opción useLua para CACHE_CONFIGURATION.
- ![Icono de corrección](../../assets/fix.svg) **Validador**: validadores actualizados para las nuevas versiones de Redis y RabbitMQ.

## v2002.1.18

Fecha de publicación: 8 de abril de 2024

- ![nuevo icono](../../assets/new.svg) **PHP** — Se agregó compatibilidad con PHP 8.3.
- ![Icono de corrección](../../assets/fix.svg) **Validador**: se ha actualizado el validador de EOL.

## v2002.1.17

Fecha de la versión: 16 de enero de 2024

- ![Icono de corrección](../../assets/fix.svg) **Validador para Elasticsearch y OpenSearch**: se ha corregido el validador que generaba un mensaje engañoso para instalar un servicio de búsqueda cuando LiveSearch estaba habilitado.<!-- MCLOUD-10167 -->
- ![icono de corrección](../../assets/fix.svg) **Advertencia de implementación**: se ha corregido un problema que provocaba advertencias de implementación sobre carpetas que no estaban vacías.<!-- MCLOUD-8958 -->

## v2002.1.16

Fecha de la versión: 16 de octubre de 2023

- ![nuevo icono](../../assets/new.svg) **ENABLE_WEBHOOKS variable de entorno global**—Se agregó la variable global [ENABLE_WEBHOOKS](../environment/variables-global.md#enable_webhooks) para usarla con los webhooks de Commerce para conectarse a un extremo externo, como la acción de tiempo de ejecución de App Builder o un sistema de administración de inventario de terceros.

## v2002.1.15

Fecha de la versión: 31 de julio de 2023

- ![icono de corrección](../../assets/fix.svg) **Códigos de error**: se ha actualizado el esquema de código de error y el generador de documentos de código de error.
- ![Icono de corrección](../../assets/fix.svg) **Validador para el modelo Redis personalizado**: se ha actualizado el validador para los modelos backend Redis personalizados. [Vea el ejemplo de configuración de caché](../environment/variables-deploy.md#cache_configuration).
- ![Icono de corrección](../../assets/fix.svg) **Validador para RabbitMQ**: se ha agregado compatibilidad con RabbitMQ 3.11
- ![Icono de corrección](../../assets/fix.svg) **Se corrigió el vínculo incorrecto**-Se corrigió el vínculo incorrecto a la documentación de incorporación en la plantilla de correo electrónico de bienvenida.

## v2002.1.14

Fecha de la versión: 10 de marzo de 2023

- ![nuevo icono](../../assets/new.svg) **PHP**—Se agregó compatibilidad con PHP 8.2.
- ![nuevo icono](../../assets/new.svg) **Validadores para servicios**—Se han actualizado los validadores para Commerce 2.4.6 servicios necesarios: MariaDB 10.6, Redis 7.0, PHP 8.2, OpenSearch 2.x y RabbitMQ 3.9.
- ![icono de corrección](../../assets/fix.svg) **ece-tools db-dump**—Se ha corregido un problema que provocaba que la operación de `db-dump` se detuviera antes de tiempo.

## v2002.1.13

Fecha de la versión: 27 de octubre de 2022

- ![nuevo icono](../../assets/new.svg) **Se agregó compatibilidad con Adobe I/O Events para Adobe Commerce**. Los desarrolladores de extensiones ahora pueden usar el marco de trabajo [Adobe I/O Events](https://developer.adobe.com/events/docs/) para enviar información de evento de Commerce desde instancias de nube a sus aplicaciones escritas para [Adobe App Builder](https://developer.adobe.com/app-builder/docs/overview/). Adobe I/O Events para Adobe Commerce se encuentra en Vista previa de socio.<!-- CEXT-932 -->
- ![nuevo icono](../../assets/new.svg) **Validador para la configuración de OPcache**—Se agregó un validador para comprobar la configuración de OPcache en busca de rutas excluidas.<!-- MCLOUD-9485 -->
- ![Icono de corrección](../../assets/fix.svg) **Se ha corregido un problema con la configuración de la caché de GraphQL**. Ahora, ECE-Tools mantiene el valor de GraphQL `id_salt` en la configuración de `cache` en el archivo `app/etc/env.php`.<!-- MCLOUD-9486 -->

## v2002.1.12

Fecha de la versión: 13 de septiembre de 2022

- ![nuevo icono](../../assets/new.svg) **Habilitar`synchronous_replication`**—ECE-Tools establece `synchronous_replication=>true` en el archivo `app/etc/env.php` cuando `MYSQL_USE_SLAVE_CONNECTION` está habilitado. Esta configuración solo afecta a Commerce 2.4.6+. Vea la descripción de la variable `MYSQL_USE_SLAVE_CONNECTION` en [Implementar variables](../environment/variables-deploy.md#mysql_use_slave_connection).<!-- MCLOUD-9142 -->
- ![nuevo icono](../../assets/new.svg) **OpenSearch**: funcionalidad añadida para configurar y establecer el motor `opensearch` para la próxima versión 2.4.6 de Adobe Commerce. Consulte [Configurar el servicio OpenSearch](../services/opensearch.md).<!-- MCLOUD-9236 -->

## v2002.1.11

Fecha de lanzamiento: 4 de agosto de 2022

- ![Icono de corrección](../../assets/fix.svg) **ElasticSuite Validator y OpenSearch**: se ha corregido un problema con el validador de comprobación de integridad de ElasticSuite al instalar OpenSearch.<!-- MCLOUD-8767 -->
- ![icono de corrección](../../assets/fix.svg) **Tipos de valor devuelto para los comandos de implementación**—Tipos de valor devuelto fijos para los comandos de implementación.<!-- AC-3208 -->
- ![corregir el icono](../../assets/fix.svg) **[!DNL RabbitMQ]problema con la nueva instalación de Commerce 2.4.5**—Se ha corregido el [!DNL RabbitMQ] problema de bloqueo en la nueva instalación de Commerce 2.4.5.<!-- MCLOUD-9059 -->

## v2002.1.10

Fecha de la versión: 31 de marzo de 2022

- ![icono de corrección](../../assets/fix.svg) **Elasticsearch 7.10**: se han actualizado los validadores para que admitan la versión 7.10 de Elasticsearch.<!-- MCLOUD-8548 -->

## v2002.1.9

Fecha de la versión: 10 de marzo de 2022

- ![nuevo icono](../../assets/new.svg) **OpenSearch**: se ha agregado compatibilidad con OpenSearch para las versiones de Adobe Commerce 2.4.4, 2.4.3-p2 y 2.3.7-p3.<!-- MCLOUD-8296 -->
- ![nuevo icono](../../assets/new.svg) **PHP**—Se agregó compatibilidad con PHP 8.1.
- ![icono de corrección](../../assets/fix.svg) **symfony/process**—Se ha agregado la compatibilidad con symfony/process ^5.3.<!-- MCLOUD-8283 -->

- ![nuevo icono](../../assets/new.svg) **Procesos múltiples de consumidor**—Se ha agregado una opción `multiple_processes` para que pueda especificar el número de procesos que se generarán para cada consumidor. Vea la descripción de la variable `CRON_CONSUMERS_RUNNER` en [Implementar variables](../environment/variables-deploy.md#cron_consumers_runner).<!-- MCLOUD-8295 -->
- ![nuevo icono](../../assets/new.svg) **esquema OpenSearch y ruta de acceso completa al host**—Se ha agregado la capacidad de configurar un esquema Elasticsearch y una ruta de acceso completa al host.
- ![icono de corrección](../../assets/fix.svg) **AWS S3**—Se ha cambiado el método de habilitación de AWS S3.
- ![Icono de correcciones](../../assets/fix.svg) **Corrección de driver_options reader**—Se ha agregado la lectura de driver_options para la conexión DB desde el archivo `env.php` por `ece-tools` para los validadores.<!-- MCLOUD-8420 -->

## v2002.1.8

Fecha de la versión: 25 de octubre de 2021

- ![nuevo icono](../../assets/new.svg) **Ubicación de volcado alternativa**—Se ha agregado la opción `--dump-directory` para que pueda elegir un directorio de destino para un volcado de la base de datos. Ahora `/app/var/dump-main` es el directorio de destino predeterminado para un volcado de la base de datos. Ver [Administración de copias de seguridad: volcar la base de datos](../storage/database-dump.md)<!-- MCLOUD-8063 -->
- ![Icono de corrección](../../assets/fix.svg) **Actualizar monólogo**: se ha actualizado la versión mínima requerida para el paquete `monolog` a `^2.3`.<!-- ACMP-1263 -->
- ![icono de corrección](../../assets/fix.svg) **Actualizar Symfony**: se han actualizado las dependencias de Symfony para que sean compatibles con Adobe Commerce 2.4.4.<!-- ACMP-1533 -->
- ![Icono de corrección](../../assets/fix.svg) **Carga automática de característica/resolución**: se corrigió un problema que se producía al implementar en un entorno de integración y al ver el error `CRITICAL: [9] Required configuration is missed in autoload section of composer.json file.`.<!-- https://github.com/magento/ece-tools/pull/799 -->

## v2002.1.7

Fecha de la versión: 29 de julio de 2021

**Actualizaciones de configuración**—

- ![nuevo icono](../../assets/new.svg) agregó compatibilidad con Composer 2.0.<!--MCLOUD-8003-->

- ![Icono de corrección](../../assets/fix.svg) **Se han actualizado los requisitos del compositor para`symphony/console`**: se han actualizado los requisitos de versión de ECE-Tools `composer.json` para el paquete `symphony/console` a fin de corregir un problema que causaba que los comandos `di:compile` dieran el siguiente error: `Incompatible argument type: Required type: int. Actual type: string`<!--MC-42919-->

- ![icono de corrección](../../assets/fix.svg) Se han actualizado las comprobaciones del software al final de su vida útil (`eol.yaml`) para incluir Elasticsearch 7.9.x.<!--MCLOUD-7938-->

## v2002.1.6

Fecha de publicación: 20 de abril de 2021

- ![nuevo icono](../../assets/new.svg) **Credenciales de autenticación Redis**—Se ha agregado la capacidad de leer las credenciales de autorización de Redis de la propiedad `relationships` durante la fase de implementación.<!--MCLOUD-7694-->

- ![nuevo icono](../../assets/new.svg) **Credenciales de autorización de Elasticsearch**—Se agregó la capacidad de leer las credenciales de autorización de Elasticsearch de la propiedad `relationships` durante la fase de implementación.<!--MCLOUD-7695-->

- ![nuevo icono](../../assets/new.svg) **Servicio de almacenamiento de sesión dedicado**—Se agregó `redis-session` como una segunda opción para el almacenamiento de sesión. Puede usar el servicio `redis-session` para almacenar información de sesión y usar el servicio `redis` para la caché con el fin de proporcionar un mejor rendimiento.<!--MCLOUD-7698-->

- ![nuevo icono](../../assets/new.svg) **Mensajes obsoletos de SPLIT_DB**—Se han agregado mensajes críticos y de advertencia de validador para la opción obsoleta `SPLIT_DB` de Adobe Commerce 2.4.2 y su eliminación en Adobe Commerce 2.5.0.<!--MCLOUD-7806-->

- ![Icono de corrección](../../assets/fix.svg) **Versión de Elasticsearch de las relaciones**—Validador de servicio fijo para recuperar la versión correcta de Elasticsearch de las propiedades de `relationships` en los entornos Cloud Docker e integración.<!--MCLOUD-7572-->

- ![Icono de correcciones](../../assets/fix.svg) **Validación flexible de puertos de Redis**—Ahora Redis puede validar el puerto en una conexión de caché personalizada desde la dirección URL `server`. Por ejemplo, puede agregar el número de puerto a la dirección URL del servidor de la siguiente manera: `server: 'tcp://rfs-store-simple-page-cache:26379'`. Esto ayuda a evitar errores de validación en los que falta la opción `port` o es incorrecta.<!--MCLOUD-7722-->

- ![Icono de corrección](../../assets/fix.svg) **Actualización a Adobe Commerce 2.4.2**—Se ha corregido el problema que requería que los usuarios ejecutaran manualmente `bin/magento setup:upgrade` para que sus sitios estuvieran operativos después de actualizar a Adobe Commerce 2.4.2.<!--MCLOUD-7776-->

## v2002.1.5

Fecha de publicación: 1 de febrero de 2021

- ![nuevo icono](../../assets/new.svg) **Almacenamiento remoto**—Se ha agregado la variable de entorno `REMOTE_STORAGE` para habilitar Proyectos en la nube para el almacenamiento remoto de archivos multimedia mediante un servicio de almacenamiento, como AWS S3. Esta opción de configuración forma parte del paquete ECE-Tools, pero no es compatible con Adobe Commerce en la infraestructura en la nube.<!--MCLOUD-7153-->

- ![nuevo icono](../../assets/new.svg) **Nuevo comando `cloud:config:validate`**—Se agregó el comando `php vendor/bin/ece-tools cloud:config:validate` para validar la configuración de `.magento.env.yaml` antes de insertar cambios en el entorno de nube remoto.<!--MCLOUD-7120-->

- ![nuevo icono](../../assets/new.svg) **Vaciando el opcache**—Se agregó compatibilidad con la opción PHP `opcache.enable_cli` para vaciar el OPcache antes de ejecutar el enlace de implementación. Esta configuración restablece la configuración de la caché para garantizar que se aplican los valores de configuración actuales en cada implementación.<!--MCLOUD-7015-->

- ![nuevo icono](../../assets/new.svg) **Validación de Aurora DB**: se ha actualizado la validación del servicio de base de datos para que sea compatible con la base de datos Aurora.<!--MCLOUD-7269-->

- ![nuevo icono](../../assets/new.svg) **Nueva variable de entorno SCD_NO_PARENT**—Se ha agregado la variable de entorno `SCD_NO_PARENT` (para Adobe Commerce >=2.4.2) para administrar la generación de contenido estático para las temáticas principales.<!--MCLOUD-7284-->

- ![Icono de correcciones](../../assets/fix.svg) **Límites y comandos de memoria**—Se ha corregido un problema por el que los comandos de `php vendor/bin/ece-tools` no funcionaban si el tamaño del archivo de `cloud.log` superaba el límite de memoria PHP. En lugar de leer todo el archivo `cloud.log` en la memoria, ahora solo se lee un subconjunto más pequeño de datos del archivo de registro.<!--MCLOUD-7275--><!--MCLOUD-7400-->

- ![icono de corrección](../../assets/fix.svg) **Conexiones de base de datos personalizadas**—Se ha corregido un problema de configuración de `.magento.env.yaml` en el que no se usaban las conexiones de base de datos personalizadas definidas para `DATABASE_CONFIGURATION`. No se agregaba la configuración de conexión a `app/etc/env.php`.<!--MCLOUD-7426-->

- ![icono de corrección](../../assets/fix.svg) **Registros de errores vacíos**—Se ha corregido un problema que hacía que las implementaciones fallaran si `cloud.error.log` estaba vacío.<!--MCLOUD-7296-->

- ![Icono de corrección](../../assets/fix.svg) **Validación de MariaDB 10.3**—Validación fija de MariaDB 10.3 para Adobe Commerce 2.3.6-p1.<!--MCLOUD-7416-->

- ![icono de corrección](../../assets/fix.svg) **Registro de caché:flush**: se mejoraron las entradas de registro para indicar el comienzo y el final del paso `cache:flush`.<!--MCLOUD-7503-->

## v2002.1.4

Fecha de la versión: 19 de noviembre de 2020

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que provocaba un error de implementación cuando el motor de búsqueda especificado en la variable de entorno `SEARCH_CONFIGURATION` es un valor distinto de `elasticsearch`.<!--MCLOUD-7283-->

## v2002.1.3

Fecha de la versión: 9 de noviembre de 2020

**Actualizaciones de infraestructura**—

- ![nuevo icono](../../assets/new.svg) agregó compatibilidad con ECE-Tools para el directorio de solo lectura `pub/static` cuando el contenido estático está configurado para implementarse en la fase de compilación.<!--MC-37699-->

- ![nuevo icono](../../assets/new.svg) agregó compatibilidad con Elasticsearch 7.9 y Redis 6 por compatibilidad con próximas versiones de Adobe Commerce.<!--MCLOUD-7191-->

- ![icono de corrección](../../assets/fix.svg) Se ha actualizado ECE-Tools `composer.json` para agregar una dependencia necesaria para la herramienta Parches de calidad. Esto corrige una dependencia circular que existía entre los paquetes ECE-Tools y magento-cloud-patch.<!--MCLOUD-6910-->

**Mejoras de validación y registro**—

- ![nuevo icono](../../assets/new.svg) Se agregó la validación del motor de búsqueda para garantizar que `elasticsearch` esté configurado para Adobe Commerce en la infraestructura en la nube 2.4 y posterior. Si la validación falla, la implementación se detiene con un mensaje de error crítico que sugiere correcciones para el problema. Ver [errores críticos, implementación de fase](../dev-tools/error-reference.md#deploy-stage).<!--MCLOUD-6937-->

- ![nuevo icono](../../assets/new.svg) agregó validación de Elasticsearch para comprobar la compatibilidad entre la versión del servicio Elasticsearch y la versión de Adobe Commerce.<!--MCLOUD-7193-->

- ![nuevo icono](../../assets/new.svg) Se ha actualizado el mensaje de error de compatibilidad de Elasticsearch para mostrar las versiones de Elasticsearch compatibles con el módulo Elasticsearch de Adobe Commerce. El mensaje de error ahora proporciona las versiones específicas de Elasticsearch que se deben instalar en la infraestructura de Cloud para que sea compatible con el módulo de Elasticsearch que use su versión de Adobe Commerce. Ver [errores de advertencia, implementación de fase](../dev-tools/error-reference.md#deploy-stage-1).<!--MCLOUD-6698-->

- ![nuevo icono](../../assets/new.svg) agregó errores de advertencia `2026` y `2027` para la configuración de variable de entorno `MAGE_MODE` no válida. El único valor válido es `production`. Antes de esta corrección, `MAGE_MODE` se podía establecer en `developer` sin errores de implementación, solo para provocar errores más adelante al intentar escribir en archivos de solo lectura. Ver [errores de advertencia](../dev-tools/error-reference.md#warning-errors).<!--MCLOUD-6708-->

- ![Icono de corrección](../../assets/fix.svg) Se ha corregido la validación de los servicios Redis, RabbitMQ y MySQL para garantizar que estas versiones sean compatibles con la versión de Adobe Commerce. Las versiones válidas de estos servicios ahora se escriben en `cloud.log`.<!--MCLOUD-7098-->

- ![icono de corrección](../../assets/fix.svg) Actualizó `cloud.log` para incluir el límite de solicitudes simultáneas para enviar solicitudes durante el calentamiento de la caché. Este valor está configurado en la variable [WARM_UP_CONCURRENCY](../environment/variables-post-deploy.md#warm_up_concurrency) posterior a la implementación.<!--MCLOUD-5563-->

**Actualizaciones de comandos CLI**—

- ![nuevo icono](../../assets/new.svg) agregó comandos CLI (`cloud:config:create` y `cloud:config:update`) para crear y actualizar el archivo `.magento.env.yaml` con una configuración que puede incluir una o más variables de compilación, implementación y posteriores a la implementación. Ver [Crear archivo de configuración desde CLI](../environment/configure-env-yaml.md#create-configuration-file-from-cli).<!--MCLOUD-7072-->

**Actualizaciones de variables de entorno**—

- ![nuevo icono](../../assets/new.svg) agregó la variable de compilación [SKIP_COMPOSER_DUMP_AUTOLOAD](../environment/variables-build.md#skip_composer_dump_autoload). Al establecer la variable en `true`, se impide que la aplicación ejecute el comando `composer dump-autoload` durante una instalación de Cloud Docker para Commerce. La variable solo es relevante para los contenedores de Cloud Docker para Commerce con sistemas de archivos editables (creados para pruebas y desarrollo con `./vendor/bin/ece-docker build:compose --with-test`). Con estas instalaciones, omitir el comando `composer dump-autoload` evita errores cuando se ejecutan otros comandos que intentan obtener acceso a archivos desde un directorio `generated` eliminado.<!--MCLOUD-6939-->

## v2002.1.2

Fecha de lanzamiento: 5 de agosto de 2020

**Mejoras de validación y registro**—

- ![nuevo icono](../../assets/new.svg) agregó el archivo `schema.error.yaml` que incluye todas las notificaciones de error y advertencia que pueden ocurrir durante el proceso de generación, implementación y posterior a la implementación, además de sugerencias para resolver los errores. La información contenida en este archivo también está disponible en la _Guía de Cloud para Commerce_. Consulte [Referencia de mensaje de error para ece-tools](../dev-tools/error-reference.md).<!--MCLOUD-5878-->

- ![nuevo icono](../../assets/new.svg) cambió las entradas del registro de errores de Cloud (`/var/log/cloud.error.log`) al formato JSON para que el registro sea más fácil de analizar mediante programación.<!--MCLOUD-5879-->

- ![nuevo icono](../../assets/new.svg) agregó comprobaciones de errores adicionales para generar, implementar y posimplementar el procesamiento, y mejoró las comprobaciones existentes:

   - Código de error 2026: error al restaurar algunos datos generados durante la fase de compilación en los directorios montados

   - Código de error 3004: no se pueden crear archivos de copia de seguridad

   - Código de error 102: se agregaron comprobaciones adicionales para detectar problemas que se producen cuando el archivo `env.php` no se puede escribir en <!--MCLOUD-6221-->

- ![nuevo icono](../../assets/new.svg) agregó la variable de entorno **QUALITY_PATCHES** para especificar uno o más parches de calidad para aplicar durante el proceso de implementación. Ver [variables de compilación](../environment/variables-build.md#quality_patches).<!--MCLOUD-6375-->

## v2002.1.1

Fecha de publicación: 25 de junio de 2020

- ![nuevo icono](../../assets/new.svg) **actualizaciones de infraestructura**—

   - ![nuevo icono](../../assets/new.svg) **Mejoras en el registro**: se ha mejorado la capacidad de seguimiento de registros al asignar códigos de salida a errores de implementación críticos y al exponer los códigos de salida en las notificaciones de mensajes de error y los eventos de registro. Consulte [Referencia de mensaje de error para ece-tools](../dev-tools/error-reference.md).<!-- MCLOUD-5637, 5531-->

   - ![nuevo icono](../../assets/new.svg) mejoró el proceso de volcados de base de datos (`vendor/bin/ece-tools db-dump`) y actualizó los mensajes de registro para aclarar que la operación de volcado de base de datos cambia la aplicación al modo de mantenimiento, detiene los procesos de cola de consumidores y deshabilita los trabajos cron antes de que comience el volcado.<!--MCLOUD-5324, MCLOUD-2062-->

   - ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema para garantizar que la dirección URL del proyecto se actualice correctamente al implementarla en los entornos de ensayo y producción. Ahora, `ece-tools` usa la dirección URL de la ruta con el atributo `primary:true` establecido en la configuración de ruta del proyecto. Ver [Implementar variables](../environment/variables-deploy.md#update_urls).<!--MCLOUD-5883-->

   - ![icono de corrección](../../assets/fix.svg) Actualizó el flujo de trabajo del escenario de compilación `generate.xml` para aplicar parches. Los parches se deben aplicar antes para actualizar Adobe Commerce y solucionar los problemas que puedan provocar errores en los pasos de `di:compile` y `module:refresh`.<!--MCLOUD-5941-->

   - ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema en el proceso de instalación que devolvía incorrectamente el error `Crypt key missing`. El valor `crypt/key` se genera automáticamente durante la instalación.<!--MCLOUD-6120-->

- ![nuevo icono](../../assets/new.svg) **Actualizaciones de servicio**—

   - ![nuevo icono](../../assets/new.svg) agregó compatibilidad con PHP 7.4 y MariaDB 10.4.<!--MAGECLOUD-2957, MCLOUD-4144-->

- ![nuevo icono](../../assets/new.svg) **Actualizaciones de variables de entorno**—

   - ![nuevo icono](../../assets/new.svg) agregó la variable **SCD_USE_BALER** para habilitar el módulo Baler para el agrupamiento de JavaScript durante el proceso de compilación de Adobe Commerce en la nube. Ver la descripción de la variable en [variables de compilación](../environment/variables-build.md#scd_use_baler).<!-- MCLOUD-3456, MCLOUD-3457-->

   - ![nuevo icono](../../assets/new.svg) agregó la variable de entorno **REDIS_BACKEND** para configurar el modelo de back-end Redis para la caché Redis para Adobe Commerce 2.3.5 o posterior. Consulte la descripción de la variable en [implementar variables](../environment/variables-deploy.md#redis_backend).<!--MCLOUD-5721, MCLOUD-5865-->

- ![nuevo icono](../../assets/new.svg) **actualizaciones de comandos CLI**—

   - ![nuevo icono](../../assets/new.svg) Se han actualizado los siguientes comandos de CLI con una opción para un registro más detallado:

      - `app:config:dump`
      - `app:config:import`
      - `module:enable`

     El nivel de registro de cada llamada está determinado por la configuración de la variable [`VERBOSE_COMMANDS`](../environment/variables-build.md#verbose_commands) en el archivo `.magento.env.yaml`.<!--MCLOUD-3503-->

- ![nuevo icono](../../assets/new.svg) **Mejoras de validación**—

   - ![nuevo icono](../../assets/new.svg) **comprobaciones de compatibilidad de Elasticsearch 7.x**: se ha actualizado la validación de Elasticsearch para las comprobaciones de compatibilidad del software Elasticsearch 7.x.<!--MCLOUD-5542-->

   - ![nuevo icono](../../assets/new.svg) **Comprobaciones de validación de EOL y versión de servicio actualizada**: validación actualizada para comprobar las versiones de servicio instaladas con los requisitos de Adobe Commerce 2.4.<!--MCLOUD-6144-->

   - ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema de validación para que el siguiente mensaje de advertencia posterior a la implementación se muestre únicamente si falta la configuración del vínculo `post-deploy` en el archivo `.magento.app.yaml`:

     ```text
     Your application does not have the "post_deploy" hook enabled.
     ```

     <!--MCLOUD-4077-->

   - ![nuevo icono](../../assets/new.svg) **Validación agregada para las dependencias de Zend Framework**—Validación de dependencia del compositor agregada para Zend Framework que ha migrado al proyecto Laminas. Si faltan las dependencias necesarias, aparece el siguiente mensaje de error durante el proceso de generación.

     ```text
     Required configuration is missing from the autoload section of the composer.json file.
     Add ("Laminas\Mvc\Controller\Zend\": "setupsrc/ Zend/Mvc/Controller/") to
     the `autoload -> psr-4` section. Then, re-run the "composer update" command locally, and
     commit the updated composer.json and composer.lock files.
     ```

     Ver [Verificar dependencias de Zend Framework](../development/commerce-version.md#verify-zend-framework-composer-dependencies).<!--MCLOUD-4094-->

   - ![nuevo icono](../../assets/new.svg) **Validación agregada para `env.php` archivo y datos**—Comprobaciones agregadas para el archivo y los datos `env.php` durante el proceso de instalación y actualización.<!--MCLOUD-5991-->

      - Si falta el archivo `env.php` en la instalación y el valor `crypt/key` no se especifica en el archivo `.magento.app.yaml`, la implementación falla con la siguiente notificación:

        ```text
        The crypt/key key value does not exist in the ./app/etc/env.php file or the CRYPT_KEY cloud environment variable``Missing crypt key for upgrading Magento`.
        ```

      - Si la instalación no incluye el archivo `env.php`, o si la configuración contiene sólo un tipo de caché, el comando `cron:enable` se ejecuta durante el proceso de actualización para restaurar el archivo con todos los `cache_types`. Se agrega la siguiente notificación al registro:

        ```text
        Magento state indicated as installed but configuration file app/etc/env.php was empty or did not exist.
        Required data will be restored from environment configurations and from the .magento.env.yaml file.
        ```

## v2002.1.0

Fecha de publicación: 6 de febrero de 2020

- ![nuevo icono](../../assets/new.svg) **actualizaciones de infraestructura**—

   - ![nuevo icono](../../assets/new.svg) **Se agregó un paquete independiente para Cloud Docker para Commerce**—Se desacopló el paquete Docker del paquete `ece-tools` para mantener la calidad del código y proporcionar versiones independientes. Las actualizaciones y correcciones relacionadas con `ece-tools` se administran desde el repositorio de [magento-cloud-docker](https://github.com/magento/magento-cloud-docker) de GitHub.<!--MAGECLOUD-2927-->

   - ![nuevo icono](../../assets/new.svg) **Funciones de parches actualizadas**—Se ha movido la funcionalidad de parches del paquete ECE-Tools a un paquete [magento-cloud-patch](https://github.com/magento/magento-cloud-patches) independiente. Durante la implementación, `ece-tools` usa el nuevo paquete para aplicar parches. Ver [Notas de la versión de parches de nube](cloud-patches.md).<!--MAGECLOUD-4567-->

   - ![nuevo icono](../../assets/new.svg) **Dependencias actualizadas del compositor**—Se ha actualizado el archivo `composer.json` para Adobe Commerce en la infraestructura en la nube con una dependencia para el paquete `magento/magento-cloud-docker`. Ahora, `ece-tools` incluye dependencias para todos los paquetes en [`Cloud Tools Suite for Commerce`](cloud-tools-suite.md). Estos paquetes se instalan y actualizan automáticamente al instalar o actualizar `ece-tools`.

- ![nuevo icono](../../assets/new.svg) **Compatibilidad con implementaciones basadas en escenarios**—<!--MAGECLOUD-4101-->

   - ![nuevo icono](../../assets/new.svg) Ahora puede personalizar los procesos de generación, implementación y posterior implementación mediante archivos de configuración XML para anular o personalizar la configuración predeterminada.

   - ![nuevo icono](../../assets/new.svg) **Se ha cambiado la configuración de `hooks` en`.magento.app.yaml`**. Hemos actualizado el formato de configuración de `hooks` para admitir implementaciones basadas en escenarios. El formato heredado de versiones anteriores de ECE-Tools 2002.0.x todavía es compatible. Sin embargo, debe actualizar al nuevo formato para utilizar la función de implementación basada en escenarios. Consulte [Implementaciones basadas en escenarios](../deploy/scenario-based.md#add-scenarios-using-build-and-deploy-hooks).

>[!NOTE]
>
>Antes de actualizar a ECE-Tools versión 2002.1.0, revise [hacia atrás   cambios incompatibles](backward-incompatible-changes.md) para obtener información sobre los cambios que podrían requerir que   actualice la configuración o los procesos del proyecto de Adobe Commerce en la nube.

- ![nuevo icono](../../assets/new.svg) **Actualizaciones de servicio**—

   - ![nuevo icono](../../assets/new.svg) agregó compatibilidad con PHP 7.3.<!--MAGECLOUD-4022-->

   - ![nuevo icono](../../assets/new.svg) agregó compatibilidad con RabbitMQ 3.8.<!--MAGECLOUD-4674-->

   - ![nuevo icono](../../assets/new.svg) Se agregó la validación para comprobar las versiones del servicio instalado respecto a la fecha límite para cada servicio. Ahora, los clientes recibirán una notificación si la versión de un servicio se encuentra dentro de los tres meses posteriores a la fecha límite y una advertencia si la fecha límite se encuentra en el pasado.<!--MAGECLOUD-4076-->

   - ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema de configuración de Elasticsearch para garantizar que la configuración correcta de Elasticsearch esté establecida en todos los entornos.<!--MAGECLOUD-4474-->

>[!NOTE]
>
>Consulte [Versiones de servicio](../services/services-yaml.md#service-versions) para obtener una lista de los servicios que se usan en Adobe Commerce en la infraestructura en la nube y su compatibilidad de versión con la plantilla en la nube.

- ![nuevo icono](../../assets/new.svg) **Actualizaciones de variables de entorno**—

   - ![nuevo icono](../../assets/new.svg) ha ampliado la funcionalidad de la variable de entorno `WARM_UP_PAGES` para admitir la precarga de caché en páginas de producto específicas. Vea la definición expandida en el tema [variables posteriores a la implementación](../environment/variables-post-deploy.md#warm_up_pages).<!--MAGECLOUD-4444-->

   - ![nuevo icono](../../assets/new.svg) agregó la variable de entorno `ERROR_REPORT_DIR_NESTING_LEVEL` para simplificar la administración de datos del informe de errores en el directorio `<magento_root>/var/report/`. Consulte la descripción de la variable en el tema [variables de compilación](../environment/variables-build.md#error_report_dir_nesting_level).

   - ![icono de corrección](../../assets/fix.svg) eliminó las variables de entorno `SCD_EXCLUDE_THEMES`, `STATIC_CONTENT_THREADS`, `DO_DEPLOY_STATIC_CONTENT` y `STATIC_CONTENT_SYMLINK`. Ver [cambios incompatibles con versiones anteriores](backward-incompatible-changes.md#environment-configuration-changes).<!--MAGECLOUD-4407, MAGECLOUD-3873-->

   - ![Icono de corrección](../../assets/fix.svg) Se ha corregido un problema en el proceso de configuración de Elastic Suite para que la configuración predeterminada se sobrescriba como se esperaba al configurar la variable de implementación `ELASTICSUITE_CONFIGURATION` sin la opción `_merge`.<!--MAGECLOUD-4388-->

- ![nuevo icono](../../assets/new.svg) **actualizaciones de comandos CLI**—

   - ![nuevo icono](../../assets/new.svg) **Nuevo comando cron**: ahora puede administrar manualmente el procesamiento cron en su entorno de Adobe Commerce en la nube mediante los comandos `cron:disable` y `cron:enable`. Utilice el comando disable para todos los procesos cron activos y deshabilitar todos los trabajos cron. Utilice el comando enable para volver a habilitar los trabajos cron cuando esté listo. Consulte [Deshabilitar trabajos cron](../application/crons-property.md#disable-cron-jobs).

   - ![nuevo icono](../../assets/new.svg) **Informes de errores mejorados**—Se ha agregado un mejor registro para los errores de comandos de CLI que se producen durante el procesamiento de ECE-Tools.<!--MAGECLOUD-4849-->

   - ![nuevo icono](../../assets/new.svg) **Quitar comandos de compilación obsoletos**— Se eliminaron los siguientes comandos de compilación: `m2-ece-build`, `m2-ece-deploy`, `m2-ece-scd-dump` y se cambió el nombre de `ece-tools docker` comandos a `ece-docker`. Ver [cambios incompatibles con versiones anteriores](backward-incompatible-changes.md)<!--MAGECLOUD-4392-->

- ![nuevo icono](../../assets/new.svg) eliminó el archivo `build_options.ini` obsoleto y agregó validación para fallar en la compilación si el archivo existe. Utilice el archivo [.magento.env.yaml](../environment/configure-env-yaml.md) para configurar las opciones de generación.

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que ocasionaba que fallara el proceso de compilación cuando el archivo `config.php` estaba vacío.<!--MAGECLOUD-4127-->

## 2002.0.23

Fecha de publicación: 27 de febrero de 2020

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema de compatibilidad con `ece-tools` versiones 2002.0.x que impedía que la generación de contenido estático bajo demanda se completara correctamente en el modo de producción.

## Versiones anteriores

Consulte el [archivo de notas de la versión](cloud-release-archive.md) para la versión 2002.0.22 y anteriores.
