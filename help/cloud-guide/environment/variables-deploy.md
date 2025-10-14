---
title: Implementación de variables
description: Consulte la lista de variables de entorno que controlan las acciones en la fase de implementación de Adobe Commerce en la infraestructura en la nube.
feature: Cloud, Configuration, Cache, Deploy, SCD, Storage, Search
recommendations: noDisplay, catalog
role: Developer
exl-id: 980ec809-8c68-450a-9db5-29c5674daa16
source-git-commit: 5fc2082ca2aae8a1466821075c01ce756ba382cc
workflow-type: tm+mt
source-wordcount: '2502'
ht-degree: 0%

---

# Implementación de variables

Las siguientes variables _deploy_ controlan acciones en la fase de implementación y pueden heredar y anular valores de las [variables globales](variables-global.md). Inserte estas variables en la fase `deploy` del archivo `.magento.env.yaml`:

```yaml
stage:
  deploy:
    DEPLOY_VARIABLE_NAME: value
```

Para obtener más información sobre cómo personalizar el proceso de generación e implementación:

- [Configuración de implementación](configure-env-yaml.md)
- [Proceso de implementación](../deploy/process.md)

## `CACHE_CONFIGURATION`

- **Predeterminado**—_No establecido_
- **Versión**: Adobe Commerce 2.1.4 y posterior

Configure la página Redis y el almacenamiento en caché predeterminado. Al establecer el parámetro `cm_cache_backend_redis`, debe especificar las opciones `server`, `port` y `database`.

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      frontend:
        default:
          backend: file
        page_cache:
          backend: file
```

{{merge-options}}

El siguiente ejemplo combina nuevos valores en una configuración existente:

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      _merge: true
      frontend:
        default:
          backend_options:
            database: 10
        page_cache:
          backend_options:
            database: 11
```

El siguiente ejemplo usa la [característica de precarga de Redis](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/redis/redis-pg-cache.html#redis-preload-feature) tal como se define en la _guía de configuración_:

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      _merge: true
      frontend:
        default:
          id_prefix: '061_'
          backend_options:
            preload_keys:
              - '061_EAV_ENTITY_TYPES:hash'
              - '061_GLOBAL_PLUGIN_LIST:hash'
              - '061_DB_IS_UP_TO_DATE:hash'
              - '061_SYSTEM_DEFAULT:hash'
```

Para usar un modelo [REDIS_BACKEND](#redis_backend) personalizado (no solo de la lista de permitidos), establezca la opción `_custom_redis_backend` en `true` para habilitar la validación correcta como en el siguiente ejemplo:

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      frontend:
        default:
          _custom_redis_backend: true
          backend: '\CustomRedisModel'
```

## `CLEAN_STATIC_FILES`

- **Predeterminado**—`true`
- **Versión**: Adobe Commerce 2.1.4 y posterior

Habilita o deshabilita la limpieza de [archivos de contenido estático](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html) generados durante la fase de compilación o implementación. Se recomienda usar el valor predeterminado _true_ en desarrollo.

- **`true`**: elimina todo el contenido estático existente antes de implementar el contenido estático actualizado.
- **`false`**: la implementación solo sobrescribe los archivos de contenido estático existentes si el contenido generado contiene una versión más reciente.

Si modifica el contenido estático mediante un proceso independiente, establezca el valor en _false_.

```yaml
stage:
  deploy:
    CLEAN_STATIC_FILES: false
```

Si no se limpian los archivos de vista estática antes de la implementación, pueden producirse problemas si se implementan actualizaciones en los archivos existentes sin quitar las versiones anteriores. Debido a las reglas [static file fallback](https://developer.adobe.com/commerce/frontend-core/guide/caching/#clean-static-files-cache), las operaciones de reserva pueden mostrar el archivo incorrecto si el directorio contiene varias versiones del mismo archivo.

## `CRON_CONSUMERS_RUNNER`

- **Predeterminado**—`cron_run = false`, `max_messages = 1000`
- **Versión**—Adobe Commerce 2.2.0 y posterior

Utilice esta variable de entorno para confirmar que las colas de mensajes se están ejecutando después de una implementación.

- `cron_run`: valor booleano que habilita o deshabilita el trabajo cron de `consumers_runner` (predeterminado = `false`).
- `max_messages`: un número que especifica el número máximo de mensajes que cada consumidor debe procesar antes de finalizar (predeterminado = `1000`). Puede establecer el valor en `0` para evitar que el consumidor finalice.
- `consumers`: una matriz de cadenas que especifica los consumidores que se van a ejecutar. Una matriz vacía ejecuta _todos_ los consumidores.

- `multiple_processes`: un número que especifica el número de procesos que se generarán para cada consumidor. Compatible con Commerce **2.4.4** o superior.

>[!NOTE]
>
>Para devolver una lista de la cola de mensajes `consumers`, ejecute el comando `./bin/magento queue:consumers:list` en el entorno remoto.

Matriz de ejemplo que ejecuta `consumers` específico y `multiple_processes` para que se genere para cada consumidor:

```yaml
stage:
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 1000
      consumers:
        - example_consumer_1
        - example_consumer_2
-     multiple_processes:
        example_consumer_1: 4
        example_consumer_2: 3
```

Ejemplo de una matriz vacía que ejecuta todos los `consumers`:

```yaml
stage:
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 1000
      consumers: []
```

De manera predeterminada, el proceso de implementación sobrescribe toda la configuración del archivo `env.php`. Consulte [Administrar colas de mensajes](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/message-queues/manage-message-queues.html) en la _Guía de configuración de Commerce_ para Adobe Commerce local.

## `CONSUMERS_WAIT_FOR_MAX_MESSAGES`

- **Predeterminado**—`false`
- **Versión**—Adobe Commerce 2.2.0 y posterior

Configure cómo `consumers` procesa los mensajes de la cola de mensajes eligiendo una de las siguientes opciones:

- `false`—`Consumers` procesan los mensajes disponibles en la cola, cierran la conexión TCP y finalizan. `Consumers` no espera a que otros mensajes entren en la cola, aunque el número de mensajes procesados sea menor que el valor `max_messages` especificado en la variable de implementación `CRON_CONSUMERS_RUNNER`.

- `true`—`Consumers` continúan procesando mensajes de la cola de mensajes hasta alcanzar el número máximo de mensajes (`max_messages`) especificado en la variable de implementación `CRON_CONSUMERS_RUNNER` antes de cerrar la conexión TCP y finalizar el proceso de consumidor. Si la cola se vacía antes de llegar a `max_messages`, el consumidor espera a que lleguen más mensajes.

>[!WARNING]
>
>Si usa trabajadores para ejecutar `consumers` en lugar de usar un trabajo cron, establezca esta variable como true.

```yaml
stage:
  deploy:
    CONSUMERS_WAIT_FOR_MAX_MESSAGES: false
```

## `CRYPT_KEY`

- **Predeterminado**—_No establecido_
- **Versión**: Adobe Commerce 2.1.4 y posterior

>[!WARNING]
>
>Establezca el valor `CRYPT_KEY` a través del archivo [!DNL Cloud Console] en lugar del archivo `.magento.env.yaml` para evitar exponer la clave en el repositorio de código fuente de su entorno. Consulte [Establecer variables de entorno y proyecto](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/project/overview.html#configure-environment).

Cuando mueve la base de datos de un entorno a otro sin un proceso de instalación, necesita la información criptográfica correspondiente. Adobe Commerce usa el valor de clave de cifrado establecido en [!DNL Cloud Console] como el valor `crypt/key` del archivo `env.php`.

## `DATABASE_CONFIGURATION`

- **Predeterminado**—_No establecido_
- **Versión**: Adobe Commerce 2.1.4 y posterior

Si ha definido una base de datos en la [propiedad de relaciones](../application/properties.md#relationships) del archivo `.magento.app.yaml`, puede personalizar las conexiones de base de datos para la implementación.

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      some_config: 'some_value'
```

{{merge-options}}

El siguiente ejemplo combina nuevos valores en una configuración existente:

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      some_config: 'some_new_value'
      _merge: true
```

Además, puede configurar un prefijo de tabla.

>[!WARNING]
>
>Si no utiliza la opción de combinación con el prefijo de tabla, debe proporcionar la configuración de conexión predeterminada o la implementación no superará la validación.

El ejemplo siguiente utiliza el prefijo de tabla `ece_` con la configuración de conexión predeterminada en lugar de utilizar la opción `_merge`:

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      connection:
        default:
          username: user
          host: host
          dbname: magento
          password: password
      table_prefix: 'ece_'
```

Salida de ejemplo:

```
MariaDB [main]> SHOW TABLES;
+-------------------------------------+
| Tables_in_main                      |
+-------------------------------------+
| ece_admin_passwords                 |
| ece_admin_system_messages           |
| ece_admin_user                      |
| ece_admin_user_session              |
| ece_adminnotification_inbox         |
| ece_amazon_customer                 |
| ece_authorization_rule              |
| ece_cache                           |
| ece_cache_tag                       |
| ece_captcha_log                     |
...
```

## `ELASTICSUITE_CONFIGURATION`

- **Predeterminado**—_No establecido_
- **Versión**—Adobe Commerce 2.2.0 y posterior

Conserva la configuración personalizada del servicio [!DNL Elastic Suite] entre implementaciones y la usa en la sección &quot;system/default/mile_elasticsuite_core_base_settings&quot; de la configuración principal de [!DNL Elastic Suite]. Si está instalado el paquete del compositor [!DNL Elastic Suite], se configura automáticamente.

```yaml
stage:
  deploy:
    ELASTICSUITE_CONFIGURATION:
      es_client:
        servers: 'remote-host:9200'
      indices_settings:
        number_of_shards: 1
        number_of_replicas: 0
```

>[!NOTE]
>
>En un clúster de ensayo/producción profesional que tiene tres nodos (o tres nodos de servicio en [Arquitectura escalable](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/architecture/scaled-architecture#service-tier)), `indices_settings` debe establecerse de la siguiente manera:
>
>```yaml
>           indices_settings:
>               number_of_shards: 3
>               number_of_replicas: 2
>```

{{merge-options}}

El siguiente ejemplo combina un nuevo valor con la configuración existente:

```yaml
stage:
  deploy:
    ELASTICSUITE_CONFIGURATION:
      indices_settings:
        number_of_shards: 3
        number_of_replicas: 2
      _merge: true
```

**Limitaciones conocidas**:

- Cambiar el motor de búsqueda a cualquier tipo distinto de `elasticsuite` provoca un error de implementación acompañado de un error de validación adecuado
- La eliminación del servicio de Elasticsearch provoca un error de implementación acompañado de un error de validación adecuado

>[!NOTE]
>
>Para obtener más información sobre cómo usar o solucionar problemas del complemento [!DNL Elastic Suite] con Adobe Commerce, consulte la [[!DNL Elastic Suite] documentación](https://github.com/Smile-SA/elasticsuite).

## `ENABLE_GOOGLE_ANALYTICS`

- **Predeterminado**—`false`
- **Versión**: Adobe Commerce 2.1.4 y posterior

Habilita y deshabilita Google Analytics al implementarlo en entornos de ensayo e integración. De forma predeterminada, Google Analytics solo es true para el entorno de producción. Establezca este valor en `true` para habilitar Google Analytics en los entornos de ensayo e integración.

- **`true`**: habilita Google Analytics en entornos de ensayo e integración.
- **`false`**: deshabilita Google Analytics en entornos de ensayo e integración.

Agregar la variable de entorno `ENABLE_GOOGLE_ANALYTICS` al escenario `deploy` en el archivo `.magento.env.yaml`:

```yaml
stage:
  deploy:
    ENABLE_GOOGLE_ANALYTICS: true
```

>[!NOTE]
>
>El proceso de implementación siempre habilita Google Analytics en entornos de producción.

## `FORCE_UPDATE_URLS`

- **Predeterminado**—`true`
- **Versión**: Adobe Commerce 2.1.4 y posterior

Al implementarse en entornos de ensayo y producción Pro o Starter, esta variable reemplaza las direcciones URL base de Adobe Commerce en la base de datos por las direcciones URL del proyecto especificadas por la variable [`MAGENTO_CLOUD_ROUTES`](variables-cloud.md). Use esta configuración para anular el comportamiento predeterminado de la variable de implementación [UPDATE_URLS](#update_urls), que se omite al implementar en entornos de ensayo o producción.

```yaml
stage:
  deploy:
    FORCE_UPDATE_URLS: true
```

## `LOCK_PROVIDER`

- **Predeterminado**—`file`
- **Versión**: Adobe Commerce 2.2.5 y posterior

El proveedor de bloqueo evita el inicio de trabajos cron y grupos cron duplicados. Usar el proveedor de bloqueos `file` en el entorno de producción. Los entornos iniciales y el entorno de integración Pro no usan la variable [MAGENTO_CLOUD_LOCKS_DIR](variables-cloud.md), por lo que `ece-tools` aplica el proveedor de bloqueo `db` automáticamente.

```yaml
stage:
  deploy:
    LOCK_PROVIDER: "db"
```

Consulte [Configurar el bloqueo](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/lock-provider.html) en la _Guía de instalación_.

## `MYSQL_USE_SLAVE_CONNECTION`

- **Predeterminado**—`false`
- **Versión**: Adobe Commerce 2.1.4 y posterior

>[!TIP]
>
>La variable `MYSQL_USE_SLAVE_CONNECTION` solo es compatible con Adobe Commerce en entornos de clúster de Cloud Infrastructure, Staging y Production Pro, y no con proyectos iniciales.

Adobe Commerce puede leer varias bases de datos de forma asincrónica. Configúrelo en `true` para que use automáticamente una conexión de _solo lectura_ con la base de datos para recibir tráfico de solo lectura en un nodo que no sea maestro. Esta conexión mejora el rendimiento mediante el equilibrio de carga, ya que solo un nodo administra el tráfico de lectura-escritura. Establezca el valor en `false` para quitar cualquier matriz de conexión de solo lectura existente del archivo `env.php`.

```yaml
stage:
  deploy:
    MYSQL_USE_SLAVE_CONNECTION: true
```

Cuando la variable `MYSQL_USE_SLAVE_CONNECTION` se establece en `true`, el parámetro `synchronous_replication` se establece en `true` de forma predeterminada en el archivo `env.php` en los entornos de ensayo y producción de Pro. Cuando `MYSQL_USE_SLAVE_CONNECTION` está establecido en `false`, el parámetro `synchronous_replication` no está configurado.

## `QUEUE_CONFIGURATION`

- **Predeterminado**—_No establecido_
- **Versión**: Adobe Commerce 2.1.4 y posterior

Utilice esta variable de entorno para conservar la configuración personalizada del servicio de cola entre implementaciones. Esta variable admite los protocolos AMQP (para RabbitMQ) y STOMP (para ActiveMQ Artemis). Por ejemplo, si prefiere usar un servicio de cola de mensajes existente en lugar de depender de la infraestructura de nube para crearlo, use la variable de entorno `QUEUE_CONFIGURATION` para conectarlo al sitio:

```yaml
stage:
  deploy:
    QUEUE_CONFIGURATION:
      amqp:
        host: test.host
        port: 1234
      amqp2:
        host: test.host2
        port: 12345
      mq:
        host: mq.host
        port: 1234
```

Para elementos ActiveMQ que utilizan el protocolo STOMP:

```yaml
stage:
  deploy:
    QUEUE_CONFIGURATION:
      stomp:
        host: activemq.host
        port: 61616
        user: username
        password: password
```

{{merge-options}}

El siguiente ejemplo combina nuevos valores en una configuración existente:

```yaml
stage:
  deploy:
    QUEUE_CONFIGURATION:
      _merge: true
      amqp:
        host: changed1.host
        port: 5672
      amqp2:
        host: changed2.host2
        port: 12345
      mq:
        host: changedmq.host
        port: 1234
```

## `REDIS_BACKEND`

- **Predeterminado**—`Cm_Cache_Backend_Redis`
- **Versión**: Adobe Commerce 2.3.0 y posterior

Especifica la configuración del modelo backend para la caché de Redis.

La versión 2.3.0 y posteriores de Adobe Commerce incluyen los siguientes modelos backend:

- `Cm_Cache_Backend_Redis`
- `\Magento\Framework\Cache\Backend\Redis`
- `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache`

El ejemplo muestra cómo establecer `REDIS_BACKEND`

```yaml
stage:
  deploy:
    REDIS_BACKEND: '\Magento\Framework\Cache\Backend\RemoteSynchronizedCache'
```

>[!NOTE]
>
>Si especifica `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache` como modelo back-end de Redis para habilitar la [caché L2](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html), `ece-tools` genera la configuración de caché automáticamente. Vea un ejemplo de [archivo de configuración](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html#configuration-example) en la _Guía de configuración de Adobe Commerce_. Para anular la configuración de caché generada, use la variable de implementación [CACHE_CONFIGURATION](#cache_configuration).

## `REDIS_USE_SLAVE_CONNECTION`

- **Predeterminado**—`false`
- **Versión**—Adobe Commerce 2.1.16 y posterior

>[!WARNING]
>
>_no_ habilita esta variable en un proyecto de [arquitectura escalada](../architecture/scaled-architecture.md). Provoca errores de conexión de Redis. Los esclavos de Redis siguen activos, pero no se utilizan para las lecturas de Redis. Como alternativa, Adobe recomienda utilizar Adobe Commerce 2.3.5 o posterior, implementar una nueva configuración de back-end de Redis e implementar el almacenamiento en caché L2 para Redis.

>[!TIP]
>
>La variable `REDIS_USE_SLAVE_CONNECTION` solo es compatible con Adobe Commerce en entornos de clúster de Cloud Infrastructure, Staging y Production Pro, y no con proyectos iniciales.

Adobe Commerce puede leer varias instancias de Redis de forma asincrónica. Configúrelo en `true` para usar automáticamente una conexión de _solo lectura_ con una instancia de Redis para recibir tráfico de solo lectura en un nodo que no sea maestro. Esta conexión mejora el rendimiento mediante el equilibrio de carga, ya que solo un nodo administra el tráfico de lectura-escritura. Establezca el valor en `false` para quitar cualquier matriz de conexión de solo lectura existente del archivo `env.php`.

```yaml
stage:
  deploy:
    REDIS_USE_SLAVE_CONNECTION: true
```

Debe tener un servicio Redis configurado en el archivo `.magento.app.yaml` y en el archivo `services.yaml`.

[ECE-Tools versión 2002.0.18](../release-notes/cloud-release-archive.md#v2002018) y posteriores usan más configuraciones de tolerancia a errores. Si Adobe Commerce no puede leer datos de la instancia de Redis _esclavo_, leerá datos de la instancia de Redis _maestro_.

La conexión de solo lectura no está disponible para su uso en el entorno de integración o si utiliza la variable [`CACHE_CONFIGURATION` &#x200B;](#cache_configuration).

## `VALKEY_BACKEND`

- **Predeterminado**—`Cm_Cache_Backend_Redis`
- **Versión**—Adobe Commerce 2.8.0 y posterior

`VALKEY_BACKEND` especifica la configuración del modelo back-end para la caché de Valkey.

La versión 2.8.0 y posteriores de Adobe Commerce incluyen los siguientes modelos backend:

- `Cm_Cache_Backend_Redis`
- `\Magento\Framework\Cache\Backend\Redis`
- `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache`

En el siguiente ejemplo se describe cómo establecer `VALKEY_BACKEND`:

```yaml
stage:
  deploy:
  VALKEY_USE_SLAVE_CONNECTION: true
  VALKEY_BACKEND: '\Magento\Framework\Cache\Backend\RemoteSynchronizedCache'
```

>[!NOTE]
>
>Si especifica `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache` como modelo de servidor de Valkey para habilitar la [caché L2](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html), `ece-tools` genera la configuración de caché automáticamente. Vea un ejemplo de [archivo de configuración](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html#configuration-example) en la _Guía de configuración de Adobe Commerce_. Para anular la configuración de caché generada, use la variable de implementación [CACHE_CONFIGURATION](#cache_configuration).

## `VALKEY_USE_SLAVE_CONNECTION`

- **Predeterminado**—`false`
- **Versión**: Adobe Commerce 2.4.8 y posterior

>[!WARNING]
>
>_no_ habilita esta variable en un proyecto de [arquitectura escalada](../architecture/scaled-architecture.md). Causa errores de conexión de Valkey. Los esclavos de Redis siguen activos, pero no se utilizan para las lecturas de Redis. Como alternativa, Adobe recomienda utilizar Adobe Commerce 2.4.8 o posterior, implementar una nueva configuración de back-end de Valkey e implementar el almacenamiento en caché L2 para Valkey.

>[!TIP]
>
>La variable `VALKEY_USE_SLAVE_CONNECTION` solo es compatible con Adobe Commerce en entornos de clúster de Cloud Infrastructure, Staging y Production Pro, y no con proyectos iniciales.

Adobe Commerce puede leer varias instancias de Redis de forma asincrónica. `VALKEY_USE_SLAVE_CONNECTION` se estableció en `true` para usar automáticamente una conexión de _solo lectura_ a una instancia de Redis con el fin de recibir tráfico de solo lectura en un nodo que no sea maestro. Esta conexión mejora el rendimiento mediante el equilibrio de carga, ya que solo un nodo administra el tráfico de lectura-escritura. Establezca `VALKEY_USE_SLAVE_CONNECTION` en `false` para quitar cualquier matriz de conexión de solo lectura existente del archivo `env.php`.

```yaml
stage:
  deploy:
    VALKEY_USE_SLAVE_CONNECTION: true
```

Debe tener un servicio Redis configurado en el archivo `.magento.app.yaml` y en el archivo `services.yaml`.

[ECE-Tools versión 2002.0.18](../release-notes/cloud-release-archive.md#v2002018) y posteriores usan más configuraciones de tolerancia a errores. Si Adobe Commerce no puede leer los datos de la instancia de Valkey _esclavo_, leerá los datos de la instancia de Redis _maestro_.

La conexión de solo lectura no está disponible para su uso en el entorno de integración o si utiliza la variable [`CACHE_CONFIGURATION` &#x200B;](#cache_configuration).

## `RESOURCE_CONFIGURATION`

- **Predeterminado**: no establecido
- **Versión**: Adobe Commerce 2.1.4 y posterior

Asigna un nombre de recurso a una conexión de base de datos. Esta configuración corresponde a la sección `resource` del archivo `env.php`.

{{merge-options}}

El siguiente ejemplo combina nuevos valores en una configuración existente:

```yaml
stage:
  deploy:
    RESOURCE_CONFIGURATION:
      _merge: true
      default_setup:
        connection: default
```

## `SCD_COMPRESSION_LEVEL`

- **Predeterminado**—`4`
- **Versión**: Adobe Commerce 2.1.4 y posterior

Especifica el nivel de compresión [gzip](https://www.gnu.org/software/gzip) (`0` a `9`) que se usará para comprimir contenido estático; `0` deshabilita la compresión.

```yaml
stage:
  deploy:
    SCD_COMPRESSION_LEVEL: 5
```

## `SCD_COMPRESSION_TIMEOUT`

- **Predeterminado**—`600`
- **Versión**: Adobe Commerce 2.1.4 y posterior

Cuando el tiempo necesario para comprimir los recursos estáticos supera el límite de tiempo de espera de compresión, interrumpe el proceso de implementación. Establezca el tiempo máximo de ejecución, en segundos, para el comando de compresión de contenido estático.

```yaml
stage:
  deploy:
    SCD_COMPRESSION_TIMEOUT: 800
```

## `SCD_MATRIX`

- **Predeterminado**—_No establecido_
- **Versión**: Adobe Commerce 2.1.4 y posterior

Puede configurar varias configuraciones regionales por tema. Esta personalización acelera el proceso de implementación al reducir el número de archivos de temas innecesarios. Por ejemplo, puede implementar el tema _magento/backend_ en inglés y un tema personalizado en otros idiomas.

El siguiente ejemplo implementa el tema `Magento/backend` con tres configuraciones regionales:

```yaml
stage:
  deploy:
    SCD_MATRIX:
      "magento/backend":
        language:
          - en_US
          - fr_FR
          - af_ZA
```

Además, puede elegir _no_ implementar un tema:

```yaml
stage:
  deploy:
    SCD_MATRIX:
      "magento/backend": [ ]
```

## `SCD_MAX_EXECUTION_TIME`

- **Predeterminado**—_No establecido_
- **Versión**—Adobe Commerce 2.2.0 y posterior

Permite aumentar el tiempo de ejecución máximo esperado para la implementación de contenido estático.

De forma predeterminada, Adobe Commerce establece el tiempo de ejecución máximo esperado en 900 segundos, pero en algunos casos puede necesitar más tiempo para completar la implementación de contenido estático para un proyecto de Cloud.

```yaml
stage:
  deploy:
    SCD_MAX_EXECUTION_TIME: 3600
```

{{scd-timing-warning}}

## `SCD_NO_PARENT`

- **Predeterminado**—`false`
- **Versión**: Adobe Commerce 2.4.2 y posterior

En la fase de implementación, establezca `SCD_NO_PARENT: true` de modo que la generación de contenido estático para los temas principales no se produzca durante la fase de implementación. Esta configuración minimiza el tiempo de implementación y evita el tiempo de inactividad del sitio que puede producirse si la compilación de contenido estático falla durante la implementación. Consulte [Implementación de contenido estático](../deploy/static-content.md).

```yaml
stage:
  deploy:
    SCD_NO_PARENT: true
```

## `SCD_STRATEGY`

- **Predeterminado**—`quick`
- **Versión**—Adobe Commerce 2.2.0 y posterior

Permite personalizar la [estrategia de implementación](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-strategy.html) para el contenido estático. Consulte [Implementar archivos de vista estática](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html).

Utilice estas opciones _solamente_ si tiene más de una configuración regional:

- `standard`: implementa todos los archivos de vista estática para todos los paquetes.
- `quick`—(_default_) minimiza el tiempo de implementación.
- `compact`: conserva espacio en disco en el servidor. En la versión 2.2.4 y anteriores de Adobe Commerce, esta configuración anula el valor de `scd_threads` con un valor de `1`.

```yaml
stage:
  deploy:
    SCD_STRATEGY: "compact"
```

## `SCD_THREADS`

- **Predeterminado**—Automático
- **Versión**: Adobe Commerce 2.1.4 y posterior

Establece el número de subprocesos para la implementación de contenido estático. El valor predeterminado se establece en función del recuento de subprocesos de CPU detectado y no supera un valor de 4. El aumento del número de subprocesos acelera la implementación de contenido estático; reducir el número de subprocesos hace que se ralentice. Puede establecer el valor del subproceso, por ejemplo:

```yaml
stage:
  deploy:
    SCD_THREADS: 2
```

Para reducir aún más el tiempo de implementación, use [Administración de configuración](../store/store-settings.md) con el comando `scd-dump` para mover la implementación estática a la fase de compilación.

## `SEARCH_CONFIGURATION`

- **Predeterminado**—_No establecido_
- **Versión**: Adobe Commerce 2.1.4 y posterior

Utilice esta variable de entorno para conservar la configuración personalizada del servicio de búsqueda entre implementaciones. Por ejemplo:

Configuración de Elasticsearch:

```yaml
stage:
  deploy:
    SEARCH_CONFIGURATION:
      engine: elasticsearch
      elasticsearch_server_hostname: http://elasticsearch.internal
      elasticsearch_server_port: '9200'
      elasticsearch_index_prefix: magento2
      elasticsearch_server_timeout: '15'
```

Configuración de OpenSearch (para Commerce 2.4.6 y posterior):

```yaml
stage:
  deploy:
    SEARCH_CONFIGURATION:
      engine: opensearch
      opensearch_server_hostname: 'http://opensearch.internal'
      opensearch_server_port: '9200'
      opensearch_index_prefix: 'magento2'
      opensearch_server_timeout: '15'
```

{{merge-options}}

El siguiente ejemplo combina un nuevo valor con la configuración existente:

```yaml
stage:
  deploy:
    SEARCH_CONFIGURATION:
      engine: elasticsearch
      elasticsearch_server_port: '9200'
      _merge: true
```

## `SESSION_CONFIGURATION`

- **Predeterminado**—_No establecido_
- **Versión**: Adobe Commerce 2.1.4 y posterior

Configure el almacenamiento de sesión de Redis. Requiere las opciones `save`, `redis`, `host`, `port` y `database` para la variable de almacenamiento de sesión. Por ejemplo:

```yaml
stage:
  deploy:
    SESSION_CONFIGURATION:
      redis:
        bot_first_lifetime: 100
        bot_lifetime: 10001
        database: 0
        disable_locking: 1
        host: redis.internal
        max_concurrency: 10
        max_lifetime: 10001
        min_lifetime: 100
        port: 6379
      save: redis
```

{{merge-options}}

El siguiente ejemplo combina un nuevo valor con la configuración existente:

```yaml
stage:
  deploy:
    SESSION_CONFIGURATION:
      _merge: true
      redis:
        max_concurrency: 10
```

## `SKIP_SCD`

- **Predeterminado**— _No establecido_
- **Versión**: Adobe Commerce 2.1.4 y posterior

Se establece en `true` para omitir la implementación de contenido estático durante la fase de implementación.

En la fase de implementación, configure `SKIP_SCD: true` para que la generación de contenido estático no se produzca durante la fase de implementación. Esta configuración minimiza el tiempo de implementación y evita el tiempo de inactividad del sitio que puede producirse si la compilación de contenido estático falla durante la implementación. Consulte [Implementación de contenido estático](../deploy/static-content.md).

```yaml
stage:
  deploy:
    SKIP_SCD: true
```

## `UPDATE_URLS`

- **Predeterminado**—`true`
- **Versión**: Adobe Commerce 2.1.4 y posterior

En la implementación, reemplace las direcciones URL base de Adobe Commerce en la base de datos por las direcciones URL del proyecto especificadas por la variable [`MAGENTO_CLOUD_ROUTES`](variables-cloud.md). Esta configuración es útil para el desarrollo local, donde las direcciones URL base están configuradas para el entorno local. Al implementar en un entorno de Cloud, las direcciones URL se actualizan para que pueda acceder a su tienda y al administrador mediante las direcciones URL del proyecto.

Si debe actualizar las direcciones URL al implementar en entornos de ensayo y producción Pro o Starter, utilice la variable [`FORCE_UPDATE_URLS`](#force_update_urls).

```yaml
stage:
  deploy:
    UPDATE_URLS: false
```

## `VERBOSE_COMMANDS`

- **Predeterminado**—_No establecido_
- **Versión**: Adobe Commerce 2.1.4 y posterior

Habilite o deshabilite el nivel de detalle de depuración de [Symfony](https://symfony.com/doc/current/console/verbosity.html) para los comandos CLI de `bin/magento` ejecutados durante la fase de implementación.

>[!NOTE]
>
>Para usar la configuración VERBOSE_COMMANDS para controlar los detalles en la salida del comando para los comandos CLI `bin/magento` correctos y fallidos, debe establecer [MIN_LOGGING_LEVEL](variables-global.md#minlogginglevel) `debug`.

Elija el nivel de detalle proporcionado en los registros:

- `-v`= salida normal
- `-vv`= salida más detallada
- `-vvv` = resultado detallado ideal para la depuración

```yaml
stage:
  deploy:
    VERBOSE_COMMANDS: "-vv"
```
