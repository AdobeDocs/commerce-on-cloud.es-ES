---
title: Archivo de notas de la versión para ece-tools
description: Obtenga información sobre las mejoras archivadas para ece-tools.
hide: true
hidefromtoc: true
recommendations: noDisplay, noCatalog
exl-id: 3ba39fa6-88e9-4177-956d-f3e382bf59e3
source-git-commit: 0d84d29c470a098c7238b6ca7cc9538463dda695
workflow-type: tm+mt
source-wordcount: '7145'
ht-degree: 0%

---

# Archivo de notas de la versión para ece-tools

>[!NOTE]
>
>Estas notas de la versión proporcionan información y actualizaciones para `ece-tools` v2002.0.22 y versiones posteriores. Consulte [Notas de la versión de Cloud Tools Suite](cloud-tools-suite.md) para obtener las últimas actualizaciones de `ece-tools` y otros paquetes de Cloud.

## v2002.0.22

La versión `ece-tools` 2002.0.22 cambia la estructura del paquete `ece-tools` para desvincular la versión de `Adobe Commerce on cloud infrastructure` parches de la versión ECE-Tools. A partir de esta versión, las revisiones y correcciones críticas se enviarán mediante el paquete [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches), que es una nueva dependencia para el paquete `ece-tools`. Hemos realizado estos cambios para reducir la complejidad a la hora de programar actualizaciones de versiones y trabajar con contribuciones de la comunidad.

- ![nuevo icono](../../assets/new.svg) **Cambios en el paquete de herramientas ECE**

   - ![nuevo icono](../../assets/new.svg) movió los parches de Adobe Commerce del paquete `ece-tools` a un nuevo paquete de compositor [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches).

   - ![nuevo icono](../../assets/new.svg) Actualizó el archivo `composer.json` del paquete `ece-tools` para agregar una dependencia para el paquete `magento/magento-cloud-patches` v1.0.0.

   - ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que provocaba que el proceso de aplicación de parches de `ece-tools` se interrumpiera al aplicar conjuntos de parches sobre las versiones de solo seguridad, a partir de la versión 2.3.2-p2 y posteriores. Este problema fue introducido por el nuevo esquema de versiones adoptado para [parches de solo seguridad](https://experienceleague.adobe.com/en/docs/commerce-operations/release/notes/security-patches/overview).<!--MAGECLOUD-4661-->

- ![icono de corrección](../../assets/fix.svg) **Parches y correcciones críticas**: Actualice los entornos de nube con `ece-tools` versión 2002.0.22 para aplicar las siguientes revisiones y correcciones críticas. Estas revisiones están incluidas en el paquete `magento/magento-cloud-patches` v1.0.0.

   - ![Icono de correcciones](../../assets/fix.svg) **Revisiones de seguridad de Page Builder para las versiones 2.3.1.x y 2.3.2.x**: corrige un problema en la vista previa de Page Builder que permite a los usuarios no autenticados acceder a algunos métodos de creación de plantillas que se pueden usar para almacenar en déclencheur la ejecución de código arbitrario a través de la red (RCE), lo que da como resultado filtraciones de información global. Este problema puede producirse al utilizar versiones no compatibles de Page Builder con Adobe Commerce 2.3.1 y 2.3.2.<!--MAGECLOUD-4649-->

   - ![icono de corrección](../../assets/fix.svg) **parches MSI**: corrige problemas que causaban errores de indización y problemas de rendimiento al usar la configuración de inventario predeterminada para administrar existencias.<!--MAGECLOUD-4428-->

   - ![Icono de corrección](../../assets/fix.svg) **Compatibilidad con versiones anteriores de las nuevas interfaces de correo**: corrige un problema de incompatibilidad con versiones anteriores causado por la interfaz PHP `Magento\Framework\Mail\EmailMessageInterface` introducida en Adobe Commerce v2.3.3. En el ámbito de esta revisión, el nuevo `EmailMessageInterface` hereda del antiguo `MessageInterface`, y los módulos principales de Adobe Commerce se revierten para depender de `MessageInterface`.<!--MAGECLOUD-4422-->

   - ![Icono de corrección](../../assets/fix.svg) **La paginación del catálogo no funciona en Elasticsearch 6.x**: corrige un problema crítico con la paginación de resultados de búsqueda que afecta a los clientes que usan Elasticsearch 6.x como motor de búsqueda del catálogo.<!--MAGECLOUD-4448-->

## v2002.0.21

- ![nuevo icono](../../assets/new.svg) **actualizaciones de Docker**—

   - ![nuevo icono](../../assets/new.svg) **Nuevas imágenes Docker**—Compatible con las versiones 2.3.3 y posteriores<!-- MAGECLOUD-3345 -->

      - PHP versión 7.3.<!-- MAGECLOUD-4017 -->

      - Caché de barniz 6.2.0<!-- MAGECLOUD-4017 -->

   - ![nuevo icono](../../assets/new.svg) agregó compatibilidad para aplicar la configuración de enlace personalizada especificada en `.magento.app.yaml` en el entorno Docker. Anteriormente, el entorno de Docker solo admitía la configuración de enlace predeterminada.<!-- MAGECLOUD-3505-->

   - ![nuevo icono](../../assets/new.svg) Los archivos ENV de Docker ya no se generan durante la generación de Docker y el comando `docker:config:convert` está obsoleto. Los datos correspondientes ahora se almacenan en el archivo `docker-compose.yml`.<!-- MAGECLOUD-3816-->

   - ![nuevo icono](../../assets/new.svg) **Se ha actualizado la imagen de PHP** y se ha agregado Node.js a la imagen Docker de PHP para admitir las funciones node, npm y grunt-cli.<!-- MAGECLOUD-3953 -->

- ![nuevo icono](../../assets/new.svg) **Actualizaciones de variables de entorno**-

   - ![nuevo icono](../../assets/new.svg) agregó la variable de implementación **LOCK_PROVIDER** para configurar el proveedor de bloqueo, lo que impide el inicio de trabajos cron duplicados y grupos cron. Consulte la descripción de la variable en el tema [implementar variables](../environment/variables-deploy.md#lock_provider).<!-- MAGECLOUD-4052 -->

   - ![nuevo icono](../../assets/new.svg) agregó la variable de entorno **CONSUMERS_WAIT_FOR_MAX_MESSAGES** para configurar cómo los consumidores procesan los mensajes de la cola de mensajes al usar la variable de entorno `CRON_CONSUMERS_RUNNER` para administrar los trabajos cron. Consulte la descripción de la variable en el tema [implementar variables](../environment/variables-deploy.md#consumers_wait_for_max_messages).<!-- MAGECLOUD-4071 -->

   - ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que puede provocar errores de interbloqueo de la base de datos cuando el trabajo cron de `consumers_runner` inicia varias instancias del mismo consumidor en nodos diferentes. Ahora, si ha habilitado la variable de implementación [**CRON_CONSUMERS_RUNNER**](../environment/variables-deploy.md#cron_consumers_runner) en su entorno, el trabajo de `consumers_runner` utiliza la opción `single-thread` para iniciar una instancia de cada consumidor en un solo nodo.<!-- MAGECLOUD-3913 -->

   - ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que afectaba a la funcionalidad de [**WARM_UP_PAGES**](../environment/variables-post-deploy.md#warm_up_pages) y que usa una dirección URL de almacén predeterminada. Ahora, si el comando `config:show:default-url` no puede obtener una dirección URL base, se utilizará la dirección URL de la variable MAGENTO_CLOUD_ROUTES.<!-- MAGECLOUD-3866 -->

- ![nuevo icono](../../assets/new.svg) Actualizó la información de registro devuelta por el comando `module:refresh`. Ahora puede ver una lista detallada de los módulos habilitados en el archivo `cloud.log`.<!-- MAGECLOUD-2514 -->

- ![nuevo icono](../../assets/new.svg) Se mejoraron las notificaciones de advertencia y validación de compatibilidad de versiones para los problemas de compatibilidad entre la versión de Adobe Commerce y los servicios instalados, como Elasticsearch, [!DNL RabbitMQ], Redis y DB.<!-- MAGECLOUD-3535 -->

- ![nuevo icono](../../assets/new.svg) agregó compatibilidad con RabitMQ versión 3.8.<!-- MAGECLOUD-4674-->

- ![nuevo icono](../../assets/new.svg) Se han actualizado las validaciones interactivas de la compatibilidad del servicio para reflejar las versiones compatibles con las nuevas versiones de Adobe Commerce 2.3.3 y 2.2.10. Consulte [Requisitos del sistema](https://experienceleague.adobe.com/en/docs/commerce-operations/installation-guide/system-requirements) en la _Guía de instalación_ para ver las versiones recomendadas.<!-- MAGECLOUD-4018 -->

- ![icono de corrección](../../assets/fix.svg) Se mejoró el mensaje de registro devuelto cuando el proceso de administración de trabajos de cron en la fase de implementación intenta detener un trabajo de cron que ya ha finalizado para aclarar que este problema no es un error. Se cambió el nivel de registro de `INFO` a `DEBUG`.<!-- MAGECLOUD-3653-->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que se producía al ejecutar el comando `setup:upgrade` y que no interrumpía el proceso de implementación cuando se producía un error durante la tarea `app:config:import`.<!-- MAGECLOUD-3806 -->

- ![nuevo icono](../../assets/new.svg) Cambió el nivel de registro predeterminado del controlador de archivos a `debug` para reducir la cantidad de detalles en el registro mostrado en [!DNL Cloud Console], al tiempo que se sigue proporcionando información detallada para la depuración.<!-- MAGECLOUD-3871 -->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que provocaba un error en la implementación de contenido estático durante la compilación. Después de una instalación y del volcado de configuración de `ece-tools`, se produjo un error si no se especificó ninguna configuración regional para el usuario administrador en el archivo `config.php`. Ahora existe una configuración regional predeterminada para el usuario administrador en el archivo `config.php`.<!-- MAGECLOUD-3957 -->

- ![icono de corrección](../../assets/fix.svg) Se corrigió un error de `Undefined index error` que se produce cuando un comando CLI de `magento-cloud` falla en un entorno que no está configurado con una dirección URL segura (https). Ahora, el paquete ECE-Tools utiliza la dirección URL base (http) si la dirección URL segura no está disponible.<!-- MAGECLOUD-4009 -->

## v2002.0.20

- ![nuevo icono](../../assets/new.svg) **Actualizaciones de Docker**—

   - ![nuevo icono](../../assets/new.svg) Ahora puede realizar pruebas funcionales con el paquete `ece-tools` en el entorno Docker. Ver [prueba de aplicación](https://developer.adobe.com/commerce/cloud-tools/docker/test/code-testing).<!-- MAGECLOUD-3129/3684 -->

   - ![nuevo icono](../../assets/new.svg) Se agregó compatibilidad para configurar módulos PHP usando el archivo `.magento.app.yaml`. Cualquier extensión [PHP especificada en el archivo `.magento.app.yaml`](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/app/php-settings#enable-extensions) pasa a estar disponible en los contenedores Docker PHP.<!-- MAGECLOUD-3357 -->

   - ![nuevo icono](../../assets/new.svg) Hay nuevos comandos disponibles para mejorar la experiencia de línea de comandos de Docker. Consulte la sección [`bin/magento-docker` de la referencia de Docker ](https://developer.adobe.com/commerce/cloud-tools/docker/quick-reference#cloud-docker-cli).<!-- MAGECLOUD-3569 -->

   - ![nuevo icono](../../assets/new.svg) agregó la capacidad de usar Mutagen.io para sincronizar archivos durante el desarrollo entre el host local y Docker.<!-- MAGECLOUD-3559 -->

   - ![icono de corrección](../../assets/fix.svg) Corrigió la ruta predeterminada al usar el entorno Docker. Ahora, cuando utilice SSH para iniciar sesión en el contenedor Docker, se encontrará en la raíz del proyecto en el directorio `/app`, como se espera.<!-- MAGECLOUD-3582 -->

   - ![Icono de corrección](../../assets/fix.svg) Actualizó la biblioteca Sodium de la versión 1.0.11 a la versión 1.0.18 y actualizó la extensión Sodium PHP.<!-- MAGECLOUD-3832 -->

     >[!WARNING]
     >
     >Los clientes de Adobe Commerce en la infraestructura en la nube deben [Enviar un ticket de soporte de Adobe Commerce](https://support.magento.com/hc/en-us/articles/360000913794#submit-ticket) para actualizar el paquete libsodio en los entornos de Producción profesional y Ensayo antes de actualizar a Adobe Commerce 2.3.2. Actualmente, no se pueden actualizar los entornos de Inicio a Adobe Commerce 2.3.2.

   - ![icono de corrección](../../assets/fix.svg) agregó los complementos de Elasticsearch `analysis-icu` y `analysis-phonetic` a todas las imágenes de Docker.<!-- MAGECLOUD-3446 -->

   - ![icono de corrección](../../assets/fix.svg) Validaciones mejoradas: al utilizar opciones para el comando `docker:build`, debe proporcionar un valor al utilizar una opción. Además, se agregó validación para la versión del nodo al usar el comando `docker:build run`.<!-- MAGECLOUD-3486 & MAGECLOUD-3678 -->

- ![nuevo icono](../../assets/new.svg) **Actualizaciones de variables de entorno**—

   - ![nuevo icono](../../assets/new.svg) agregó compatibilidad con los prefijos de tabla de base de datos usando la [variable de entorno DATABASE_CONFIGURATION](../environment/variables-deploy.md#database_configuration).<!-- MAGECLOUD-2901 -->

   - ![nuevo icono](../../assets/new.svg) agregó la variable de implementación **FORCE_UPDATE_URLS** para actualizar las URL de base al implementar en los entornos de ensayo y producción de Pro y Starter. Ver la definición en el contenido de [implementar variables](../environment/variables-deploy.md#force_update_urls).<!-- MAGECLOUD-3602 -->

   - ![nuevo icono](../../assets/new.svg) agregó la variable posterior a la implementación **TTFB_TESTED_PAGES** para configurar las pruebas de página de _Tiempo hasta el primer byte_ a fin de comprobar el rendimiento de la aplicación en los sitios implementados en la infraestructura de la nube. Consulte la descripción de la variable en [variables posteriores a la implementación](../environment/variables-post-deploy.md).<!-- MAGECLOUD-3643 -->

   - ![Icono de corrección](../../assets/fix.svg) Se ha corregido un problema con SCD multiproceso, que causaba errores aleatorios en la implementación de contenido estático. La solución fue establecer la variable **SCD_THREADS** en `1`. Ahora puede aumentar el recuento según sea necesario. Vea las definiciones de las [variables de implementación](../environment/variables-deploy.md#scd_threads) y las [variables de compilación](../environment/variables-build.md#scd_threads).<!-- MAGECLOUD-3611 -->

   - ![Icono de correcciones](../../assets/fix.svg) Puede configurar la variable de entorno **WARM_UP_PAGES** para almacenar en caché páginas únicas, dominios múltiples y páginas múltiples. Vea la definición expandida en el contenido de [variables posteriores a la implementación](../environment/variables-post-deploy.md#warm_up_pages).<!-- MAGECLOUD-3258 -->

- ![icono de corrección](../../assets/fix.svg) agregó el archivo `pub/static/.htaccess` a la lista de exclusión. [Corrección enviada por Björn Kraus de PHOENIX MEDIA GmbH](https://github.com/magento/ece-tools/pull/455).<!-- MAGECLOUD-3545/Github#455 -->

- ![icono de corrección](../../assets/fix.svg) Se corrigió un error cuando todos los mensajes de validación se mostraban como `Critical` si al menos un validador de nivel crítico devolvía un error.<!-- MAGECLOUD-3178 -->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que provocaba un error de implementación si la dirección URL base no existía en la base de datos.<!-- MAGECLOUD-3075 -->

- ![nuevo icono](../../assets/new.svg) agregó un nuevo comando **`env:config:show`** al paquete `ece-tools` que muestra servicios de entorno, rutas o variables. Ver [Servicios, rutas y variables](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/dev-tools/ece-tools/package-overview#services-routes-and-variables). [Característica enviada por Vladimir Kerkhoff](https://github.com/magento/ece-tools/pull/486).<!-- MAGECLOUD-3451 -->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que provocaba un error crítico al intentar instalar Adobe Commerce 2.2.6 o una versión anterior con `ece-tools` desarrollo después de la refactorización del shell.<!-- MAGECLOUD-3665 -->

- ![Icono de corrección](../../assets/fix.svg) Se ha corregido un problema que causaba que las instalaciones de Adobe Commerce 2.1.x y 2.2.x fallaran con una advertencia sobre el uso de una versión obsoleta de Carbon.<!-- MAGECLOUD-3704 -->

- ![icono de corrección](../../assets/fix.svg) disminuyó el nivel de registro `cloud.log` para la salida del shell de `info` a `debug`.<!-- MAGECLOUD-3277 -->

- ![icono de corrección](../../assets/fix.svg) agregó la opción `--remove-definers (-d)` al comando `ece-tools db-dump` para quitar los definidores del archivo de volcado.<!-- MAGECLOUD-3510 -->

## v2002.0.19

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que sobrescribe el archivo `env.php` durante una implementación, lo que provoca la pérdida de configuraciones personalizadas. Esta actualización garantiza que Adobe Commerce en la infraestructura de la nube actualice el archivo `env.php` con cada implementación, conservando al mismo tiempo las configuraciones personalizadas.<!-- MAGECLOUD-3668 -->

## v2002.0.18

- ![nuevo icono](../../assets/new.svg) **Actualizaciones de Docker**—

   - ![nuevo icono](../../assets/new.svg) Ahora, el entorno Docker admite la configuración de cron definida en la propiedad [crons del archivo .magento.app.yaml](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/app/properties/crons-property).<!-- MAGECLOUD-3150 -->

   - ![nuevo icono](../../assets/new.svg) **Nuevo contenedor de Docker**—Se agregó un [contenedor de proxy de terminación TLS](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service#varnish-container) para facilitar la terminación SSL de Varnish a través de HTTPS.<!-- MAGECLOUD-2890 -->

   - ![nuevo icono](../../assets/new.svg) **Nueva imagen Docker**—Se ha agregado una imagen Node.js para admitir Gulp y otras capacidades, como Prueba unitaria de JS de Jasmine.<!-- MAGECLOUD-3345 -->

   - ![nuevo icono](../../assets/new.svg) **Modos de generación de Docker**: ahora puede elegir iniciar el entorno de Docker en [modo de producción o modo de desarrollador](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/#launch-mode). El modo de desarrollador admite el desarrollo activo con permisos de sistema de archivos completos y editables.<!-- MAGECLOUD-3152/3511 -->

   - ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que hacía que la implementación de Docker fallara con un error `Name or service not known` si la caché estaba configurada para un servicio que no está disponible. Ahora, puede quitar un servicio del archivo [`.magento/services.yaml`](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/service/services-yaml). El generador de configuración de Docker actualiza automáticamente el servicio en el archivo `docker/config.php.dist`.<!-- MAGECLOUD-3369 -->

   - ![nuevo icono](../../assets/new.svg) agregó validaciones interactivas para la compatibilidad del servicio. Ahora, si un servicio solicitado no es compatible con la versión de Adobe Commerce u otros servicios, el _modo interactivo_ le pide al usuario un mensaje y una opción para continuar. Consulte las [versiones de servicio](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#service-containers) disponibles para Docker. Utilice la opción `-n` para omitir la interactividad con fines de CI/CD.<!-- MAGECLOUD-3251 -->

   - ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema con el comando Docker compose `db-dump` que borraba los volcados existentes.<!-- MAGECLOUD-3366 -->

   - ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que asignaba el almacenamiento de caché de Redis `session`, `default` y `page_cache` al mismo id. de base de datos.<!-- MAGECLOUD-3172 -->

- ![nuevo icono](../../assets/new.svg) **Actualizaciones de variables de entorno**—

   - ![nuevo icono](../../assets/new.svg) La nueva variable de entorno **ELASTICSUITE\_CONFIGURATION** conserva la configuración de servicio personalizada entre implementaciones. Ver la definición en el contenido de [implementar variables](../environment/variables-deploy.md#elasticsuite_configuration).<!-- MAGECLOUD-3205 -->

   - ![nuevo icono](../../assets/new.svg) agregó la variable de entorno **SCD_MAX_EXECUTION_TIMEOUT** para que pueda aumentar el tiempo para completar la implementación de contenido estático desde el archivo `.magento.env.yaml`. Vea la definición en [implementar variables](../environment/variables-deploy.md#scd_max_execution_time), [generar variables](../environment/variables-build.md#scd_max_execution_time) y [variables globales](../environment/variables-global.md#scd_max_execution_time).<!-- MAGECLOUD-2822 -->

      - ![nuevo icono](../../assets/new.svg) agregó la variable de entorno **MAGENTO_CLOUD_LOCKS_DIR** para configurar la ruta al punto de montaje del proveedor de bloqueos en la infraestructura de la nube. El proveedor de bloqueo evita el inicio de trabajos cron y grupos cron duplicados. Esta variable es compatible con la versión 2.2.5 y posteriores de Adobe Commerce y se configura automáticamente. Ver la definición en [variables de nube](../environment/variables-cloud.md).<!-- MAGECLOUD-3135 -->

      - ![icono de corrección](../../assets/fix.svg) Cambió los valores predeterminados de la variable de entorno **SCD_THREADS** para determinar automáticamente el valor óptimo en función del recuento de subprocesos de CPU detectado. Vea las definiciones actualizadas en las [variables de implementación](../environment/variables-deploy.md#scd_threads) y en las [variables de compilación](../environment/variables-build.md#scd_threads).<!-- MAGECLOUD-3382 -->

- ![Icono de corrección](../../assets/fix.svg) Se ha corregido un problema con un parche para el Mecanismo de aislamiento de BD que provocaba un error al actualizar a Adobe Commerce en la versión 2002.0.16 de la infraestructura de nube.<!-- MAGECLOUD-3383 -->

- ![icono de corrección](../../assets/fix.svg) agregó un parche que reemplaza _Gráficos de imágenes de Google_ por _Gráficos de imágenes_. Consulte el artículo de DevBlog [Desaprobación y actualización de los gráficos de imágenes de Google para M1](https://community.magento.com/t5/Magento-DevBlog/Google-Image-Charts-deprecation-and-update-for-M1/ba-p/125006).<!-- MAGECLOUD-3456 -->

- ![Icono de corrección](../../assets/fix.svg) Ha agregado validación para la [variable SEARCH_CONFIGURATION](../environment/variables-deploy.md#search_configuration). La implementación falla cuando no se ha establecido la opción &#39;engine&#39; y no se requiere `_merge`.<!-- MAGECLOUD-3470 -->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que exponía datos confidenciales tras producirse una excepción. Ahora la información confidencial está enmascarada correctamente.<!-- MAGECLOUD-3525 -->

- ![icono de corrección](../../assets/fix.svg) mejoró la configuración de tolerancia a errores del paquete de Magento Open Source. En caso de que Adobe Commerce no pueda leer datos de la instancia de Redis `slave`, se realizará una lectura desde la instancia de Redis `master`. Consulte [REDIS_USE_SLAVE_CONNECTION](../environment/variables-deploy.md#redis_use_slave_connection).<!-- MAGECLOUD-2899 -->

## v2002.0.17

>[!NOTE]
>
>La versión 2002.0.17 de `ece-tools` incluye una revisión de seguridad importante. Consulte [Recursos técnicos: parches de Magento Open Source](https://magento.com/tech-resources/download#download2288).

- ![nuevo icono](../../assets/new.svg) **Actualizaciones de servicio**—Compatible con las siguientes versiones de Adobe Commerce: 2.2.8 y posteriores 2.2.x, 2.3.1 y posteriores 2.3.x

   - Se agregó compatibilidad con Elasticsearch versión 6.x.<!-- MAGECLOUD-3196 -->

   - Se ha agregado compatibilidad con la versión 5.0 de Redis.

- ![nuevo icono](../../assets/new.svg) **Nuevas imágenes Docker**—Se agregaron los siguientes servicios a la compilación de Docker:

   - Elasticsearch 6.5<!-- MAGECLOUD-3196 -->

   - Redis 5.0<!-- MAGECLOUD-3223 -->

- ![nuevo icono](../../assets/new.svg) **Nueva variable de entorno**—Anteriormente, se agotaba el tiempo de espera codificado para la compresión SCD. Ahora puede configurar el tiempo de espera de compresión SCD mediante la variable de entorno **SCD_COMPRESSION_TIMEOUT**. Vea las definiciones en el contenido de [variables de compilación](../environment/variables-build.md#scd_compression_timeout) e [variables de implementación](../environment/variables-deploy.md#scd_compression_timeout).<!-- MAGECLOUD-2870 -->

- ![icono de corrección](../../assets/fix.svg) agregó la opción `--use-rewrites` al comando de instalación para que utilice reescrituras de servidor web para los vínculos generados en la tienda y el acceso de administrador para mejorar la seguridad y la experiencia del cliente.<!-- MAGECLOUD-3246 -->

- ![icono de corrección](../../assets/fix.svg) agregó marcas de hora al archivo `var/log/install_upgrade.log` para que muestre las fechas de los eventos de instalación y actualización.<!-- MAGECLOUD-2895 -->

## v2002.0.16

- ![nuevo icono](../../assets/new.svg) **actualizaciones de Docker**—

   - Ahora, la configuración de servicio predeterminada generada en el entorno de Docker es la misma que la configuración predeterminada en la plantilla de nube.<!-- MAGECLOUD-3025 -->

   - Puede enviar correo desde su entorno Docker utilizando el servicio `sendmail`.<!-- MAGECLOUD-2907 -->

   - Se ha agregado la capacidad de [configurar Xdebug](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug) para depurar en el entorno Cloud Docker.<!-- MAGECLOUD-2891 -->

   - Se corrigió un problema con los permisos del servicio web al generar el archivo `docker-compose.yml`.<!-- MAGECLOUD-2883 -->

- ![nuevo icono](../../assets/new.svg) **Mejora de la actualización**—Se ha agregado validación para confirmar que la propiedad `autoload` del archivo `composer.json` contenga los cambios de configuración necesarios antes de actualizar a Adobe Commerce v2.3. Ver [Actualización](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/upgrade/commerce-version).<!-- MAGECLOUD-2392 -->

- ![nuevo icono](../../assets/new.svg) El proceso de compresión al implementar contenido estático ahora incluye todos los recursos (generados o personalizados de forma nativa) y se produce durante la fase de compilación al principio de la sección [`build:transfer`](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/app/properties/hooks-property). Anteriormente, el proceso de compresión se producía antes de aplicar la minificación y el agrupamiento personalizados de los recursos estáticos. [Corrección enviada por Rafael Garcia Lepper de Trytens Limited](https://github.com/magento/ece-tools/pull/413).<!-- MAGECLOUD-3104 -->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un error de conexión a base de datos que se producía durante la implementación inmediatamente después de configurar una base de datos y una relación de servicio adicionales. Además, esta corrección soluciona un problema que se producía durante el proceso de configuración de Commerce Reporting for Starter. Para empezar, esta actualización es un elemento &quot;obligatorio&quot; para usar los informes de Commerce.<!-- MAGECLOUD-3035 -->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema de validación con la configuración de la base de datos que hacía que fallara el proceso de implementación.<!-- MAGECLOUD-3003 -->

- ![icono de corrección](../../assets/fix.svg) Actualizó la restricción con la versión apropiada del paquete `symfony/yaml` para usarlo con [constantes de PHP](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/env/configure-env-yaml#php-constants). El análisis constante no funciona cuando se usa una versión de paquete de `symfony/yaml` anterior a la 3.2. [Corrección enviada por Vladimir Kerkhoff](https://github.com/magento/ece-tools/pull/404).<!-- MAGECLOUD-2956 -->

- ![nuevo icono](../../assets/new.svg) **Comprobación de configuración del entorno**—Se agregó validación para comprobar la versión de PHP y advertir a los usuarios si no están utilizando la última versión recomendada.<!--MAGECLOUD-2903-->

- ![Icono de corrección](../../assets/fix.svg) Se ha corregido un problema con el procesamiento de variables JSON con formato incorrecto. Ahora, si una variable JSON causa un error de sintaxis, aparece una advertencia en el archivo `cloud.log` y la implementación continúa usando la variable predeterminada.<!-- MAGECLOUD-2851 -->

- ![icono de corrección](../../assets/fix.svg) Se corrigió un error de conexión que se produjo durante la implementación inmediatamente después de deshabilitar el servicio Redis.<!-- MAGECLOUD-2747 -->

- ![nuevo icono](../../assets/new.svg) **Cambios de registro**—Se ha actualizado el [nivel de registro](../environment/log-handlers.md#log-levels) de `Info` a `Notice` para los siguientes eventos de proceso de compilación e implementación:<!--MAGECLOUD-2925-->

   - Inicio y fin del proceso de reconciliación de módulos instalados en `composer.json` con valores de configuración compartidos en el archivo `app/etc/config.php`

   - Inicio y final del proceso de validación de la configuración

   - Inicio y final del proceso de `setup:di:compile` para la generación de clases

- ![nuevo icono](../../assets/new.svg) **Nuevas variables de entorno**—

   - **[RESOURCE_CONFIGURATION implementa la variable](../environment/variables-deploy.md#resource_configuration)**—Utilice esta variable para asignar un nombre de recurso a una conexión de base de datos.<!-- MAGECLOUD-3026 & MAGECLOUD-2963-->

   - **[Variable global X_FRAME_CONFIGURATION](../environment/variables-global.md#x_frame_configuration)**: utilice esta variable para cambiar la configuración del encabezado `X-Frame-Options` para procesar una página de Adobe Commerce en `<frame>`, `<iframe>` o `<object>`.<!-- MAGECLOUD-3048 -->

- ![icono de corrección](../../assets/fix.svg) **Actualizaciones de variables de entorno**—Se han cambiado las siguientes variables de entorno:

   - **[WARM_UP_PAGES](../environment/variables-post-deploy.md)**: se ha agregado la capacidad de precargar la caché para páginas especificadas en todos los dominios definidos para una tienda Adobe Commerce. Anteriormente, si el sitio estaba configurado con varios dominios, el proceso posterior a la implementación no pudo cargar previamente la caché para las páginas especificadas en dominios no predeterminados y devolvió el siguiente error en el registro posterior a la implementación: `ERROR: Warming up failed: <uri>`<!-- MAGECLOUD-2466 -->

   - **SCD_COMPRESSION_LEVEL**: se ha actualizado la documentación y el archivo de muestra `.magento.env.yaml` con los valores predeterminados correctos para el nivel de compresión SCD. Vea las definiciones en el contenido de [variables de compilación](../environment/variables-build.md#scd_compression_level) e [variables de implementación](../environment/variables-deploy.md#scd_compression_level).<!-- MAGECLOUD-2823 -->

   - **SCD_EXCLUDE_THEMES**: esta variable de entorno está obsoleta. Use [SCD_MATRIX](../environment/variables-build.md#scd_matrix) para controlar la configuración del tema.<!--MAGECLOUD-2882-->

   - **SCD\_MATRIX**: se corrigió el proceso de validación para evitar un problema que se producía cuando SCD_MATRIX omitía un valor de tema que contenía diferentes casos de caracteres. Vea las definiciones en el contenido de [variables de compilación](../environment/variables-build.md#scd_matrix) e [variables de implementación](../environment/variables-deploy.md#scd_matrix).<!-- MAGECLOUD-2904 -->

   - **Variables de administrador**—<!-- MAGECLOUD-2573/MAGECLOUD-2848 -->

      - Se ha mejorado la seguridad al administrar credenciales para el usuario administrador mediante variables de entorno. Ya no puede utilizar las variables de entorno ADMIN_EMAIL, ADMIN_USERNAME y ADMIN_PASSWORD para anular las credenciales de administrador durante las actualizaciones. Si no puede acceder al Panel de administración, use la característica _Olvidé la contraseña_ o el comando CLI `admin:user:create` para crear un nuevo usuario administrador. Ver [Acceder a tu panel de administración](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/start/onboarding#admin).

      - ADMIN_EMAIL ya no es necesario al actualizar o aplicar parches.

## v2002.0.15

- ![nuevo icono](../../assets/new.svg) **actualizaciones de Docker**—

   - Ahora el generador Docker usa los servicios especificados en los archivos de configuración `.magento.app.yaml` y `.magento/services.yaml` al [crear su entorno Docker](https://developer.adobe.com/commerce/cloud-tools/docker/configure/). Puede elegir una versión de servicio diferente mediante parámetros de compilación.<!-- MAGECLOUD-2888 -->

   - Imagen PHP 7.2 agregada: compatibilidad añadida para PHP 7.2 en Cloud Docker; se ha actualizado la [configuración de Launch Docker](https://developer.adobe.com/commerce/cloud-tools/docker/configure/) para incluir la opción `docker:build --php` y especificar la versión de PHP compatible con su versión de Adobe Commerce.<!-- MAGECLOUD-2799 -->

   - Se ha agregado un [contenedor Cron](https://developer.adobe.com/commerce/cloud-tools/docker/containers/cli#cron-container) basado en la imagen PHP-CLI.<!-- MAGECLOUD-2565 -->

   - Se agregaron los siguientes servicios a la versión de Docker:

      - [!DNL RabbitMQ] 3.5 y 3.7<!-- MAGECLOUD-2567 & 2889-->

      - Elasticsearch 1.7, 2.4 y 5.2<!-- MAGECLOUD-2569 & 2887 -->

      - Redis 3.2 y 4.0<!-- MAGECLOUD-2886 -->

- ![nuevo icono](../../assets/new.svg) **Configurar con constantes de PHP**—Se ha agregado compatibilidad con [constantes de PHP](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/env/configure-env-yaml#php-constants) en el archivo de configuración `.magento.env.yaml`.<!-- MAGECLOUD- 2575 -->

- ![nuevo icono](../../assets/new.svg) **Nueva variable de entorno**: de forma predeterminada, solo el entorno de producción tiene Google Analytics habilitado. Puede habilitar Google Analytics en los entornos de ensayo e integración con la [variable de entorno ENABLE_GOOGLE_ANALYTICS](../environment/variables-deploy.md#enable_google_analytics).<!--MAGECLOUD-2879-->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que quitaba las configuraciones personalizadas de cron del archivo `env.php` después de una reimplementación. Ahora, las configuraciones personalizadas de cron permanecen de forma segura en el archivo `env.php`.<!-- MAGECLOUD-2923 -->

- ![icono de corrección](../../assets/fix.svg) Se corrigieron incoherencias en los mensajes y [niveles de registro](../environment/log-handlers.md#log-levels) para las fases de compilación, implementación y posterior a la implementación. Se ha aumentado el nivel de los mensajes de registro inicial y final de **info** a **notify** para todas las fases y subfases. Se agregaron los mensajes de registro inicial y final, donde corresponda.<!-- MAGECLOUD-2919 -->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que involucraba procesos cron y que impedía el inicio de la fase posterior a la implementación, cuando estaba configurada. Ahora, si tiene habilitado el vínculo posterior a la implementación, los procesos cron volverán a habilitarse al principio de la fase posterior a la implementación.<!-- MAGECLOUD-2862 -->

- ![icono de corrección](../../assets/fix.svg) ha resuelto un problema que impedía una instalación correcta de Adobe Commerce al especificar una configuración de base de datos personalizada. Anteriormente, el proceso de instalación usaba la configuración de base de datos de la [variable MAGENTO_CLOUD_RELATIONSHIP](../environment/variables-cloud.md) aunque se designara información de conexión personalizada en la [variable de entorno DATABASE_CONFIGURATION](../environment/variables-deploy.md#database_configuration).<!--MAGECLOUD-2736-->

- ![icono de corrección](../../assets/fix.svg) corrigió el comando `config:dump` para que incluya cada configuración regional del sitio web en la sección `system` del archivo `config.php`.<!--MAGECLOUD-2740-->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que provocaba _errores de calentamiento_ durante la fase posterior a la implementación al corregir la referencia de la dirección URL base de origen.<!--MAGECLOUD-2797-->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que generaba archivos incorrectamente durante el proceso de `setup:di:compile`, lo que afectaba al módulo Amazon Pay.<!--MAGECLOUD-2850-->

## v2002.0.14

- ![nuevo icono](../../assets/new.svg) **Verificar estado ideal**—El asistente de `ideal-state` ahora verifica la configuración actual durante cada implementación y proporciona instrucciones claras para actualizar la configuración a fin de lograr una implementación más rápida y sin tiempo de inactividad.<!--MAGECLOUD-2372-->

- ![Icono de corrección](../../assets/fix.svg) **Cumplimiento de PCI**: se han actualizado los protocolos de mensajería para Adobe Commerce en la infraestructura en la nube para que requieran la versión 1.2 de Seguridad de la capa de transporte (TLS) al conectarse con servicios de mensajería de terceros. Si utiliza un servicio de mensajes que no es compatible con TLS versión 1.2, debe actualizar el servicio. De lo contrario, se mostrará el siguiente mensaje de error cuando la aplicación de Adobe Commerce intente conectarse al servidor de mensajes para enviar un correo electrónico: `Unable to connect via TLS`.<!--MAGECLOUD-2521-->

- ![nuevo icono](../../assets/new.svg) **Mejora de la implementación**: Se agregó validación para advertir a los clientes si un entorno de ensayo o producción tiene habilitadas las opciones `dev`, `debug` o `debug_logging` para evitar problemas de rendimiento causados por una actividad de registro excesiva.<!--MAGECLOUD-2517-->

- ![icono de corrección](../../assets/fix.svg) **correcciones de implementación**—

   - Ahora el modo de mantenimiento está habilitado al principio de la fase de implementación y deshabilitado al final. Si la implementación falla, el sitio permanece en modo de mantenimiento hasta que se resuelvan los problemas de implementación. Anteriormente, el sitio regresaba al modo de producción aunque se produjera un error en la implementación.<!--MAGECLOUD-2603-->

      - Se han modificado las comprobaciones de validación de la fase de implementación para reducir el nivel de error en los siguientes problemas de implementación de `CRITICAL` a `WARNING`, de modo que se complete la implementación. Anteriormente, estos problemas provocaban que la implementación fallara.

      - La configuración del entorno contiene valores incorrectos para las variables de implementación o nube.

   - La versión de Elasticsearch en la infraestructura en la nube no es compatible con la versión del módulo elasticsearch/elasticsearch compatible con Adobe Commerce en la infraestructura en la nube. Consulte el [artículo de solución de problemas de Elasticsearch](https://support.magento.com/hc/en-us/articles/360015758471-Deployment-fails-or-interrupts-with-cloud-log-error-Elasticsearch-version-is-not-compatible-with-current-version-of-magento) en la base de conocimiento de asistencia de Adobe Commerce.<!--MAGECLOUD-2600-->

   - Se ha corregido un problema con la configuración compartida en el archivo `app/etc/config.php` que causaba `recursion detected` errores durante la implementación.<!--MAGECLOUD-2173-->

- ![icono de corrección](../../assets/fix.svg) **correcciones relacionadas con Cron**—

   - Se ha corregido un problema de programación cron que impedía que se ejecutaran trabajos si especificaba una frecuencia cron distinta de la predeterminada (1 minuto).<!--MAGECLOUD-2602-->

   - Se ha corregido un problema en la fase de implementación que permitía que los trabajos cron siguieran ejecutándose durante la implementación, lo que podía provocar bloqueos de la base de datos y otros problemas críticos. Ahora, todos los trabajos cron se detienen antes de que comience la fase de implementación y se reinician después de que finalice la implementación.&lt;!—MAGECLOUD—2537—>

   - Se ha corregido el flujo de trabajo de cron en las versiones 2.2.x para desbloquear los trabajos de cron congelados de modo que se puedan detener antes de iniciar la implementación. Anteriormente, un trabajo cron bloqueado provocaba que la implementación se detuviera.<!--MAGECLOUD-2501-->

- ![icono de corrección](../../assets/fix.svg) Cambió el formato del archivo `config.php` generado por el comando `vendor/bin/ece-tools config:dump` para utilizar sintaxis de matriz corta y sangría de 4 espacios para cumplir con los estándares de codificación de Adobe Commerce.<!--MAGECLOUD-2527-->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un error de implementación que se produce cuando `.magento.env.yaml` contiene `{{ base_url }}` y `{{ unsecure_base_url }}` marcadores de posición para las configuraciones web en lugar de la configuración de URL predeterminada para un proyecto de Adobe Commerce en la nube./<!--MAGECLOUD-2607-->

## v2002.0.13

- ![nuevo icono](../../assets/new.svg) **Habilitar implementación sin tiempo de inactividad**—Ahora Adobe Commerce en la nube pone en cola las solicitudes con los cambios de base de datos necesarios durante la implementación y aplica los cambios tan pronto como se completa la implementación. Las solicitudes se pueden retener durante un máximo de 5 minutos para garantizar que no se pierdan sesiones. Consulte [Opciones de implementación de contenido estático para reducir el tiempo de inactividad de la implementación en la nube](https://support.magento.com/hc/en-us/articles/360004861194-Static-content-deployment-options-to-reduce-deployment-downtime-on-Cloud).<!--MAGECLOUD-2169-->

- ![nuevo icono](../../assets/new.svg) **Docker Compose para Cloud**: Se han realizado las siguientes mejoras en el proceso de configuración de [Docker](https://developer.adobe.com/commerce/cloud-tools/docker/configure/):

   - Se agregó un comando—`docker:config:convert` para convertir los archivos de configuración de PHP al formato Docker ENV para simplificar la configuración del entorno. Ahora, usted copia los archivos de configuración de PHP en el directorio Docker y los convierte a los archivos ENV Docker. Ver [Iniciar Docker](https://developer.adobe.com/commerce/cloud-tools/docker/configure/).<!--MAGECLOUD-2359-->

   - El proceso de instalación de Adobe Commerce en la nube ahora admite la implementación en sistemas de archivos de solo lectura y de lectura-escritura para emular más estrechamente el sistema de archivos en la nube. Consulte [Configurar Docker](https://developer.adobe.com/commerce/cloud-tools/docker/configure/).&lt;!—MAGECLOUD—2357—>

   - **Compatibilidad con el servicio Redis**: Se ha agregado una imagen Redis, que se implementa en un contenedor de Docker y se configura automáticamente para que funcione con su instalación de Docker.&lt;!—MAGECLOUD—2442—>

   - Ahora tiene la capacidad de volcado de la base de datos al usar el [contenedor de base de datos](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service#database-container) de Cloud Docker. Además, puede [compartir archivos](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#sharing-data-between-host-machine-and-container) entre un equipo host y un contenedor mediante el directorio `docker/mnt`.<!-- MAGECLOUD-2577 -->

   - **Compatibilidad con el servicio Varnish**— Se ha agregado una imagen Varnish, que se implementa automáticamente en un contenedor Docker. Después de la implementación, puede configurar manualmente Varnish siguiendo las prácticas recomendadas de Adobe Commerce. Consulte [Configurar y utilizar Barniz](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cache/varnish/config-varnish).&lt;!—MAGECLOUD—2358—>

   - Acceso seguro al sitio: se ha añadido la compatibilidad con SSL para acceder a la tienda de Adobe Commerce y al panel de administración.&lt;!—MAGECLOUD—2360—>

- ![Icono de corrección](../../assets/fix.svg) **Se ha mejorado la compatibilidad con la extensión de la infraestructura en la nube de Adobe Commerce**: se ha bajado de categoría el requisito de versión mínima para el paquete guzzlehttp/guzzle en el archivo [composer.json](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/overview) de la infraestructura en la nube de Adobe Commerce a la versión 6.2 para que el paquete `ece-tools` sea compatible con más extensiones.<!--MAGECLOUD-2205-->

- ![nuevo icono](../../assets/new.svg) **Aplicar cambios personalizados a su aplicación de Adobe Commerce durante la fase de compilación**: dividimos la fase de compilación en dos procesos independientes para que pueda usar los enlaces para aplicar cambios personalizados al contenido estático generado antes de empaquetar la aplicación para su implementación. El proceso _build :generate_genera código, aplica parches y genera contenido estático. El proceso _build:transfer_ transfiere el código generado y el contenido estático al destino final. Ver [enlaces de aplicaciones](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/app/properties/hooks-property).<!--MAGECLOUD-2363-->

- ![Icono de corrección](../../assets/fix.svg) **Comprobaciones de configuración del entorno**: se ha mejorado la validación de la configuración del entorno para advertir a los clientes sobre las incompatibilidades de la versión y los errores de configuración antes de crear e implementar Adobe Commerce en la infraestructura en la nube.

   - Se agregó validación específica de la versión para identificar valores y variables de entorno no compatibles u obsoletos.<!--MAGECLOUD-2183-->

   - Se ha añadido una comprobación de compatibilidad de Elasticsearch para advertir a los usuarios sobre los problemas de configuración de Elasticsearch. Ahora, la implementación falla si la versión del servicio Elasticsearch en el servidor es incompatible con Adobe Commerce. Anteriormente, la implementación se realizaba correctamente incluso si la versión de Elasticsearch era incompatible, lo que causaba problemas con el catálogo de productos después de la implementación del sitio.<!--MAGECLOUD-2389-->

     Puede resolver la incompatibilidad [enviando un ticket de soporte](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/deploy/best-practices) para actualizar Elasticsearch a una versión compatible o cambiar la configuración de Adobe Commerce para especificar una versión compatible del cliente PHP de Elasticsearch.

      - Para Adobe Commerce versión 2.1.x a 2.2.2, actualice Elasticsearch a la versión 2.4.

      - Para la versión 2.2.3 y posteriores de Adobe Commerce, actualice Elasticsearch a la versión 5.2.

      - Si tiene Elasticsearch 1.x o 2.x y no desea actualizar, actualice el requisito de la versión del cliente Adobe Commerce Elasticsearch PHP en composer.json a `"elasticsearch/elasticsearch": "~2.0"`.

   - Se ha mejorado la validación de las variables de entorno para identificar las opciones de configuración que pueden provocar conflictos durante las fases de generación, implementación y posterior a la implementación. Por ejemplo, se muestra un mensaje de advertencia durante el proceso de instalación y actualización si la configuración global para la implementación de contenido estático entra en conflicto con la configuración de la fase de compilación o implementación.<!--MAGECLOUD-2156-->

- ![icono de corrección](../../assets/fix.svg) **Actualizaciones de variables de entorno**—Se han cambiado las siguientes variables de entorno:

   - **[Variable global SKIP_HTML_MINIFICATION](../environment/variables-global.md#skip_html_minification)**: se ha cambiado el valor predeterminado a `true` para habilitar la minificación de contenido de HTML bajo demanda, lo que minimiza el tiempo de inactividad al implementar en los entornos de ensayo y producción. Esta configuración es necesaria para implementaciones sin tiempo de inactividad.<!--MAGECLOUD-2435-->

   - **[CLEAN_STATIC_FILES implementa la variable](../environment/variables-deploy.md#clean_static_files)**: se ha agregado la capacidad de administrar el procesamiento de archivos estáticos limpios para el contenido estático generado durante la fase de compilación en función de la configuración de la variable de entorno CLEAN_STATIC_FILES. Anteriormente, los archivos de contenido estático generados durante la fase de compilación siempre se limpiaban.<!--MAGECLOUD-1506-->

- ![icono de corrección](../../assets/fix.svg) **Registro**—Se han realizado los siguientes cambios para mejorar los mensajes de registro y reducir el tamaño del registro:

   - Las entradas del registro de errores de implementación ahora incluyen el resultado del comando de las operaciones que provocan los errores aunque la configuración del entorno no especifique el registro de nivel de depuración. Ver [`MIN_LOGGING_LEVEL`](../environment/variables-global.md#min_logging_level).<!--MAGECLOUD-2489-->

   - Se agregó el registro de los errores de implementación que se producen cuando las fábricas generadas requeridas por algunas extensiones no se pueden generar correctamente porque el sistema de archivos está en un estado de solo lectura.<!--MAGECLOUD-2209-->

   - Se ha reducido el tamaño del registro de implementación y se han corregido los problemas de formato causados por los comandos de instalación que usan la barra de progreso interactiva.<!--MAGECLOUD-2402-->

   - Se eliminó la información detallada innecesaria y se actualizaron los niveles de prioridad de algunas instrucciones de registro.<!--MAGECLOUD-2227-->

- ![icono de corrección](../../assets/fix.svg) **correcciones específicas de Cron**—

   - Se ha cambiado la configuración predeterminada del trabajo cron para la duración del historial de 3d (4320 min) a 1h (60 min) para evitar problemas de rendimiento y errores de implementación que pueden producirse cuando la cola cron se llena demasiado rápido.<!--MAGECLOUD-2427-->

   - Se ha mejorado el proceso de administración de trabajos de cron durante la fase de implementación para evitar bloqueos de bases de datos y otros problemas críticos. Ahora, todos los trabajos cron se detienen durante la fase de implementación y se reinician una vez finalizada la implementación.<!--MAGECLOUD-2445-->

   - Se ha corregido un problema con el mecanismo de bloqueo para programar consumidores iniciado por los trabajos cron en las versiones 2.2.0 y posteriores de Adobe Commerce para evitar que los trabajos cron inicien consumidores duplicados.<!--MAGECLOUD-2464-->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema con el [proceso de compresión de contenido estático](../environment/variables-intro.md) (`gzip`) que causaba errores de `not overwritten` y `no such file or directory` al hacer referencia al archivo comprimido durante el proceso de implementación.<!-- MAGECLOUD-2182-->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que impedía que el comando `php ./vendor/bin/ece-tools config:dump` quitara secciones redundantes del archivo `config.php` durante el proceso de volcado si no se especificaba la configuración regional del almacén. Ahora puede mover fácilmente los archivos de configuración entre entornos. Después de actualizar a `ece-tools` v2002.0.13, vuelva a generar los archivos `config.php` más antiguos con el comando `config:dump` mejorado. Consulte [Administración de configuración para la configuración del almacén](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure-store/store-settings).<!--MAGECLOUD-2444-->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que provocaba un error durante la fase de implementación si la configuración de ruta del archivo `.magento/routes.yaml` redirige de un dominio [apex](https://blog.cloudflare.com/zone-apex-naked-domain-root-domain-cname-supp/) a un dominio `www`.<!--MAGECLOUD-2556-->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema con la opción `_merge` de la variable [`SEARCH_CONFIGURATION`](../environment/variables-deploy.md#search_configuration) que causaba resultados de combinación incorrectos si no se incluye el parámetro `engine` en el archivo de configuración `.magento.env.yaml` actualizado. Ahora, la operación de combinación sobrescribe correctamente sólo los valores especificados en el `.magento.env.yaml` actualizado sin que sea necesario establecer el parámetro `engine`.<!--MAGECLOUD-2520-->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema de configuración de Redis que habilitaba incorrectamente el bloqueo de sesión para Adobe Commerce en las versiones 2.2.1 y posteriores de la infraestructura en la nube, lo que puede provocar un rendimiento lento y tiempos de espera. Ahora, el bloqueo de sesión está deshabilitado de forma predeterminada. El problema se debía a un cambio en el comportamiento predeterminado del parámetro `disable_locking` introducido en la versión 1.3.4 del paquete del controlador de sesión de Redis. Consulte [paquete colinmollenhour/php-redis-session-abstract](https://github.com/colinmollenhour/php-redis-session-abstract).<!-- MAGECLOUD-2515-->

## v2002.0.12

- ![nuevo icono](../../assets/new.svg) **Docker Compose para Cloud**—Se agregó un comando—`docker:build`—para generar una configuración de [Docker Compose](https://developer.adobe.com/commerce/cloud-tools/docker/configure/) desde el repositorio de Cloud `ece-tools`.<!-- MAGECLOUD-2250 -->

- ![nuevo icono](../../assets/new.svg) **Cambiar configuración regional**—Ahora puede cambiar la configuración regional del almacén sin exportar e importar el proceso de configuración. Mientras la aplicación se encuentra en Producción y SCD_ON_DEMAND está habilitado, las opciones de configuración regional de almacén y administrador están disponibles.<!-- MAGECLOUD-2019 -->

- ![nuevo icono](../../assets/new.svg) <!-- MAGECLOU-1998 -->**Mapa del sitio y robots**: se ha creado un [flujo de trabajo](../store/robots-sitemap.md) para agregar un archivo de `robots.txt` y generar un archivo de `sitemap.xml` para una sola configuración de dominio sin necesidad de realizar un cambio en la infraestructura.

- ![nuevo icono](../../assets/new.svg) **asistentes**—Se agregaron dos [asistentes](../deploy/smart-wizards.md) para ayudarle con la configuración de la nube:<!-- MAGECLOUD-1910 -->

   - `ideal-state`: configure el estado ideal para un tiempo de inactividad mínimo en la implementación

   - `master-slave`: configurar el equilibrio de carga para la base de datos y Redis

- ![nuevo icono](../../assets/new.svg) **Actualización del módulo**—Se agregó un comando de nube—`module:refresh`—para habilitar módulos que se deshabilitaron o no se habilitaron explícitamente, de manera similar a como se hace automáticamente durante una compilación.<!-- MAGECLOUD-1521 -->

- ![nuevo icono](../../assets/new.svg) agregó la capacidad de elegir combinar o sobrescribir la configuración de los servicios mediante la opción `_merge` en las configuraciones de [CACHE](../environment/variables-deploy.md#cache_configuration), [SESSION](../environment/variables-deploy.md#session_configuration), [QUEUE](../environment/variables-deploy.md#queue_configuration) y [SEARCH](../environment/variables-deploy.md#search_configuration).<!-- MAGECLOUD-2105 -->

- ![nuevo icono](../../assets/new.svg) **archivo de muestra de configuración del entorno**—Hemos agregado un archivo de muestra `.magento.env.yaml` al paquete ECE-Tools que incluye una descripción detallada y los posibles valores para cada variable de entorno.<!-- MAGECLOUD-1908 -->

   - También agregamos una validación profunda para la configuración de `.magento.env.yaml` que evita errores en el proceso de implementación causados por valores inesperados. Cuando se produce un error, ahora recibe un mensaje de error detallado que comienza con: `Environment configuration is not valid. Please correct .magento.env.yaml file with next suggestions:`<!-- MAGECLOUD-1907 -->

- ![nuevo icono](../../assets/new.svg) agregó las siguientes [**variables de entorno**](../environment/variables-intro.md):

   - Ahora puede definir varias configuraciones regionales para cada tema mediante la nueva variable de entorno [SCD_MATRIX](../environment/variables-deploy.md#scd_matrix), que reduce la cantidad de archivos de tema que se van a implementar.<!-- MAGECLOUD-1501 -->

   - Se agregó la variable de entorno [DATABASE_CONFIGURATION](../environment/variables-deploy.md#database_configuration) para personalizar las conexiones de base de datos para la implementación.<!-- MAGECLOUD-2047 -->

   - La nueva variable [MIN_LOGGING_LEVEL](../environment/variables-global.md#min_logging_level) anula el nivel de registro mínimo de todas las secuencias de salida sin realizar cambios en el código.<!-- MAGECLOUD-2129 -->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que causaba tiempo de inactividad entre la fase de implementación y la posterior a la implementación. Ahora, la fase posterior a la implementación comienza _inmediatamente_ después de que finalice la fase de implementación.

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que no limpiaba de la programación los trabajos de cron correctos, los que tenían `status = success`.<!-- MAGECLOUD-2268 -->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema con el vínculo `post_deploy` que borraba la caché en la fase de implementación en lugar de en la fase posterior a la implementación del proyecto.<!-- MAGECLOUD-2113 -->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que se producía al usar SCD con varias configuraciones regionales, que generaba el mismo archivo `js-translation.json` para cada configuración regional.<!-- MAGECLOUD-2034 -->

- ![icono de corrección](../../assets/fix.svg) optimizó el comando `db:dump` en el paquete `ece-tools` para evitar bloquear tablas y aumentar la velocidad.<!-- MAGECLOUD-2033 -->

## v2002.0.11

>[!NOTE]
>
>La versión 2002.0.11 de ECE-Tools es necesaria para la compatibilidad con la versión 2.2.4.

- ![nuevo icono](../../assets/new.svg) **Configuración de conexiones de solo lectura a nodos no maestros**—Esta versión agrega la capacidad de configurar una conexión de solo lectura a un nodo no maestro para recibir tráfico de solo lectura (para [MariaDB](../environment/variables-deploy.md#mysql_use_slave_connection)).<!--MAGECLOUD-143 -->[Redis](../environment/variables-deploy.md#redis_use_slave_connection) y para <!--MAGECLOUD-143 -->

- ![nuevo icono](../../assets/new.svg) **Asistente para configuración**—Se ha agregado un asistente para comprobar la configuración de la implementación de contenido estático. Ver [asistentes inteligentes](../deploy/smart-wizards.md).<!--MAGECLOUD-1910 -->

- ![nuevo icono](../../assets/new.svg) **Compatibilidad con la consola Symfony**—Se ha agregado compatibilidad con la consola Symfony 4 con Adobe Commerce 2.3.<!-- MAGECLOUD-1966 -->

- ![icono de corrección](../../assets/fix.svg) **Optimizaciones de programación Cron**: se mejoró la administración de colas y el registro mejorado para ayudar a depurar los problemas relacionados con cron.<!-- MAGECLOUD-1607 -->

- ![icono de corrección](../../assets/fix.svg) La validación de la implementación falla si un valor de `ADMIN_EMAIL` o `ADMIN_USERNAME` es el mismo que el de una cuenta de administrador existente.<!-- MAGECLOUD-1221 -->

- ![icono de corrección](../../assets/fix.svg) eliminó la compatibilidad con SOLR para las versiones 2.2.x. Las versiones 2.1.x conservan la capacidad de habilitar SOLR.<!-- MAGECLOUD-1282 -->

- ![icono de corrección](../../assets/fix.svg) La primera instalación de los entornos de ensayo y producción de un proyecto PRO ahora incluye diferentes prefijos de índice para Elasticsearch a fin de evitar posibles conflictos al identificar registros que pertenecen a cada entorno.<!-- MAGECLOUD-1489 -->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que interrumpió la fase de compilación de la arquitectura heredada durante la implementación de contenido estático.<!-- MAGECLOUD-2021 -->

- ![Icono de correcciones](../../assets/fix.svg) **Mejoras específicas de Cron**—Se ha vuelto a trabajar la implementación de Cron:<!-- MAGECLOUD-1607 -->

   - Se ha corregido un problema que provocaba que la cola cron se llenara rápidamente. Ahora borra los trabajos obsoletos de cron de una manera más confiable.

   - Se ha reorganizado la secuencia de trabajos cron para que todos los trabajos de subprocesos independientes se inicien antes del grupo general.

   - Se ha mejorado el registro para ayudar mejor a depurar los problemas de cron.

   - **NOTA**: Esta versión resuelve muchos problemas relacionados con cron. Si actualmente usas algunos parches relacionados con cron en _m2-hotfixes_, elimínalos.

- ![icono de corrección](../../assets/fix.svg) **mejoras específicas de SCD**—

   - Puede usar las variables de entorno `VERBOSE_COMMANDS` y `SCD_COMPRESSION_LEVEL` durante las fases _build_ y de_ploy.<!-- MAGECLOUD-1819 -->

   - Se ha corregido un problema que ocasionaba que la implementación fallara con un error aleatorio al encontrar un valor inesperado para la variable de entorno `SCD_COMPRESSION_LEVEL`. Se ha mejorado la validación de la configuración para proporcionar notificaciones significativas. Vea [`SCD_COMPRESSION_LEVEL`](../environment/variables-build.md#scd_compression_level) para obtener los valores aceptables.<!-- MAGECLOUD-2043 -->

   - Se corrigió el comportamiento del flujo de configuración de la variable de entorno `SCD_COMPRESSION_LEVEL` para que las invalidaciones funcionen según lo esperado.<!-- MAGECLOUD-2044 -->

   - Se ha corregido un problema que impedía la configuración de la variable de entorno `SCD_THREADS` en el archivo `.magento.env.yaml` _implementar_ fase.<!-- MAGECLOUD-2046 -->

## v2002.0.10

- ![nuevo icono](../../assets/new.svg) **Implementación de contenido estático (SCD)**: hay un nuevo proceso de implementación alternativo para generar contenido estático cuando se solicite (a petición). Esto reduce el tiempo de inactividad y mejora la administración de la caché al generar los recursos más críticos.<!-- MAGECLOUD-1285 -->

   - **Nueva variable de entorno**: se agregó la variable de entorno global `SCD_ON_DEMAND` para generar contenido estático cuando se solicita.<!-- MAGECLOUD-1738 -->

   - **Vínculo posterior a la implementación**: Se ha agregado un vínculo `post_deploy` para el archivo `.magento.app.yaml` que borra la caché y carga previamente (calienta) la caché _después de_ de que el contenedor empiece a aceptar conexiones. Solo está disponible para proyectos profesionales que contienen entornos de ensayo y producción en [!DNL Cloud Console] y para proyectos iniciales. Aunque no es obligatorio, funciona en conjunto con la variable de entorno `SCD_ON_DEMAND`.<!-- MAGECLOUD-1788 -->

- ![nuevo icono](../../assets/new.svg) **Optimización**: se optimizó el movimiento o la copia de archivos durante la implementación para mejorar la velocidad de implementación y reducir las cargas en el sistema de archivos.<!-- MAGECLOUD-1842 -->

- ![nuevo icono](../../assets/new.svg) **Registro de implementación**—Se ha agregado la capacidad de habilitar los controladores Syslog y Graylog Extended Log Format (GELF) para generar registros durante el proceso de implementación. Consulte [Controladores de registro](../environment/log-handlers.md).<!-- MAGECLOUD-1751 -->

- ![nuevo icono](../../assets/new.svg) agregó las siguientes [**variables de entorno**](../environment/variables-intro.md):

   - `CRYPT_KEY`: proporcione una clave criptográfica a otro entorno al mover una base de datos.<!-- MAGECLOUD-1556 -->

   - `SKIP_HTML_MINIFICATION`—_Variable de entorno global_ que omite la copia de los archivos de vista estática en el directorio `var/view_preprocessed` y genera HTML minimizado cuando se solicita.<!-- MAGECLOUD-1621 and MAGECLOUD-1736-->

   - `SCD_ON_DEMAND`—_Variable de entorno global_ para generar contenido estático cuando se solicita.<!-- MAGECLOUD-1738 -->

   - `WARM_UP_PAGES`: puede enumerar las páginas que se utilizarán para precargar la caché. Disponible en las nuevas [variables posteriores a la implementación](../environment/variables-post-deploy.md).

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que implicaba que un parche aplicado localmente rompía la implementación en una instancia. Ahora, ECE-Tools puede detectar que se ha aplicado un parche.<!-- MAGECLOUD-982 -->

- ![Icono de corrección](../../assets/fix.svg) Se ha corregido un conflicto entre el paquete de JavaScript y la funcionalidad GZIP. Ahora estas características funcionan correctamente juntas.<!-- MAGECLOUD-1735 -->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que causaba que fallaran los comandos CLI de ECE-Tools al usar versiones anteriores de PHP 7.0.x.<!-- MAGECLOUD-1744 -->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que impedía la implementación de contenido estático con la estrategia compacta en varios subprocesos.<!-- MAGECLOUD-1822 -->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema con el bloqueo de sesión de Redis que provocaba un retraso en el inicio de sesión del administrador. Además, la corrección está disponible para 2.1.x.<!-- MAGECLOUD-1853 -->

## v2002.0.9

>[!NOTE]
>
>Debe [actualizar el metapaquete de Adobe Commerce en la infraestructura en la nube](../dev-tools/install-package.md#update-the-metapackage) para obtener esta y todas las actualizaciones futuras.

- ![nuevo icono](../../assets/new.svg) **ece-tools**: el paquete `ece-tools` ahora es compatible con Adobe Commerce 2.1.x.<!-- MAGECLOUD-1086 -->

- ![nuevo icono](../../assets/new.svg) **Configuración de Redis**: ahora puede [configurar la página de Redis](../environment/variables-deploy.md#cache_configuration) y la caché predeterminada y el almacenamiento de sesión de Redis mediante una variable de entorno.<!-- MAGECLOUD-1552 -->

- ![nuevo icono](../../assets/new.svg) **Mejoras en los servicios Search, AMQP y Redis**: hemos unificado el flujo de configuración del servicio para que ahora se comporte de la misma manera para todos los servicios. Ya no se admite la edición manual del archivo `env.php` para configurar servicios. Debe usar variables de entorno o el archivo `.magento.env.yaml` en su lugar.<!-- MAGECLOUD-1437 -->

- ![icono de corrección](../../assets/fix.svg) **variables de entorno**—

   - El uso de `env:STATIC_CONTENT_THREADS` estaba obsoleto y se eliminará en una versión futura. Use [SCD_THREADS](../environment/variables-deploy.md#scd_threads) en su lugar.<!-- MAGECLOUD-1507 -->

   - La variable de entorno `STATIC_CONTENT_EXCLUDE_THEMES` estaba en desuso. Debe usar la variable de entorno `SCD_EXCLUDE_THEMES` en su lugar.<!-- MAGECLOUD-1640 -->

- ![Icono de corrección](../../assets/fix.svg) **Registro**: se ha simplificado el registro para las operaciones de aplicación de parches integradas.<!-- MAGECLOUD-1674 -->

- ![icono de corrección](../../assets/fix.svg) Se quitó la compatibilidad con el modo `developer` y la variable de entorno `APPLICATION_MODE` porque estaban causando un comportamiento inesperado.<!-- MAGECLOUD-1615 -->

- ![icono de corrección](../../assets/fix.svg): se ha corregido un problema que causaba errores de implementación de contenido estático relacionados con Redis. Ahora, la implementación de contenido estático de subprocesos múltiples se ejecuta según lo diseñado.<!-- MAGECLOUD-1630 -->

- ![icono de corrección](../../assets/fix.svg) Hemos corregido un problema que impedía a los usuarios guardar modificaciones en los campos de configuración del administrador, que se marcan como confidenciales después de ejecutar el comando `app:config:dump`.<!-- MAGECLOUD-1175 -->

- ![icono de corrección](../../assets/fix.svg) Hemos agregado compatibilidad con una versión anterior de `symfony/yaml` para solucionar conflictos con algunos paquetes, que aún no son compatibles con la versión más reciente.<!-- MAGECLOUD-1674 -->

## v2002.0.8

>[!NOTE]
>
>Combinamos `vendor/magento/ece-patches` con `vendor/magento/ece-tools` en esta versión. Ya no necesita actualizar el paquete `vendor/magento/ece-patches` por separado.

**Nuevas características:**

- **Registro mejorado**<!-- MAGECLOUD-1253 -MAGECLOUD-1495 -->
   - Hemos mejorado la mensajería de registro para proporcionar mejores explicaciones cuando el proceso de generación o implementación anula una variable de entorno.
   - Ahora puede ver el progreso de la instalación y actualización en tiempo real. Siga el archivo `install_update.log` para ver el progreso. Por ejemplo,

     ```bash
     tail -f var/log/install_upgrade.log
     ```

- **Nuevo comando cron**: ahora puede desbloquear trabajos cron atascados específicos en lugar de detener y volver a iniciar todos con el comando [`cron:unlock`](https://support.magento.com/hc/en-us/articles/360033099451). No disponible en 2.1.<!-- MAGECLOUD-1367 -->

- **Archivo de configuración unificado**: Ahora puede configurar las fases de generación e implementación mediante un archivo [`.magento.env.yaml`](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/env/configure-env-yaml).<!-- MAGECLOUD-1369 -->

- **Archivos de configuración de copia de seguridad**: el proceso de implementación ahora crea automáticamente una copia de seguridad de los archivos de configuración `app/etc/env.php` y `app/etc/config.php` después de la implementación. También agregamos un [nuevo comando CLI](https://support.magento.com/hc/en-us/articles/360033182871) para restaurar estos archivos de configuración a partir de una copia de seguridad.<!-- MAGECLOUD-1372 -->

- **Solucionar problemas de errores de validación**: Hemos cambiado el comando que debe usar para resolver los errores de validación cuando `config.php` no contiene datos suficientes para la implementación de contenido estático. Anteriormente, el mensaje de error le indicaba que ejecutara `bin/magento app:config:dump`. Ahora debe ejecutar `php ./vendor/bin/ece-tools config:dump`.<!-- MAGECLOUD-1491 -->

- **Nuevas variables de entorno**: Ahora puede usar variables de entorno para conectar los servicios personalizados [search](../environment/variables-deploy.md#search_configuration) y [AMQP-based](../environment/variables-deploy.md#queue_configuration) a su sitio.<!-- MAGECLOUD-1410 -->

- Hemos implementado parches inteligentes. Ahora el paquete aplica parches basados no en Adobe Commerce en la versión de infraestructura en la nube, sino en la versión de paquete parcheado.<!--MAGECLOUD-1090-->

**Problemas resueltos:**

- Se ha corregido un problema de registro que causaba errores de compilación.<!-- MAGECLOUD-1162 -->

- Se corrigió un problema que ocasionaba excepciones de tiempo de espera al ejecutar implementaciones en modo interactivo.<!-- MAGECLOUD-1389 -->

- Se ha corregido un problema que ocasionaba errores al utilizar la estrategia compacta para la generación de contenido estático. No disponible en 2.1.<!-- MAGECLOUD-1446 MAGECLOUD-1485-->

- Se ha corregido un problema que impedía que el script de implementación identificara correctamente los entornos de ensayo y producción.<!-- MAGECLOUD-1493 -->

- Se ha corregido un problema que causaba que los problemas de red interrumpieran las conexiones de base de datos y causaran errores durante el proceso de instalación y actualización.<!-- MAGECLOUD-1520 -->

- Se ha corregido un problema que impedía exportar los archivos de configuración con `app:config:dump` más de una vez. No disponible en 2.1.<!--  MAGECLOUD-1567  -->

- Se ha corregido un problema de _bloqueo_ en la sesión de Redis que provocaba un retraso de inicio de sesión de _Admin_. No disponible en 2.1.<!--  MAGECLOUD-1582  -->

- Se ha corregido un problema de implementación relacionado con el control de versiones que causaba un conflicto con otros módulos de aplicación de parches basados en Compositor.<!-- MAGECLOUD-1450 -->

- Se ha corregido un problema que causaba problemas de memoria PHP durante la importación.<!-- MAGECLOUD-1310 -->

- Se ha eliminado el parche; se ha corregido un error en `colinmollenhour/credis` v1.6 para habilitar la compatibilidad con Adobe Commerce en la infraestructura en la nube 2.2.1. No disponible en 2.1.<!-- MAGECLOUD-1033 -->

## v2002.0.7

**Problemas resueltos:**

- Eliminamos los enlaces simbólicos de `var/view_preprocessed` para solucionar un problema que causaba conflictos de minificación de JavaScript.<!-- MAGECLOUD-1454 -->

## v2002.0.6

**Problemas resueltos:**

- Se ha corregido un problema que causaba `gzip` errores cuando el nombre de un archivo o directorio contiene espacios.<!-- MAGECLOUD-1413 -->

- Se ha corregido un problema que impedía que los scripts de implementación reconocieran y habilitaran correctamente las dependencias de módulo.<!-- MAGECLOUD-1424 -->

## v2002.0.5

**Nuevas características:**

- **Configurar un consumidor de cron con una variable de entorno**: ahora puede configurar consumidores de cron mediante la nueva variable de entorno `CRON_CONSUMERS_RUNNER`.

- **Análisis de configuración**: ahora analizamos componentes críticos durante el proceso de compilación/implementación y detenemos el proceso si el análisis falla, lo que evita un tiempo de inactividad innecesario debido a que el sitio está en modo de mantenimiento.

- **Generar/implementar notificaciones**: Hemos agregado un archivo de configuración que puede usar para [configurar notificaciones de Slack o por correo electrónico](../environment/set-up-notifications.md) para acciones de compilación/implementación en todos sus entornos.

- **Compresión de contenido estático**: Ahora comprimimos contenido estático con [gzip](https://www.gnu.org/software/gzip/) durante las fases de compilación e implementación. Esta compresión, junto con la compresión de Fastly, ayuda a reducir el tamaño de su tienda y aumentar la velocidad de implementación. Si es necesario, puede deshabilitar la compresión mediante una [opción de compilación](../environment/variables-build.md) o [implementar variable](../environment/variables-deploy.md). Consulte los siguientes temas para obtener más información:

   - [Variables de entorno de aplicación](../application/variables-property.md)

   - [Rendimiento de implementación de contenido estático](../deploy/static-content.md)

   - [Proceso de implementación](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/deploy/best-practices)

- **Administración de configuración**: Ahora generamos automáticamente un archivo `app/etc/config.php` en su repositorio Git durante la fase de compilación si aún no existe. El archivo generado automáticamente solo incluye una lista de módulos y extensiones. Si el archivo ya existe, la fase de compilación continúa con normalidad. Si sigue [Configuration Management](../store/store-settings.md) más adelante, los comandos actualizarán el archivo sin necesidad de realizar pasos adicionales. Consulte [Proceso de implementación](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/deploy/best-practices) para obtener más información.

- **Volcados de base de datos**: Hemos agregado un comando CLI `magento/ece-tools` para crear volcados de base de datos en todos los entornos. Para entornos de producción de planificación profesional, este comando solo volca desde uno de los tres nodos de alta disponibilidad, por lo que es posible que no se copien los datos de producción escritos en un nodo diferente durante el volcado. Se recomienda poner la aplicación en modo de mantenimiento antes de hacer un volcado de la base de datos en entornos de producción. Consulte [Administración de copias de seguridad](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/storage/snapshots) para obtener más información.

- **Se eliminaron las limitaciones del intervalo Cron**: el intervalo cron predeterminado para todos los entornos aprovisionados en las regiones us-3, eu-3 y ap-3 es de 1 minuto. El intervalo cron predeterminado en todas las demás regiones es de 5 minutos para entornos de integración profesional y 1 minuto para entornos de ensayo y producción profesionales. Para modificar los trabajos cron existentes, edite la configuración en `.magento.app.yaml` o cree un vale de soporte para los entornos de Producción/Ensayo. Consulte [Configurar trabajos cron](../application/crons-property.md#set-up-cron-jobs) para obtener más información.

**Problemas resueltos:**

- Se ha corregido un problema que causaba largos tiempos de implementación debido a que el proceso de implementación invocaba la operación `cache-clean` antes de la implementación de contenido estático.<!-- MAGECLOUD-1327 -->

- Se corrigió un problema que ocasionaba errores durante el paso de implementación de generación de contenido estático en entornos de producción.<!-- MAGECLOUD-1322 -->

- Se ha corregido un problema que impedía que algunos comandos de `magento/ece-tools` registraran la salida en `stderr`.<!-- MAGECLOUD-1264 -->

- Se ha corregido un problema que impedía que los valores de URL base en `env.php` se actualizaran en ramas bifurcadas.<!-- MAGECLOUD-1242 -->

- Se ha corregido un problema que hacía que el comando `magento setup:install` agregara un prefijo no seguro (`http://`) a las direcciones URL base seguras.<!-- MAGECLOUD-1171 -->

- Se ha corregido un problema que impedía que los errores de revisión causaran errores de implementación.<!-- MAGECLOUD-1170 -->

- Se ha corregido un problema que impedía que `ece-tools` detuviera la ejecución y generara una excepción si no se podían aplicar parches.<!-- MAGECLOUD-1152 -->

- Se corrigió un problema que ocasionaba errores al cargar la tienda después de habilitar la minificación de HTML en el administrador.<!-- MAGECLOUD-1138 -->

## v2002.0.4

**Problemas resueltos:**

- Ahora puede [restablecer manualmente los trabajos cron atascados](https://support.magento.com/hc/en-us/articles/360033099451) mediante un comando CLI en todos los entornos a través del acceso SSH. El proceso de implementación restablece automáticamente los trabajos cron.<!-- MAGECLOUD-1355 -->

## v2002.0.3

**Problemas resueltos:**

- Se ha corregido un problema que provocaba que las páginas agotaran el tiempo de espera porque Redis tardaba demasiado en leer y escribir. Ahora puede usar el parámetro `disable_locking` en las configuraciones de Redis para evitar este problema.<!-- MAGECLOUD-1311 -->

## v2002.0.2

**Problemas resueltos:**

- El proceso de configuración de [!DNL RabbitMQ] ahora obtiene automáticamente todos los parámetros necesarios.<!-- MAGECLOUD-1246 -->

## v2002.0.1

**Nuevas características:**

- Adobe Commerce en la infraestructura de la nube ahora admite ámbitos y [estrategias de implementación de contenido estático](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-strategy). Se ha agregado el parámetro `–s` con la configuración predeterminada `quick` para la estrategia de implementación de contenido estático. Puede usar la variable de entorno [SCD_STRATEGY](../environment/variables-deploy.md) para personalizar y usar estas estrategias con sus acciones de compilación e implementación. Esta variable admite las opciones `standard`, `quick` o `compact`. Si selecciona `compact`, anulamos el valor `STATIC_CONTENT_THREADS` por `1`, lo que puede ralentizar la implementación, especialmente en entornos de producción. No disponible en 2.1.<!--- MAGECLOUD-1057 -->

- Hemos creado un archivo de registro en entornos para capturar y compilar acciones de compilación e implementación. El archivo `var/log/cloud.log` se encuentra en el directorio raíz de la aplicación.<!--- MAGECLOUD-1014 & MAGECLOUD-1023 -->

**Problemas resueltos:**

- Se ha refactorizado el paquete `ece-tools` para que sea compatible con Adobe Commerce en la infraestructura en la nube 2.2.0 y superior.<!-- MAGECLOUD-919 & MAGECLOUD-1030 -->

- Se ha corregido un problema que impedía que `ece-tools` detuviera la ejecución y generara una excepción si no se podían aplicar parches.<!-- MAGECLOUD-1186 -->

- Se ha corregido un problema que causaba que se generaran excepciones cuando la compilación de inyección de dependencia (id) se omite durante las compilaciones.<!-- MAGECLOUD-1047 & MAGECLOUD-1049 -->

- Se ha corregido un problema que hacía que el proceso de implementación sobrescribiera las configuraciones personalizadas de Redis en el archivo `env.php`.<!-- MAGECLOUD-1019 -->

- Se ha corregido un problema que causaba bucles de redireccionamiento debido a deshabilitado por el administrador seguro predeterminado.<!-- MAGECLOUD-1020 -->

## v2002.0.0

>[!WARNING]
>
>Este paquete ya no es compatible con otras versiones de Adobe Commerce en la infraestructura en la nube y **no debería** usarse.

### Versión inicial

Versión inicial de `ece-tools` para Adobe Commerce en la infraestructura en la nube 2.2.0.
