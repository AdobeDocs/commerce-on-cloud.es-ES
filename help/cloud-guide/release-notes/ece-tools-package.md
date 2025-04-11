---
title: Notas de la versión de ECE-Tools
description: Vea una lista de las mejoras más recientes del paquete ECE-Tools.
recommendations: noDisplay, catalog
last-substantial-update: 2024-04-09T00:00:00Z
exl-id: 3cbfe698-d75d-4a16-877a-52c214595344
source-git-commit: 933e0c1b8bfbafeb6a477ec7bba7dcf7667dc6ec
workflow-type: tm+mt
source-wordcount: '3092'
ht-degree: 0%

---

# Notas de la versión de ECE-Tools

El paquete [ece-tools](https://github.com/magento/ece-tools) es un conjunto de scripts y herramientas diseñadas para administrar e implementar proyectos en la nube. En estas notas de la versión se describen las mejoras más recientes realizadas en este paquete, que forma parte de [Cloud Tools Suite for Commerce](cloud-tools-suite.md).

>[!NOTE]
>
>Consulte [Actualizar ECE-Tools](../dev-tools/update-package.md) para obtener información sobre cómo actualizar a la última versión del paquete `ece-tools`.

El paquete `ece-tools` utiliza la siguiente secuencia de versiones de la versión: `200<major>.<minor>.<patch>`

Las notas de la versión incluyen:

- ![Nuevas funciones de Nuevo de iconos](../../assets/new.svg)
- ![Icono Fix](../../assets/fix.svg) Correcciones y mejoras

<!--Add release notes below-->

## v2002.2.3 {#latest}

Fecha de publicación: 9 de abril de 2025

- ![Icono de correcciones](../../assets/fix.svg) **Corrección de Valkey** Se ha corregido un problema con la configuración personalizada de Valkey.<!-- MCLOUD-13569	 - -->
- ![Icono de correcciones](../../assets/fix.svg) **Validador de correcciones**: validador fijo para RabbitMQ 4.0.<!-- MCLOUD-13560	 - -->

## v2002.2.2

Fecha de publicación: 7 de abril de 2025

## v2002.2.2

Fecha de publicación: 7 de abril de 2025

- ![nuevo icono](../../assets/new.svg) **Valkey**—Se agregó compatibilidad con un nuevo servicio (Valkey), que reemplaza a Redis.&lt;!— MCLOUD-13455 —>
- ![Icono de corrección](../../assets/fix.svg) **Opensearch2 para 2.4.4/2.4.5**—Se ha agregado compatibilidad con `opensearch2` en las versiones de Adobe Commerce 2.4.4/2.4.5. &lt;!— MCLOUD-13493 —>

## v2002.2.1

Fecha de publicación: 6 de febrero de 2024

- ![nuevo icono](../../assets/new.svg) **PHP 8.4**—Se agregó compatibilidad con PHP 8.4.<!-- MCLOUD-13145     - -->
- ![Icono de corrección](../../assets/fix.svg) **Validador para Opensearch**: se ha corregido el validador que generaba un mensaje engañoso acerca de la versión incorrecta del servicio.&lt;!— MCLOUD-13184 —>


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

- ![nuevo icono](../../assets/new.svg) **PHP** — Se agregó soporte para PHP 8.3.
- ![](../../assets/fix.svg) **icono de corrección Validador** - Validador de EOL actualizado.

## v2002.1.17

Fecha de la versión: 16 de enero de 2024

- ![](../../assets/fix.svg) **icono de reparación Validador para Elasticsearch y OpenSearch**: se corrigió el validador que producía un mensaje engañoso para instalar un servicio de búsqueda cuando LiveSearch está habilitado.<!-- MCLOUD-10167 -->
- ![icono de corrección](../../assets/fix.svg) **Advertencia de implementación**: se ha corregido un problema que provocaba advertencias de implementación sobre carpetas que no estaban vacías.<!-- MCLOUD-8958 -->

## v2002.1.16

Fecha de la versión: 16 de octubre de 2023

- ![nuevo icono](../../assets/new.svg) **ENABLE_WEBHOOKS variable de entorno global**—Se agregó la variable global [ENABLE_WEBHOOKS](../environment/variables-global.md#enable_webhooks) para usarla con los webhooks de Commerce para conectarse a un extremo externo, como la acción de tiempo de ejecución de App Builder o un sistema de administración de inventario de terceros.

## v2002.1.15

Fecha de la versión: 31 de julio de 2023

- ![Icono](../../assets/fix.svg) **de corrección Error códigos**: se han actualizado los esquema de código de error y el generador de documento de código de error.
- ![Icono](../../assets/fix.svg) **de corrección Validador para el modelo** personalizado de Redis: se ha actualizado el validador para los modelos de back-end de Redis personalizados. [Consulte el ejemplo de configuración](../environment/variables-deploy.md#cache_configuration) de caché.
- ![](../../assets/fix.svg) **icono de corrección Validador para RabbitMQ:** compatibilidad añadida para RabbitMQ 3.11
- ![Icono Fix](../../assets/fix.svg) **Se ha corregido el vincular** incorrecto: se ha corregido el vincular incorrecto de la documentación de incorporación en la plantilla de bienvenida correo electrónico.

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

- ![nuevo icono](../../assets/new.svg) **Habilitar`synchronous_replication`**—ECE-Tools establece `synchronous_replication=>true` en el archivo `app/etc/env.php` cuando `MYSQL_USE_SLAVE_CONNECTION` está habilitado. Esta configuración afecta únicamente a Commerce 2.4.6+. Consulte la descripción variable `MYSQL_USE_SLAVE_CONNECTION` en Implementar [variables](../environment/variables-deploy.md#mysql_use_slave_connection).<!-- MCLOUD-9142 -->
- ![nuevo icono](../../assets/new.svg) **OpenSearch**: se agregó funcionalidad para configurar y configurar el `opensearch` motor para la próxima versión 2.4.6 de Adobe Systems Commerce. Consulte [Configuración del servicio](../services/opensearch.md) OpenSearch.<!-- MCLOUD-9236 -->

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

**Actualizaciones** de configuración—

- ![Icono](../../assets/new.svg) nuevo Se ha añadido compatibilidad con Composer 2.0.<!--MCLOUD-8003-->

- ![Icono Fix](../../assets/fix.svg) **Se han actualizado los requisitos del compositor para`symphony/console`**—Se han actualizado los requisitos de la versión ECE-Herramientas `composer.json` para el `symphony/console` paquete para corregir un problema que provocaba que los `di:compile` comandos fallaran con el siguiente error: `Incompatible argument type: Required type: int. Actual type: string`<!--MC-42919-->

- ![Icono Fix](../../assets/fix.svg) Se han actualizado las comprobaciones de software al final de su vida útil (`eol.yaml`) para incluir Elasticsearch 7.9.x.<!--MCLOUD-7938-->

## v2002.1.6

Fecha de publicación: 20 de abril de 2021

- ![nuevo icono](../../assets/new.svg) **Credenciales** de autenticación de Redis: se ha agregado la capacidad de leer las credenciales de autorización de Redis desde la `relationships` Propiedad durante la fase implementar.<!--MCLOUD-7694-->

- ![nuevo icono](../../assets/new.svg) **Elasticsearch credenciales** de autorización: se ha añadido la capacidad de leer Elasticsearch credenciales de autorización desde la `relationships` Propiedad durante la fase de implementar.<!--MCLOUD-7695-->

- ![Icono](../../assets/new.svg) **nuevo Servicio** de almacenamiento de sesión dedicado: se ha agregado `redis-session` como segunda opción para el almacenamiento de sesión. Puede usar el servicio `redis-session` para almacenar información de sesión y usar el servicio `redis` para la caché con el fin de proporcionar un mejor rendimiento.<!--MCLOUD-7698-->

- ![nuevo icono](../../assets/new.svg) **Mensajes obsoletos de SPLIT_DB**—Se han agregado mensajes críticos y de advertencia de validador para la opción obsoleta `SPLIT_DB` de Adobe Commerce 2.4.2 y su eliminación en Adobe Commerce 2.5.0.<!--MCLOUD-7806-->

- ![Icono de corrección](../../assets/fix.svg) **Versión de Elasticsearch de las relaciones**—Validador de servicio fijo para recuperar la versión correcta de Elasticsearch de las propiedades de `relationships` en los entornos Cloud Docker e integración.<!--MCLOUD-7572-->

- ![Icono de correcciones](../../assets/fix.svg) **Validación flexible de puertos de Redis**—Ahora Redis puede validar el puerto en una conexión de caché personalizada desde la dirección URL `server`. Por ejemplo, puede agregar el número de puerto a la dirección URL del servidor de la siguiente manera: `server: 'tcp://rfs-store-simple-page-cache:26379'`. Esto ayuda a evitar errores de validación en los que falta la opción `port` o es incorrecta.<!--MCLOUD-7722-->

- ![Icono](../../assets/fix.svg) **Reparar actualización a Adobe Systems Commerce 2.4.2**: se ha corregido el problema que los usuarios debían ejecutar `bin/magento setup:upgrade` manualmente para que sus sitios estuvieran operativos después de actualizar a Adobe Systems Commerce 2.4.2.<!--MCLOUD-7776-->

## v2002.1.5

Fecha de publicación: 1 de febrero de 2021

- ![nuevo icono](../../assets/new.svg) **almacenamiento** remoto: se agregó el variable entorno para habilitar Cloud `REMOTE_STORAGE` Projects para el almacenamiento remoto de archivos de medios mediante un servicio de almacenamiento, como AWS S3. Esta opción de configuración forma parte del paquete ECE-Herramientas, pero no es compatible con Adobe Systems Commerce on infraestructura en la nube.<!--MCLOUD-7153-->

- ![nuevo icono](../../assets/new.svg) **Nuevo `cloud:config:validate` comando**: se agregó un comando `php vendor/bin/ece-tools cloud:config:validate` para validar la `.magento.env.yaml` configuración antes de enviar cambios al entorno remoto de la nube.<!--MCLOUD-7120-->

- ![nuevo icono](../../assets/new.svg) **Vaciando el opcache**—Se agregó compatibilidad con la opción PHP `opcache.enable_cli` para vaciar el OPcache antes de ejecutar el enlace de implementación. Esta configuración restablece la configuración de la caché para garantizar que se aplican los valores de configuración actuales en cada implementación.<!--MCLOUD-7015-->

- ![nuevo icono](../../assets/new.svg) **Validación de Aurora DB**: se ha actualizado la validación del servicio de base de datos para que sea compatible con la base de datos Aurora.<!--MCLOUD-7269-->

- ![nuevo icono](../../assets/new.svg) **Nueva variable de entorno SCD_NO_PARENT**—Se ha agregado la variable de entorno `SCD_NO_PARENT` (para Adobe Commerce >=2.4.2) para administrar la generación de contenido estático para las temáticas principales.<!--MCLOUD-7284-->

- ![Icono de correcciones](../../assets/fix.svg) **Límites y comandos de memoria**—Se ha corregido un problema por el que los comandos de `php vendor/bin/ece-tools` no funcionaban si el tamaño del archivo de `cloud.log` superaba el límite de memoria PHP. En lugar de leer todo el archivo `cloud.log` en la memoria, ahora solo se lee un subconjunto más pequeño de datos del archivo de registro.<!--MCLOUD-7275--><!--MCLOUD-7400-->

- ![icono de corrección](../../assets/fix.svg) **Conexiones de base de datos personalizadas**—Se ha corregido un problema de configuración de `.magento.env.yaml` en el que no se usaban las conexiones de base de datos personalizadas definidas para `DATABASE_CONFIGURATION`. No se agregaba la configuración de conexión a `app/etc/env.php`.<!--MCLOUD-7426-->

- ![icono de corrección](../../assets/fix.svg) **Registros de errores vacíos**—Se ha corregido un problema que hacía que las implementaciones fallaran si `cloud.error.log` estaba vacío.<!--MCLOUD-7296-->

- ![Icono de corrección](../../assets/fix.svg) **Validación de MariaDB 10.3**—Validación fija de MariaDB 10.3 para Adobe Commerce 2.3.6-p1.<!--MCLOUD-7416-->

- ![icono de corrección](../../assets/fix.svg) **Caché:registro de vaciado**—Entradas de registro mejoradas para indicar el comienzo y el fin del paso `cache:flush`.<!--MCLOUD-7503-->

## v2002.1.4

Fecha de la versión: 19 de noviembre de 2020

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que provocaba un error de implementación cuando el motor de búsqueda especificado en la variable de entorno `SEARCH_CONFIGURATION` es un valor distinto de `elasticsearch`.<!--MCLOUD-7283-->

## v2002.1.3

Fecha de la versión: 9 de noviembre de 2020

**Actualizaciones** de infraestructura:

- ![Icono](../../assets/new.svg) nuevo Se ha agregado compatibilidad Herramientas ECE para el directorio de solo `pub/static` lectura cuando el contenido estático se establece en implementar en el fase versión.<!--MC-37699-->

- ![nuevo icono](../../assets/new.svg) agregó compatibilidad con Elasticsearch 7.9 y Redis 6 por compatibilidad con próximas versiones de Adobe Commerce.<!--MCLOUD-7191-->

- ![icono de corrección](../../assets/fix.svg) Se ha actualizado ECE-Tools `composer.json` para agregar una dependencia necesaria para la herramienta Parches de calidad. Esto corrige una dependencia circular que existía entre los paquetes ECE-Tools y magento-cloud-patch.<!--MCLOUD-6910-->

**Mejoras de validación y registro**—

- ![nuevo icono](../../assets/new.svg) Se agregó la validación del motor de búsqueda para garantizar que `elasticsearch` esté configurado para Adobe Commerce en la infraestructura en la nube 2.4 y posterior. Si la validación falla, la implementación se detiene con un mensaje de error crítico que sugiere correcciones para el problema. Ver [errores críticos, implementación de fase](../dev-tools/error-reference.md#deploy-stage).<!--MCLOUD-6937-->

- ![nuevo icono](../../assets/new.svg) agregó validación de Elasticsearch para comprobar la compatibilidad entre la versión del servicio Elasticsearch y la versión de Adobe Commerce.<!--MCLOUD-7193-->

- ![nuevo icono](../../assets/new.svg) Se ha actualizado el mensaje de error de compatibilidad de Elasticsearch para mostrar las versiones de Elasticsearch compatibles con el módulo Elasticsearch de Adobe Commerce. El mensaje de error ahora proporciona las versiones específicas de Elasticsearch que se deben instalar en la infraestructura de Cloud para que sea compatible con el módulo de Elasticsearch que use su versión de Adobe Commerce. Ver [errores de advertencia, implementación de fase](../dev-tools/error-reference.md#deploy-stage-1).<!--MCLOUD-6698-->

- ![nuevo icono](../../assets/new.svg) agregó errores de advertencia `2026` y `2027` para la configuración de variable de entorno `MAGE_MODE` no válida. El único valor válido es `production`. Antes de esta corrección, `MAGE_MODE` se podía establecer en `developer` sin errores de implementación, solo para provocar errores más adelante al intentar escribir en archivos de solo lectura. Ver [errores de advertencia](../dev-tools/error-reference.md#warning-errors).<!--MCLOUD-6708-->

- ![Icono de corrección](../../assets/fix.svg) Se ha corregido la validación de los servicios Redis, RabbitMQ y MySQL para garantizar que estas versiones sean compatibles con la versión de Adobe Commerce. Las versiones válidas de estos servicios ahora se escriben en `cloud.log`.<!--MCLOUD-7098-->

- ![Icono](../../assets/fix.svg) de corrección Se ha actualizado el `cloud.log` para incluir el límite de solicitudes simultáneas para enviar solicitudes durante el calentamiento de la caché. Este valor se configura en el [variable entrada-implementar WARM_UP_CONCURRENCY](../environment/variables-post-deploy.md#warm_up_concurrency) .<!--MCLOUD-5563-->

**Actualizaciones** de comandos de CLI:

- ![Icono](../../assets/new.svg) nuevo Se han agregado comandos CLI (`cloud:config:create` y `cloud:config:update`) para crear y actualizar el `.magento.env.yaml` archivo con una configuración que puede incluir una o más variables versión, implementar y entrada-implementar. Consulte [Crear archivo de configuración de CLI.](../environment/configure-env-yaml.md#create-configuration-file-from-cli)<!--MCLOUD-7072-->

**Actualizaciones de variable del** entorno:

- ![Icono](../../assets/new.svg) nuevo Se agregó el [SKIP_COMPOSER_DUMP_AUTOLOAD](../environment/variables-build.md#skip_composer_dump_autoload) versión variable. Configurar el variable para `true` que impida que el aplicación ejecute el `composer dump-autoload` comando durante una instalación de Cloud Docker for Commerce. El variable solo es relevante para los contenedores de Cloud Docker for Commerce con sistemas de archivos grabables (creados para pruebas y desarrollo mediante `./vendor/bin/ece-docker build:compose --with-test`). Con tales instalaciones, omitir el comando evita errores al ejecutar otros comandos que intentan acceder a archivos `composer dump-autoload` desde un directorio eliminado `generated` .<!--MCLOUD-6939-->

## v2002.1.2

Fecha de lanzamiento: 5 de agosto de 2020

**Mejoras de validación y registro:**

- ![Icono](../../assets/new.svg) nuevo Se agregó el `schema.error.yaml` archivo que incluye todas las notificaciones de error y advertencia que pueden ocurrir durante el proceso de versión, implementar y implementar entrada, junto con sugerencias para resolver los errores. La información de este archivo también está disponible en la Guía de _Cloud para comercio_. Consulte [Error referencia del mensaje para ece-tools](../dev-tools/error-reference.md).<!--MCLOUD-5878-->

- ![Icono](../../assets/new.svg) nuevo Se han cambiado las entradas del registro de errores de nube (`/var/log/cloud.error.log`) a las formato JSON para que el registro sea más fácil de analizar mediante programación.<!--MCLOUD-5879-->

- ![Icono](../../assets/new.svg) nuevo Se han añadido comprobaciones de errores adicionales al procesamiento de versión, implementar y entrada implementar y se han mejorado las comprobaciones existentes:

   - Código Error 2026: error al restaurar algunos datos generados durante la fase de versión en los directorios montados

   - Error código 3004: no se pueden crear archivos de copia de seguridad

   - Código de Error 102: se agregaron comprobaciones adicionales de problemas que ocurren cuando el `env.php` archivo no se puede escribir <!--MCLOUD-6221-->

- ![Icono](../../assets/new.svg) nuevo Se ha agregado el **variable entorno QUALITY_PATCHES** especificar uno o más parches de calidad para aplicar durante el proceso implementación. Consulte [Variables](../environment/variables-build.md#quality_patches) de generación.<!--MCLOUD-6375-->

## v2002.1.1

Fecha de publicación: 25 de junio de 2020

- ![nuevo icono](../../assets/new.svg) **actualizaciones de infraestructura**—

   - ![nuevo icono](../../assets/new.svg) **Mejoras en el registro**: se ha mejorado la capacidad de seguimiento de registros al asignar códigos de salida a errores de implementación críticos y al exponer los códigos de salida en las notificaciones de mensajes de error y los eventos de registro. Consulte [Referencia de mensaje de error para ece-tools](../dev-tools/error-reference.md).<!-- MCLOUD-5637, 5531-->

   - ![nuevo icono](../../assets/new.svg) mejoró el proceso de volcados de base de datos (`vendor/bin/ece-tools db-dump`) y actualizó los mensajes de registro para aclarar que la operación de volcado de base de datos cambia la aplicación al modo de mantenimiento, detiene los procesos de cola de consumidores y deshabilita los trabajos cron antes de que comience el volcado.<!--MCLOUD-5324, MCLOUD-2062-->

   - ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema para garantizar que la dirección URL del proyecto se actualice correctamente al implementarla en los entornos de ensayo y producción. Ahora, `ece-tools` usa la dirección URL de la ruta con el atributo `primary:true` establecido en la configuración de ruta del proyecto. Consulte [Implementar variables](../environment/variables-deploy.md#update_urls).<!--MCLOUD-5883-->

   - ![Icono Fix](../../assets/fix.svg) Se ha actualizado el `generate.xml` flujo de trabajo del escenario versión para aplicar parches. Los parches deben aplicarse antes para actualizar Adobe Systems Commerce y solucionar cualquier problema que pueda causar el error y `di:compile` `module:refresh` los pasos.<!--MCLOUD-5941-->

   - ![Icono Fix](../../assets/fix.svg) Se ha corregido un problema en el proceso de instalación que devuelve incorrectamente el `Crypt key missing` error. El `crypt/key` valor se genera automáticamente durante la instalación.<!--MCLOUD-6120-->

- ![nuevo icono](../../assets/new.svg) **Actualizaciones** de servicio:

   - ![Nuevo icono](../../assets/new.svg) Se agregó soporte para PHP 7.4 y MariaDB 10.4.<!--MAGECLOUD-2957, MCLOUD-4144-->

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

     Consulte [Verificar las dependencias de](../development/commerce-version.md#verify-zend-framework-composer-dependencies) Zend Framework.<!--MCLOUD-4094-->

   - ![Icono](../../assets/new.svg) **nuevo Se agregó validación para `env.php` archivos y datos**: se agregaron comprobaciones del archivo y los `env.php` datos durante el proceso de instalación y actualización.<!--MCLOUD-5991-->

      - Si falta el `env.php` archivo en la instalación y el `crypt/key` valor no se especifica en el `.magento.app.yaml` archivo, el implementación falla con el siguiente notificación:

        ```text
        The crypt/key key value does not exist in the ./app/etc/env.php file or the CRYPT_KEY cloud environment variable``Missing crypt key for upgrading Magento`.
        ```

      - Si la instalación no incluye el `env.php` archivo, o la configuración contiene sólo un tipo de caché, el `cron:enable` comando se ejecuta durante el proceso de actualización para restaurar el archivo con todos los `cache_types`archivos . Se agrega la siguiente notificación al registro:

        ```text
        Magento state indicated as installed but configuration file app/etc/env.php was empty or did not exist.
        Required data will be restored from environment configurations and from the .magento.env.yaml file.
        ```

## v2002.1.0

Fecha de publicación: 6 de febrero de 2020

- ![nuevo icono](../../assets/new.svg) **Actualizaciones** de infraestructura:

   - ![Icono](../../assets/new.svg) **nuevo Se ha agregado un paquete independiente para Cloud Docker for Commerce**: se desacopló el paquete de Docker del paquete para mantener la calidad del `ece-tools` código y proporcionar versiones independientes. Las actualizaciones y correcciones relacionadas con `ece-tools` se administran desde el repositorio GitHub magento-nube-docker[](https://github.com/magento/magento-cloud-docker).<!--MAGECLOUD-2927-->

   - ![Icono](../../assets/new.svg) **nuevo Capacidades** de aplicación de parches actualizadas: se ha movido el funcionalidad de aplicación de revisiones del paquete ECE-Herramientas a un paquete magento-nube-patches](https://github.com/magento/magento-cloud-patches) independiente[. Durante implementación, `ece-tools` utiliza el nuevo paquete para aplicar parches. Consulte [Parches de nube Notas de la versión](cloud-patches.md).<!--MAGECLOUD-4567-->

   - ![](../../assets/new.svg) **icono nuevo Dependencias del compositor actualizadas**: se ha actualizado el `composer.json` archivo para Adobe Systems Commerce en infraestructura en la nube con una dependencia para el `magento/magento-cloud-docker` paquete. Ahora, `ece-tools` incluye dependencias para todos los paquetes en [`Cloud Tools Suite for Commerce`](cloud-tools-suite.md). Estos paquetes se instalan y actualizan automáticamente al instalar o actualizar `ece-tools`.

- ![](../../assets/new.svg) **icono nuevo Compatibilidad con implementaciones basadas en escenarios**:<!--MAGECLOUD-4101-->

   - ![](../../assets/new.svg) icono nuevo Ahora puede personalizar los procesos versión, implementar y entrada-implementar mediante archivos de configuración XML para anular o personalizar la configuración predeterminada.

   - ![Icono](../../assets/new.svg) **nuevo Se ha cambiado la `hooks` configuración en —Hemos actualizado el `hooks` formato de configuración para admitir implementaciones basadas en`.magento.app.yaml`** escenarios. Aún se admite el formato heredado de la versión anterior ECE-Herramientas 2002.0.x. Sin embargo, debe actualizar a la nueva formato para utilizar la característica de implementación basada en escenarios. Consulte [Implementaciones basadas en escenarios](../deploy/scenario-based.md#add-scenarios-using-build-and-deploy-hooks).

>[!NOTE]
>
>Antes de actualizar a la versión 2002.1.0 de ECE-Herramientas, revise los cambios](backward-incompatible-changes.md) incompatibles con versiones [anteriores para obtener información sobre los cambios que podrían requerir la actualización de Adobe Systems Commerce en infraestructura en la nube configuración o procesos del proyecto.

- ![nuevo icono](../../assets/new.svg) **Actualizaciones** de servicio:

   - ![Icono](../../assets/new.svg) nuevo Se agregó compatibilidad con PHP 7.3.<!--MAGECLOUD-4022-->

   - ![Icono](../../assets/new.svg) nuevo Se ha añadido compatibilidad con RabbitMQ 3.8.<!--MAGECLOUD-4674-->

   - ![Icono](../../assets/new.svg) nuevo Se ha agregado validación para comprobar las versiones de servicio instaladas con la fecha de finalización de cada servicio. Ahora, los clientes reciben un notificación si una versión de servicio está dentro de los tres meses posteriores a la fecha de EOL, y una advertencia si la fecha de EOL es en el pasado.<!--MAGECLOUD-4076-->

   - ![Icono Fix](../../assets/fix.svg) Se ha corregido un problema de configuración Elasticsearch para garantizar que se configuran los ajustes de Elasticsearch correctos en todos los entornos.<!--MAGECLOUD-4474-->

>[!NOTE]
>
>Consulte [Versiones](../services/services-yaml.md#service-versions) de servicio para obtener una lista de los servicios utilizados en Adobe Systems Commerce on infraestructura en la nube y su compatibilidad de versiones con Cloud plantilla.

- ![nuevo icono Actualizaciones](../../assets/new.svg) **de variable de** entorno:

   - ![Icono](../../assets/new.svg) nuevo Se ha ampliado el funcionalidad del variable de entorno para admitir la precarga de caché `WARM_UP_PAGES` para páginas de producto específicas. Consulte la definición ampliada en el [tema Variables](../environment/variables-post-deploy.md#warm_up_pages) de entrada-implementar.<!--MAGECLOUD-4444-->

   - ![Icono](../../assets/new.svg) nuevo Se ha agregado la variable entorno para simplificar los `ERROR_REPORT_DIR_NESTING_LEVEL` gestión de datos del informe de errores en el `<magento_root>/var/report/` directorio. Consulte la descripción del variable en el tema Variables](../environment/variables-build.md#error_report_dir_nesting_level) de [versión.

   - ![Icono Fix](../../assets/fix.svg) Se han eliminado las `SCD_EXCLUDE_THEMES`variables , `STATIC_CONTENT_THREADS`,`DO_DEPLOY_STATIC_CONTENT`, y `STATIC_CONTENT_SYMLINK` entorno. Consulte [Cambios](backward-incompatible-changes.md#environment-configuration-changes) incompatibles con versiones anteriores.<!--MAGECLOUD-4407, MAGECLOUD-3873-->

   - ![Icono Fix](../../assets/fix.svg) Se corrigió un problema en el proceso de configuración de Elastic Suite para que la configuración predeterminada se sobrescriba como se esperaba al configurar el variable de `ELASTICSUITE_CONFIGURATION` implementar sin la `_merge` opción.<!--MAGECLOUD-4388-->

- ![nuevo icono Actualizaciones](../../assets/new.svg) **** de comandos de CLI:

   - ![nuevo icono](../../assets/new.svg) **Nuevo comando** cron: ahora puede administrar manualmente el procesamiento cron en su Adobe Systems Commerce en infraestructura en la nube entorno utilizando los `cron:disable` comandos and `cron:enable` . Utilice el comando disable para detener todos los procesos cron activos y deshabilitar todos los trabajos cron. Utilice el comando enable para volver a activar los trabajos cron cuando esté listo. Consulte [Desactivación de trabajos](../application/crons-property.md#disable-cron-jobs) cron.

   - ![Icono](../../assets/new.svg) **nuevo sistema de informes** de error mejorado: se ha agregado un mejor registro para los errores de comandos de CLI que ocurren durante el procesamiento de ECE-Herramientas.<!--MAGECLOUD-4849-->

   - ![nuevo icono](../../assets/new.svg) **Quitar comandos de versión obsoletos**: se han eliminado los siguientes comandos de versión: `m2-ece-build`, `m2-ece-deploy`, `m2-ece-scd-dump`, y se ha cambiado el nombre `ece-tools docker` de los comandos a `ece-docker`. Ver [cambios incompatibles con versiones anteriores](backward-incompatible-changes.md)<!--MAGECLOUD-4392-->

- ![nuevo icono](../../assets/new.svg) eliminó el archivo `build_options.ini` obsoleto y agregó validación para fallar en la compilación si el archivo existe. Utilice el archivo [.magento.env.yaml](../environment/configure-env-yaml.md) para configurar las opciones de generación.

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que ocasionaba que fallara el proceso de compilación cuando el archivo `config.php` estaba vacío.<!--MAGECLOUD-4127-->

## 2002.0.23

Fecha de publicación: 27 de febrero de 2020

- ![Icono Fix](../../assets/fix.svg) Se ha solucionado un problema de compatibilidad con `ece-tools` las versiones 2002.0.x que impedía que la generación de contenido estática bajo demanda se completara correctamente en el modo de producción.

## Versiones anteriores

Consulte el archivo](cloud-release-archive.md) de Notas de la versión para la [versión 2002.0.22 y anteriores.
