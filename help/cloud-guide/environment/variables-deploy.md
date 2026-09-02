---
title: Implementación de variables
description: Consulte la lista de variables de entorno que controlan las acciones en la fase de implementación de Adobe Commerce en la infraestructura en la nube.
feature: Cloud, Configuration, Cache, Deploy, SCD, Storage, Search
recommendations: noDisplay, catalog
role: Developer
exl-id: 980ec809-8c68-450a-9db5-29c5674daa16
TQID: https://experienceleague.adobe.com/TNuUxXzCiXnKefww0DmKbjfJygEz2HFG-0PjCsCy2nA
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: d1e21356-0064-4f48-9089-16e3f0dbd2a6
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: bdc2bedd2696e7dde0ffb55f846a8bced2dbd25d
workflow-type: tm+mt
source-wordcount: 3106
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

Utilice `CACHE_CONFIGURATION` para combinar o anular las opciones de front-end y back-end de la caché generadas durante la implementación.

Para Adobe Commerce en la infraestructura en la nube, no edite `app/etc/env.php` directamente. El paquete `ece-tools` genera la configuración de implementación a partir de `.magento.env.yaml`, las relaciones de servicio y las variables de implementación admitidas.

Use `VALKEY_BACKEND` o `REDIS_BACKEND` para seleccionar la caché admitida o la implementación L2 para la versión exacta de Adobe Commerce. Use `CACHE_CONFIGURATION` para personalizar opciones como reintentos de conexión, tiempos de espera de lectura, prefijos de caché o claves de precarga.

La combinación de servidor y servicio de caché admitida depende de la versión de Commerce y del nivel de parche. Redis no es compatible con Adobe Commerce 2.4.9 ni con versiones de parches posteriores a las 2.4.5-p16, 2.4.6-p14, 2.4.7-p9 y 2.4.8-p4. Use Valkey para las versiones donde lo requieran [requisitos del sistema](https://experienceleague.adobe.com/en/docs/commerce-operations/installation-guide/system-requirements).

>[!NOTE]
>
>Para obtener instrucciones más detalladas sobre la configuración del servicio Redis y Valkey, consulte [Prácticas recomendadas para la configuración del servicio Valkey y Redis](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration)

De forma predeterminada, el proceso de implementación sobrescribe la configuración de caché correspondiente. Para combinar los valores especificados con la configuración generada, establezca `_merge` en `true`:

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      _merge: true
      frontend:
        default:
          backend_options:
            connect_retries: 3
          remote_backend_options:
            read_timeout: 10
```

Para reemplazar la configuración existente con los valores especificados en `CACHE_CONFIGURATION`, establezca `_merge` en `false`.

>[!IMPORTANT]
>
> No copie las opciones de `bin/magento setup:config:set` locales, como `cm_cache_backend_redis`, directamente en `CACHE_CONFIGURATION`. En proyectos en la nube, `ece-tools` obtiene detalles de conexión de servicio de las relaciones configuradas. Utilice la estructura documentada para la versión de Commerce e implementación de caché seleccionadas.

En el siguiente ejemplo se combinan asignaciones de base de datos en una configuración de caché existente. Utilice este tipo de anulación únicamente cuando el backend seleccionado y la versión de Commerce lo admitan. Aplique la configuración de front-end a `symfony_l2` solo si la documentación actual de Symfony L2 admite explícitamente la opción.

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

El siguiente ejemplo usa la [característica de precarga Redis](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cache/redis/redis-pg-cache#redis-preload-feature) tal como se define en la _guía de configuración_. Utilice las directrices de Valkey correspondientes para las versiones que utilizan Valkey.

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

Para usar un modelo [REDIS_BACKEND](#redis_backend) personalizado que no esté en la lista de permitidos, establezca `_custom_redis_backend` en `true` para que ece-tools aplique la validación adecuada:

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

Habilita o deshabilita la limpieza de [archivos de contenido estático](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment) generados durante la fase de compilación o implementación. Se recomienda usar el valor predeterminado _true_ en desarrollo.

- **`true`**: elimina todo el contenido estático existente antes de implementar el contenido estático actualizado.
- **`false`**: la implementación solo sobrescribe los archivos de contenido estático existentes si el contenido generado contiene una versión más reciente.

Si modifica el contenido estático mediante un proceso independiente, establezca el valor en _false_.

```yaml
stage:
  deploy:
    CLEAN_STATIC_FILES: false
```

Si no se limpian los archivos de vista estática antes de la implementación, pueden producirse problemas si se implementan actualizaciones en los archivos existentes sin quitar las versiones anteriores. Debido a las reglas [static file fallback](https://developer.adobe.com/commerce/frontend-core/guide/css/preprocess#clean-static-view-files), las operaciones de reserva pueden mostrar el archivo incorrecto si el directorio contiene varias versiones del mismo archivo.

## `CRON_CONSUMERS_RUNNER`

- **Predeterminado**—`cron_run = false`, `max_messages = 1000`

Utilice esta variable de entorno para confirmar que las colas de mensajes se están ejecutando después de una implementación.

- `cron_run`: valor booleano que habilita o deshabilita el trabajo cron de `consumers_runner`. El valor predeterminado es `false`.
- `max_messages`: número máximo de mensajes que procesa cada consumidor antes de finalizar. El valor predeterminado es `1000`. Para evitar que el consumidor finalice, establézcalo en `0`.
- `consumers`: una matriz de cadenas que especifica los nombres de los consumidores que se van a ejecutar. Una matriz vacía ejecuta _todos_ los consumidores.
- `multiple_processes`: número de procesos que se generarán para cada consumidor. Esta opción es compatible con Adobe Commerce 2.4.4 y versiones posteriores.

>[!NOTE]
>
>Para enumerar los consumidores de cola de mensajes disponibles, ejecute el comando `./bin/magento queue:consumers:list` en el entorno remoto.

El siguiente ejemplo ejecuta los consumidores seleccionados e inicia varios procesos para cada uno:

```yaml
stage:
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 1000
      consumers:
       example_consumer_1
       example_consumer_2
      multiple_processes:
        example_consumer_1: 4
        example_consumer_2: 3
```

El siguiente ejemplo ejecuta todos los consumidores:

```yaml
stage:
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 1000
      consumers: []
```

De manera predeterminada, el proceso de implementación sobrescribe la configuración correspondiente en el archivo `env.php`. Consulte [Administrar colas de mensajes](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/message-queues/manage-message-queues) en la _Guía de configuración de Commerce_ para Adobe Commerce local.

## `CONSUMERS_WAIT_FOR_MAX_MESSAGES`

- **Predeterminado**—`false`

Configure cómo `consumers` procesa los mensajes de la cola de mensajes eligiendo una de las siguientes opciones:

- `false`—`Consumers` procesa los mensajes disponibles, cierra la conexión TCP y finaliza independientemente del límite de `max_messages` especificado en la variable de implementación `CRON_CONSUMERS_RUNNER`.

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

>[!WARNING]
>
>Para evitar exponer la clave en el repositorio de código fuente, establezca el valor `CRYPT_KEY` a través del archivo [!DNL Cloud Console] en lugar del archivo `.magento.env.yaml`. Consulte [Establecer variables de entorno y proyecto](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/project/overview#configure-environment).

Cuando mueve la base de datos de un entorno a otro sin un proceso de instalación, necesita la información criptográfica correspondiente. Adobe Commerce usa el valor de clave de cifrado establecido en [!DNL Cloud Console] como el valor `crypt/key` del archivo `env.php`.

## `DATABASE_CONFIGURATION`

- **Predeterminado**—_No establecido_

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
>En un clúster de ensayo/producción profesional que tenga tres nodos (o tres nodos de servicio en [Arquitectura escalable](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/architecture/scaled-architecture#service-tier)), `indices_settings` debe establecerse de la siguiente manera:
>
>```yaml
>           indices_settings:
>               number_of_shards: 1
>               number_of_replicas: 2
>```

{{merge-options}}

El siguiente ejemplo combina un nuevo valor con la configuración existente:

```yaml
stage:
  deploy:
    ELASTICSUITE_CONFIGURATION:
      indices_settings:
        number_of_shards: 1
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

Habilita y deshabilita Google Analytics al implementarlo en entornos de ensayo e integración. De forma predeterminada, Google Analytics solo es true para el entorno de producción. Para habilitar Google Analytics en los entornos de ensayo e integración, establezca este valor en `true`.

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

Al implementarse en entornos de ensayo y producción Pro o Starter, esta variable reemplaza las direcciones URL base de Adobe Commerce en la base de datos por las direcciones URL del proyecto especificadas por la variable [`MAGENTO_CLOUD_ROUTES`](variables-cloud.md). Para anular el comportamiento predeterminado de la variable de implementación [UPDATE_URLS](#update_urls), usa esta configuración.

```yaml
stage:
  deploy:
    FORCE_UPDATE_URLS: true
```

## `LOCK_PROVIDER`

- **Predeterminado**— En los entornos de Producción y Ensayo, el valor predeterminado es `file` y no se puede cambiar. Para la integración de Pro y los entornos de inicio, el valor predeterminado es `db`.

El proveedor de bloqueo evita que se ejecuten los trabajos cron duplicados y los grupos cron. Adobe Commerce en la nube admite los proveedores de bloqueos `file` y `db`.

En los entornos de ensayo y producción de Pro, `MAGENTO_CLOUD_LOCKS_DIR` configura el proveedor `file`. No puede anular esta configuración. En los entornos de Pro Integration y Starter, `ece-tools` establece el proveedor `db` de forma predeterminada. Para optimizar el rendimiento local y reflejar la arquitectura de producción, establezca el proveedor en `file` en esos entornos.

```yaml
stage:
  deploy:
    LOCK_PROVIDER: 'file'
```

## `MYSQL_USE_SLAVE_CONNECTION`

- **Predeterminado**—`false`

>[!TIP]
>
>La variable `MYSQL_USE_SLAVE_CONNECTION` solo es compatible con Adobe Commerce en clústeres de la infraestructura en la nube Staging y Production Pro. No es compatible con proyectos iniciales.

Adobe Commerce puede leer varias bases de datos de forma asincrónica. Configúrelo en `true` para utilizar una conexión de _solo lectura_ con la base de datos automáticamente para recibir tráfico de solo lectura en un nodo que no sea maestro. Esta conexión mejora el rendimiento mediante el equilibrio de carga, ya que solo un nodo administra el tráfico de lectura-escritura. Para quitar cualquier matriz de conexión de solo lectura existente del archivo `env.php`, establezca en `false`.

```yaml
stage:
  deploy:
    MYSQL_USE_SLAVE_CONNECTION: true
```

Cuando la variable `MYSQL_USE_SLAVE_CONNECTION` se establece en `true`, el sistema establece el parámetro `synchronous_replication` en `true` de forma predeterminada en el archivo `env.php` en los entornos de ensayo y producción de Pro. Cuando `MYSQL_USE_SLAVE_CONNECTION` está establecido en `false`, el parámetro `synchronous_replication` no está configurado.

## `QUEUE_CONFIGURATION`

- **Predeterminado**—_No establecido_

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

Especifica la configuración del modelo backend para la caché de Redis.

La caché de Redis no es compatible con Adobe Commerce 2.4.9 ni con versiones de parches posteriores a 2.4.5-p16, 2.4.6-p14, 2.4.7-p9 y 2.4.8-p4. Para esas versiones, use Valkey y la configuración `VALKEY_BACKEND` correspondiente. Compruebe siempre el servicio de caché admitido en [requisitos del sistema](https://experienceleague.adobe.com/en/docs/commerce-operations/installation-guide/system-requirements).

Para las versiones compatibles con Redis, los modelos de back-end disponibles incluyen:

- `Cm_Cache_Backend_Redis`
- `\Magento\Framework\Cache\Backend\Redis`
- `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache`

El siguiente ejemplo habilita el back-end de la caché sincronizada de forma remota y la caché L2:

```yaml
stage:
  deploy:
    REDIS_BACKEND: '\Magento\Framework\Cache\Backend\RemoteSynchronizedCache'
```

>[!NOTE]
>
> Cuando se selecciona `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache`, `ece-tools` genera automáticamente la configuración de la caché L2. Para personalizar la configuración generada, use [`CACHE_CONFIGURATION`](#cache_configuration).

## `REDIS_USE_SLAVE_CONNECTION`

- **Predeterminado**—`false`

>[!TIP]
>
>`REDIS_USE_SLAVE_CONNECTION` solo se admite en clústeres de Adobe Commerce en Cloud Staging y Production Pro. No es compatible con proyectos iniciales.

Adobe Commerce puede leer varias instancias de Redis de forma asincrónica. Establezca esta variable en `true` para utilizar una conexión de solo lectura con una réplica de Redis para el tráfico de lectura mientras la instancia principal administra el tráfico de lectura-escritura. Para quitar una matriz de conexión de solo lectura existente de `env.php`, establézcala en `false`.

```yaml
stage:
  deploy:
    REDIS_USE_SLAVE_CONNECTION: true
```

Debe tener un servicio [Redis configurado](../services/redis.md) en los archivos `.magento.app.yaml` y `services.yaml`.

[ECE-Tools versión 2002.0.18](../release-notes/cloud-release-archive.md#v2002018) y posteriores usan más configuraciones de tolerancia a errores. Si Adobe Commerce no puede leer los datos de la réplica de Redis, vuelve a la instancia principal de Redis.

La conexión de solo lectura no está disponible en el entorno de integración. Si usa [`CACHE_CONFIGURATION`](#cache_configuration), combine los cambios en la configuración generada y compruebe que la configuración resultante conserva la conexión de réplica.

## `VALKEY_BACKEND`

- **Predeterminado**—`Cm_Cache_Backend_Redis`
- **Versión**: versiones de Adobe Commerce compatibles con Valkey

`VALKEY_BACKEND` especifica el modelo backend para la configuración de caché de Valkey. El valor predeterminado utiliza un nombre de clase heredado compatible con Redis; no significa que el servicio deba ser Redis.

Para versiones de Adobe Commerce anteriores a la 2.4.9 compatibles con Valkey, los modelos backend incluyen:

- `Cm_Cache_Backend_Redis`
- `\Magento\Framework\Cache\Backend\Redis`
- `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache`

Adobe Commerce 2.4.9 y versiones posteriores también admiten `symfony_l2`, la implementación de L2 basada en Symfony Cache. `symfony_l2` solo se admite con Valkey.

### Configurar caché sincronizada remota

Para Adobe Commerce 2.4.8, utilice la siguiente configuración cuando la implementación de caché sincronizada remota sea adecuada:

```yaml
stage:
  deploy:
    VALKEY_BACKEND: '\Magento\Framework\Cache\Backend\RemoteSynchronizedCache'
```

Al especificar el servidor remoto sincronizado, se habilita la caché L2 y `ece-tools` genera la configuración de la caché automáticamente. Consulte el [archivo de configuración de ejemplo](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration#customize-the-symfony-l2-cache-configuration). Para personalizar la configuración generada, use [`CACHE_CONFIGURATION`](#cache_configuration).

### Configurar la implementación moderna de la caché Symfony L2

Para Adobe Commerce 2.4.9 y versiones posteriores, utilice la implementación de Symfony L2:

```yaml
stage:
  deploy:
    VALKEY_BACKEND: 'symfony_l2'
```

Si se especifica `symfony_l2` como modelo backend de Valkey, se habilita la caché L2 y `ece-tools` genera la configuración de caché L2 automáticamente a partir de los detalles de conexión del servicio de Valkey, incluidos los front-end `default` y `stale_cache_enabled`. Defina `CACHE_CONFIGURATION` solo cuando necesite personalizar las opciones del servidor compatibles, como el directorio de caché local. Consulte la [implementación de la caché L2 de Symfony](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration#configure-symfony-l2-cache){target="_blank"} en la _Guía de configuración de Adobe Commerce_.

>[!NOTE]
>
>Adobe Commerce 2.4.9 incluye mejoras en la memoria caché de Symfony L2, como almacenamiento de etiquetas de caché, invalidación y compresión, con el parche ACP2E-5132, reducción de E/S de disco, eliminación de entradas de caché antiguas y reducción de la sobrecarga de memoria y de red.

## `VALKEY_USE_SLAVE_CONNECTION`

- **Predeterminado**—`false`
- **Versión**: Adobe Commerce 2.4.8 y posterior

>[!TIP]
>
>`VALKEY_USE_SLAVE_CONNECTION` solo se admite en clústeres de Adobe Commerce en Cloud Staging y Production Pro. No es compatible con proyectos iniciales.

Adobe Commerce puede leer varias instancias de Valkey asincrónicamente. Establezca `VALKEY_USE_SLAVE_CONNECTION` en `true` para usar una conexión de _solo lectura_ con una réplica de Valkey para el tráfico de solo lectura mientras la instancia principal administra el tráfico de lectura-escritura. Esta conexión mejora el rendimiento mediante el equilibrio de carga, ya que solo un nodo administra el tráfico de lectura-escritura. Para quitar una matriz de conexión de solo lectura existente de `env.php`, establézcala en `false`.

```yaml
stage:
  deploy:
    VALKEY_USE_SLAVE_CONNECTION: true
```

Debe tener un [servicio Valkey configurado](../services/valkey.md) en `.magento.app.yaml` y `.magento/services.yaml`. La disponibilidad de una conexión de réplica depende de la topología del proyecto y de la versión `ece-tools` instalada.

Antes de confiar en esta configuración, inspeccione el valor `MAGENTO_CLOUD_RELATIONSHIPS` descodificado y confirme que hay una relación de réplica. Por ejemplo:

```bash
echo "$MAGENTO_CLOUD_RELATIONSHIPS" | base64 -d | json_pp
```

Para `symfony_l2`, la compatibilidad con réplicas requiere las actualizaciones relevantes de `ece-tools` y Cloud Patches. Actualice a la última versión de `ece-tools` antes de habilitar esta configuración. Si no hay ninguna relación de réplica después de la reimplementación, póngase en contacto con el soporte técnico de Adobe Commerce.

Al usar [`CACHE_CONFIGURATION`](#cache_configuration), combine las invalidaciones admitidas en la configuración generada en lugar de reemplazar la estructura de conexión generada.

## `RESOURCE_CONFIGURATION`

- **Predeterminado**: no establecido

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

Especifica el nivel de compresión [gzip](https://www.gnu.org/software/gzip) (`0` a `9`) que se usará para comprimir contenido estático. Configúrelo en `0` para deshabilitar la compresión.

```yaml
stage:
  deploy:
    SCD_COMPRESSION_LEVEL: 5
```

## `SCD_COMPRESSION_TIMEOUT`

- **Predeterminado**—`600`

Cuando el tiempo necesario para comprimir los recursos estáticos supera el límite de tiempo de espera de compresión, interrumpe el proceso de implementación. Establezca el tiempo máximo de ejecución, en segundos, para el comando de compresión de contenido estático.

```yaml
stage:
  deploy:
    SCD_COMPRESSION_TIMEOUT: 800
```

## `SCD_MATRIX`

- **Predeterminado**—_No establecido_

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

Permite aumentar el tiempo de ejecución máximo esperado para la implementación de contenido estático.

De forma predeterminada, Adobe Commerce establece el tiempo máximo esperado de ejecución en 900 segundos, pero algunos escenarios requieren más tiempo para completar la implementación de contenido estático para un proyecto de Cloud.

```yaml
stage:
  deploy:
    SCD_MAX_EXECUTION_TIME: 3600
```

{{scd-timing-warning}}

## `SCD_NO_PARENT`

- **Predeterminado**—`false`

En la fase de implementación, establezca `SCD_NO_PARENT: true` de modo que la generación de contenido estático para los temas principales no se produzca durante la fase de implementación. Esta configuración minimiza el tiempo de implementación y evita el tiempo de inactividad del sitio que puede producirse si la compilación de contenido estático falla durante la implementación. Consulte [Implementación de contenido estático](../deploy/static-content.md).

```yaml
stage:
  deploy:
    SCD_NO_PARENT: true
```

## `SCD_STRATEGY`

- **Predeterminado**—`quick`

Permite personalizar la [estrategia de implementación](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-strategy) para el contenido estático. Consulte [Implementar archivos de vista estática](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment).

Utilice estas opciones _solamente_ si tiene más de una configuración regional:

- `standard`: implementa todos los archivos de vista estática para todos los paquetes.
- `quick`—(_default_) minimiza el tiempo de implementación.
- `compact`: conserva espacio en disco en el servidor.

```yaml
stage:
  deploy:
    SCD_STRATEGY: "compact"
```

## `SCD_THREADS`

- **Predeterminado**—Automático

Establece el número de subprocesos para la implementación de contenido estático. El valor predeterminado se establece en función del recuento de subprocesos de CPU detectado y no supera un valor de 4. El aumento del número de subprocesos acelera la implementación de contenido estático. Al reducir el número de subprocesos, se ralentiza. Puede establecer el valor del subproceso, por ejemplo:

```yaml
stage:
  deploy:
    SCD_THREADS: 2
```

Para reducir aún más el tiempo de implementación, use [Administración de configuración](../store/store-settings.md) con el comando `scd-dump` para mover la implementación estática a la fase de compilación.

## `SEARCH_CONFIGURATION`

- **Predeterminado**—_No establecido_

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

Use `SESSION_CONFIGURATION` para configurar el almacenamiento de la sesión. El ejemplo siguiente utiliza la estructura de configuración de sesión compatible con Redis. Utilícelo únicamente con la combinación de nomenclatura y servicio de almacenamiento de sesión que admite la versión exacta de Commerce. Para las sesiones respaldadas por Valkey, siga el [ejemplo de almacenamiento de sesión de Valkey](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration#apply-all-best-practice-recommendations).

No dé por hecho que las variables de caché como `VALKEY_BACKEND` o `REDIS_BACKEND` configuran sesiones. La configuración de caché y sesión es independiente. En proyectos en la nube, utilice la relación de servicio y la configuración generada siempre que sea posible; no codifique en el código valores específicos del entorno sin reemplazar el host y el puerto de ejemplo.

```yaml
stage:
  deploy:
    SESSION_CONFIGURATION:
      redis:
        bot_first_lifetime: 100
        bot_lifetime: 10001
        database: 0
        disable_locking: 1
        host: 'redis.internal'
        max_concurrency: 10
        max_lifetime: 10001
        min_lifetime: 100
        port: 6379
      save: redis
```

Reemplace `redis.internal` y `6379` por el host y el puerto del servicio de sesión para el entorno de destino cuando la configuración de implementación requiera detalles de conexión explícitos.

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

Se establece en `true` para omitir la implementación de contenido estático durante la fase de implementación.

En la fase de implementación, configure `SKIP_SCD: true` para que la generación de contenido estático no se produzca durante la fase de implementación. Esta configuración minimiza el tiempo de implementación y evita el tiempo de inactividad del sitio que puede producirse si la compilación de contenido estático falla durante la implementación. Consulte [Implementación de contenido estático](../deploy/static-content.md).

```yaml
stage:
  deploy:
    SKIP_SCD: true
```

## `UPDATE_URLS`

- **Predeterminado**—`true`

En la implementación, reemplace las direcciones URL base de Adobe Commerce en la base de datos por las direcciones URL del proyecto especificadas por la variable [`MAGENTO_CLOUD_ROUTES`](variables-cloud.md). Esta configuración es útil para el desarrollo local, donde las direcciones URL base están configuradas para el entorno local. Al implementar en un entorno de Cloud, las direcciones URL se actualizan para que pueda acceder a su tienda y al administrador mediante las direcciones URL del proyecto.

Si debe actualizar las direcciones URL al implementar en entornos de ensayo y producción Pro o Starter, utilice la variable [`FORCE_UPDATE_URLS`](#force_update_urls).

```yaml
stage:
  deploy:
    UPDATE_URLS: false
```

## `USE_LUA`

- **Predeterminado**—`false`
- **Versión**: Adobe Commerce 2.4.7 y posterior

Controla la opción de back-end de caché `use_lua` en `env.php` para el front-end de caché predeterminado (y, al usar el back-end `symfony_l2`, las opciones de back-end remoto del front-end `stale_cache_enabled`). Esta opción no se ha aplicado al front-end `page_cache`.

Utilice el valor predeterminado `false` a menos que la compatibilidad con Adobe indique explícitamente lo contrario.

```yaml
stage:
  deploy:
    USE_LUA: false
```

>[!WARNING]
>
>En Adobe Commerce 2.4.7 y 2.4.8, la configuración `USE_LUA: true` puede causar daños en la caché y problemas de pérdida de caché de GraphQL.
>
>A partir de Adobe Commerce 2.4.9, utilice la guía de configuración de caché de Valkey para su versión de Commerce y no confíe en `USE_LUA` para las nuevas implementaciones.

## `LUA_KEY`

La variable `LUA_KEY` está obsoleta. Si `LUA_KEY` se incluye en `.magento.env.yaml`, elimínelo durante la migración. En su lugar, utilice las variables `USE_LUA` y `USE_LUA_ON_GC`.

## `USE_LUA_ON_GC`

- **Predeterminado**—`true`
- **Versión**: Adobe Commerce 2.4.8 y posterior

Controla la opción de back-end de caché `use_lua_on_gc` en `env.php` para el front-end de caché predeterminado (y, al usar el back-end `symfony_l2`, las opciones de back-end remoto del front-end `stale_cache_enabled`) para la recolección de elementos no utilizados. Esta opción no se ha aplicado al front-end `page_cache`.

Utilice el valor predeterminado `true` para conservar la limpieza de etiquetas de caché atómica durante el trabajo cron de `backend_clean_cache`.

```yaml
stage:
  deploy:
    USE_LUA_ON_GC: true
```

>[!WARNING]
>
>En Adobe Commerce 2.4.8, la configuración `USE_LUA_ON_GC: false` puede provocar que la invalidación de la caché basada en etiquetas falle de forma silenciosa y requiera un vaciado de caché completo para recuperarse.
>
>En 2.4.9 y versiones posteriores, siga las [instrucciones del servicio de caché](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cache/redis/redis-pg-cache) para la versión instalada.

## `VERBOSE_COMMANDS`

- **Predeterminado**—_No establecido_

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
