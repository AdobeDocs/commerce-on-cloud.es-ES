---
title: Configurar servicios
description: Obtenga información sobre cómo configurar los servicios que utiliza Adobe Commerce en la infraestructura en la nube.
feature: Cloud, Configuration, Services
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1046'
ht-degree: 0%

---

# Configurar servicios

El archivo `services.yaml` define los servicios admitidos y utilizados por Adobe Commerce en la infraestructura de la nube, como MySQL, Redis y Elasticsearch o OpenSearch. No es necesario suscribirse a proveedores de servicios externos.

>[!NOTE]
>
>El archivo `.magento/services.yaml` se administra localmente en el directorio `.magento` del proyecto. Se accede a la configuración durante el proceso de compilación para definir las versiones de servicio necesarias solo en el entorno de integración y se elimina una vez completada la implementación, por lo que no se encontrarán en el servidor.


El script de implementación utiliza los archivos de configuración del directorio `.magento` para aprovisionar el entorno con los servicios configurados. Hay un servicio disponible para su aplicación si está incluido en la propiedad [`relationships`](../application/properties.md#relationships) del archivo `.magento.app.yaml`. El archivo `services.yaml` contiene los valores _type_ y _disk_. El tipo de servicio define el servicio _name_ y _version_.

Al cambiar una configuración de servicio, una implementación aprovisiona el entorno con los servicios actualizados, lo que afecta a los siguientes entornos:

- Todos los entornos iniciales, incluida la producción `master`
- Entornos de integración Pro

{{pro-update-service}}

## Servicios predeterminados y admitidos

La infraestructura en la nube admite e implementa los siguientes servicios:

- [MySQL](mysql.md)
- [Redis](redis.md)
- [RabbitMQ](rabbitmq.md)
- [Elasticsearch](elasticsearch.md)
- [OpenSearch](opensearch.md)

Puede ver las versiones y los valores de disco predeterminados en el archivo [default `services.yaml`](https://github.com/magento/magento-cloud/blob/master/.magento/services.yaml) actual. El siguiente ejemplo muestra los servicios `mysql`, `redis`, `opensearch` o `elasticsearch`, y `rabbitmq` definidos en el archivo de configuración `services.yaml`:

```yaml
mysql:
    type: mysql:10.4
    disk: 5120

redis:
    type: redis:6.2

opensearch:
    type: opensearch:2  # minor version not required; uses latest
    disk: 1024

rabbitmq:
    type: rabbitmq:3.9
    disk: 1024
```

## Valores de servicio

Debe proporcionar el identificador de servicio y la configuración del tipo de servicio `type: <name>:<version>`. Si el servicio utiliza almacenamiento persistente, debe proporcionar un valor de disco.

Utilice el siguiente formato:

```yaml
<service-id>:
    type: <name>:<version>
    disk: <value-MB>
```

### `service-id`

El valor `service-id` identifica el servicio en el proyecto. Solo puede utilizar caracteres alfanuméricos en minúsculas: `a` a `z` y `0` a `9`, como `redis`.

Este valor de _service-id_ se usa en la propiedad [`relationships`](../application/properties.md#relationships) del archivo de configuración `.magento.app.yaml`:

```yaml
relationships:
    redis: "<name>:redis"
```

Puede asignar nombres a varias instancias de cada tipo de servicio. Por ejemplo, puede utilizar varias instancias de Redis, una para la sesión y otra para la caché.

```yaml
redis:
    type: redis:<version>

redis2:
    type: redis:<version>
```

Al cambiar el nombre de un servicio en el archivo `services.yaml` **se quita de forma permanente** lo siguiente:

- El servicio existente antes de crear un servicio con el nuevo nombre especificado.
- Se eliminarán todos los datos existentes del servicio. El Adobe recomienda encarecidamente que [realice una copia de seguridad de su entorno de inicio](../storage/snapshots.md) antes de cambiar el nombre de un servicio existente.

### `type`

El valor `type` especifica el nombre y la versión del servicio. Por ejemplo:

```yaml
mysql:
    type: mysql:10.4
```

### `disk`

El valor `disk` especifica el tamaño del almacenamiento en disco persistente (en MB) que se va a asignar al servicio. Los servicios que utilizan almacenamiento persistente, como MySQL, deben proporcionar un valor de disco. Los servicios que utilizan memoria en lugar de almacenamiento persistente, como Redis, no requieren un valor de disco.

```yaml
mysql:
    type: mysql:10.4
    disk: 5120
```

La cantidad de almacenamiento predeterminada actual por proyecto es de 5 GB o 512 0 MB. Puede distribuir esta cantidad entre su aplicación y cada uno de sus servicios.

## Relaciones de servicio

En Adobe Commerce en proyectos de infraestructura en la nube, las [relaciones](../application/properties.md#relationships) del servicio configuradas en el archivo `.magento.app.yaml` determinan qué servicios están disponibles para su aplicación.

Puede recuperar los datos de configuración de todas las relaciones de servicio desde la variable de entorno [`$MAGENTO_CLOUD_RELATIONSHIPS`](../environment/variables-cloud.md). Los datos de configuración incluyen el nombre, el tipo y la versión del servicio junto con los detalles de conexión necesarios, como el número de puerto y las credenciales de inicio de sesión.

**Para comprobar las relaciones en el entorno local**:

1. En el entorno local, muestre las relaciones del entorno activo.

   ```bash
   magento-cloud relationships
   ```

1. Confirme `service` y `type` de la respuesta. La respuesta proporciona información de conexión, como la dirección IP y el número de puerto.

   >Respuesta de muestra abreviada

   ```yaml
   redis:
       -
   ...
           type: 'redis:7.0'
           port: 6379
   elasticsearch:
       -
   ...
           type: 'opensearch:2'
           port: 9200
   database:
       -
   ...
           type: 'mysql:10.6'
           port: 3306
   ```

**Para comprobar las relaciones en entornos remotos**:

1. Utilice SSH para iniciar sesión en el entorno remoto.

1. Enumerar los datos de configuración de relaciones para todos los servicios configurados en el entorno.

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   o bien, use el siguiente comando `ece-tools` para ver las relaciones:

   ```bash
   php ./vendor/bin/ece-tools env:config:show services
   ```

1. Confirme `service` y `type` de la respuesta. La respuesta proporciona información de conexión, como la dirección IP y el número de puerto, así como las credenciales de nombre de usuario y contraseña necesarias.

## Versiones de servicio

La compatibilidad y la versión del servicio para Adobe Commerce en la infraestructura en la nube están determinadas por las versiones implementadas y probadas en la infraestructura en la nube, y a veces difieren de las versiones admitidas por las implementaciones locales de Adobe Commerce. Consulte [Requisitos del sistema](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html?lang=es) en la guía _Instalación_ para obtener una lista de dependencias de software de terceros que el Adobe ha probado con versiones específicas de Adobe Commerce y Magento Open Source.

### Comprobaciones de EOL de software

Durante el proceso de implementación, el paquete `ece-tools` comprueba las versiones de servicio instaladas con las fechas de fin de vida útil (EOL) de cada servicio.

- Si la versión de un servicio se encuentra en los tres meses siguientes a la fecha límite, se muestra una notificación en el registro de implementación.
- Si la fecha límite se sitúa en el pasado, aparece una notificación de advertencia.

Para mantener la seguridad de la tienda, actualice las versiones de software instaladas antes de que lleguen a EOL. Puede revisar las fechas límite en el archivo `eol.yaml` de [ece-tools](https://github.com/magento/ece-tools/blob/develop/config/eol.yaml).

### Migrar a OpenSearch

{{elasticsearch-support}}

Para la versión 2.4.4 y posterior de Adobe Commerce, consulte [Configuración del servicio OpenSearch](opensearch.md).

## Cambiar la versión del servicio

Puede actualizar la versión del servicio instalado para que sea compatible con la versión de Adobe Commerce implementada en su entorno de nube.

No puede actualizar directamente la versión del servicio para un servicio instalado. Sin embargo, puede crear un servicio con la versión requerida. Consulte [Versión del servicio de downgrade](#downgrade-version).

### Actualizar la versión del servicio instalado

Puede actualizar la versión del servicio instalado actualizando la configuración del servicio en el archivo `services.yaml`.

1. Cambiar el valor [`type`](#type) para el servicio en el archivo `.magento/services.yaml`:

   > Definición del servicio original

   ```yaml
   mysql:
       type: mysql:10.3
       disk: 2048
   ```

   > Definición de servicio actualizada

   ```yaml
   mysql:
       type: mysql:10.4
       disk: 5120
   ```

1. Agregue, confirme e inserte los cambios de código.

   ```bash
   git add .magento/services.yaml
   ```

   ```bash
   git commit -m "Upgrade MySQL from MariaDB 10.3 to 10.4."
   ```

   ```bash
   git push origin <branch-name>
   ```

### Versión de downgrade

No puede reducir un servicio instalado directamente. Tiene dos opciones:

1. Cambie el nombre de un servicio existente con la nueva versión, que elimina el servicio y los datos existentes, y agrega uno nuevo.

1. Cree un servicio de y guarde los datos del servicio existente.

Cuando cambie la versión del servicio, debe actualizar la configuración del servicio en el archivo `services.yaml` y actualizar las relaciones en el archivo `.magento.app.yaml`.

**Para reducir la versión de un servicio cambiando el nombre de un servicio existente**:

1. Cambie el nombre del servicio existente en el archivo `.magento/services.yaml` y cambie la versión.

   >[!WARNING]
   >
   >Al cambiar el nombre de un servicio existente, se sustituye y se eliminan todos los datos. Si necesita conservar los datos, cree un servicio en lugar de cambiar el nombre del servicio existente.

   Por ejemplo, para reducir la versión de MariaDB para el servicio _mysql_ de la versión 10.4 a la 10.3, cambie la configuración existente de _service-id_ y _type_.

   > Definición de `services.yaml` original

   ```yaml
   mysql:
       type: mysql:10.4
       disk: 5120
   ```

   > Nueva definición de `services.yaml`

   ```yaml
   mysql2:
        type: mysql:10.3
        disk: 5120
   ```

1. Actualice las relaciones en el archivo `.magento.app.yaml`.

   > Configuración original de `.magento.app.yaml`

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

   > Se actualizó la configuración de `.magento.app.yaml`

   ```yaml
   relationships:
       database: "mysql2:mysql"
   ```

1. Agregue, confirme e inserte los cambios de código.

**Para reducir la categoría de un servicio creando un servicio**:

1. Agregue una definición de servicio al archivo `services.yaml` para su proyecto con la especificación de versión degradada. Consulte _mysql2_ en el siguiente ejemplo:

   > services.yaml

   ```yaml
   mysql:
       type: mysql:10.4
       disk: 5120
   mysql2:
       type: mysql:10.3
       disk: 5120
   ```

1. Cambie la configuración de relaciones en el archivo `.magento.app.yaml` para utilizar el nuevo servicio.

   > Configuración original de `.magento.app.yaml`

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

   > Nueva configuración de `.magento.app.yaml`

   ```yaml
   relationships:
       database: "mysql2:mysql"
   ```

1. Agregue, confirme e inserte los cambios de código.
