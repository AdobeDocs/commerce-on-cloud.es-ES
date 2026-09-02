---
title: Configurar el servicio Redis
description: Aprenda a configurar y optimizar Redis como solución de caché back-end para Adobe Commerce en infraestructuras en la nube.
feature: Cloud, Cache, Services
exl-id: be6f2462-0878-47e3-b906-ebdd4aa319f2
TQID: https://experienceleague.adobe.com/Q3w1Y1sRuQSwqmbxGfEBavrvHe0ecI9qWJjsfVc2yPU
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: df2792f9d653c4561e4e40cbc71499095f63ff71
workflow-type: tm+mt
source-wordcount: 710
ht-degree: 0%

---

# Configurar el servicio Redis

[Redis](https://redis.io) es una solución de caché back-end opcional que reemplaza a `Zend Framework Zend_Cache_Backend_File`, que Adobe Commerce usa de forma predeterminada.

>[!IMPORTANT]
>
>La caché de Redis no es compatible con Adobe Commerce 2.4.9 ni con versiones de parches posteriores a 2.4.5-p16, 2.4.6-p14, 2.4.7-p9 y 2.4.8-p4. Use [Valkey](valkey.md) para la configuración de la caché donde no se admita Redis. Consulte [Requisitos del sistema](https://experienceleague.adobe.com/en/docs/commerce-operations/installation-guide/system-requirements) para ver los servicios de caché admitidos por versión.

{{service-instruction}}

## Habilitar Redis

Para habilitar Redis, actualice los siguientes archivos:

- `.magento/services.yaml`
- `.magento.app.yaml`

### Configuración del servicio

En `.magento/services.yaml`, agregue la definición del servicio Redis. Reemplazar `<version>` por una versión de Redis compatible con su versión de Adobe Commerce y la plantilla de nube actual.

```yaml
cache:
  type: redis:<version>
```

Por ejemplo, para una versión de Commerce y una plantilla de Cloud que admitan Redis 7.2:

```yaml
cache:
  type: redis:7.2
```

La versión de ejemplo no es universal. Las versiones reales del servicio predeterminado y admitido dependen de la versión de Adobe Commerce, el nivel de parche y la plantilla de Cloud actual. Compruebe la combinación admitida en [Requisitos del sistema](https://experienceleague.adobe.com/en/docs/commerce-operations/installation-guide/system-requirements) y la plantilla de proyecto actual.

### Configuración de la relación de servicio

En `.magento.app.yaml`, configure la relación entre la aplicación y el servicio Redis:

```yaml
runtime:
  extensions:
    - redis

relationships:
  redis: "cache:redis"
```

La clave de relación `redis` es el nombre que usa la aplicación para obtener acceso al servicio. El valor `cache:redis` consiste en el id. de servicio (`cache`) y el tipo de servicio (`redis`) definidos en `.magento/services.yaml`.

### Confirmar e implementar los cambios

Añada, confirme e inserte los cambios de configuración:

```terminal
git add .magento/services.yaml .magento.app.yaml
git commit -m "Enable Redis service"
git push origin <branch-name>
```

Una vez finalizada la implementación, compruebe que la relación de servicio de Redis esté disponible.

{{service-change-tip}}

## Verificar la relación de servicio

Después de implementar la configuración, ejecute el siguiente comando desde un contenedor de aplicaciones para mostrar el objeto `MAGENTO_CLOUD_RELATIONSHIPS` descodificado:

Utilice SSH para conectarse al entorno remoto de la nube y, a continuación, ejecute:

```terminal
echo "$MAGENTO_CLOUD_RELATIONSHIPS" | base64 -d | json_pp
```

El comando muestra todas las relaciones de servicio configuradas. Busque la relación `redis` para identificar los detalles de conexión de Redis.

En el siguiente ejemplo abreviado se muestra la relación `redis`. No es un esquema universal.

```json
{
   "database" : [
      {
         "host" : "database.internal",
         "port" : 3306,
         "path" : "main",
         "scheme" : "mysql"
      }
   ],
   "opensearch" : [
      {
         "host" : "opensearch.internal",
         "port" : 9200,
         "path" : null,
         "scheme" : "http"
      }
   ],
   "redis" : [
      {
         "host" : "redis.internal",
         "port" : 6379,
         "path" : null,
         "scheme" : "redis"
      }
   ]
}
```

La salida varía según el entorno y la configuración del servicio. No codifique nombres de host, puertos, direcciones IP, nombres de clúster, versiones de servicio, nombres de usuario o contraseñas desde este ejemplo. Use los valores devueltos por `MAGENTO_CLOUD_RELATIONSHIPS` en el entorno de destino.

Si `jq` está disponible, utilice el siguiente comando para mostrar únicamente la relación de Redis:

```terminal
printf '%s' "$MAGENTO_CLOUD_RELATIONSHIPS" \
  | base64 -d \
  | jq '{redis: .redis}'
```

Para obtener más información acerca de las relaciones de servicio, vea [Configurar servicios](services-yaml.md).

## Personalizar la configuración de Redis

Para las recomendaciones de caché, sesión, L2 y conexión de réplica, consulte [Prácticas recomendadas para la configuración de los servicios Valkey y Redis](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration) en la _Guía de prácticas recomendadas de implementación del libro de estrategias_.

## Uso de la CLI de Redis

Suponiendo que su relación con Redis se llame `redis`, use el host y el puerto que `MAGENTO_CLOUD_RELATIONSHIPS` devolvió para conectarse a Redis.

Conéctese al entorno con Redis instalado y configurado y ejecute el siguiente comando:

```terminal
redis-cli -h <host> -p <port>
```

**Ejemplo**

```terminal
redis-cli -h redis.internal -p 6379
```

## Obtener la versión de Redis instalada

>[!BEGINTABS]

>[!TAB Entorno de integración]

En un entorno de integración, use el host y el puerto devueltos por la relación `redis` para ejecutar:

```terminal
redis-cli -h <host> -p <port> info | grep version
```

**Respuesta de ejemplo**

```text
redis_version:<installed-version>
gcc_version:<gcc-version>
```

La versión y los detalles de la compilación varían según el entorno. No trate una versión de ejemplo mostrada como una versión de servicio obligatorio o universal.

>[!TAB Ensayo y producción profesional]

En entornos de ensayo y producción profesionales, ejecute:

```terminal
redis-server -v
```

**Respuesta de ejemplo**

```text
Redis server v=<installed-version> ...
```

La versión y los detalles de la compilación varían según el entorno. No trate una versión de ejemplo mostrada como una versión de servicio obligatorio o universal.

>[!ENDTABS]

## Solución de problemas de Redis

Consulte los siguientes artículos de soporte de Adobe Commerce para obtener ayuda sobre la resolución de problemas de Redis:

- [Alertas administradas en Adobe Commerce: alerta de advertencia de memoria Redis](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/managed-alerts-for-adobe-commerce/managed-alerts-on-magento-commerce-redis-memory-warning-alert)
- [Alertas administradas en Adobe Commerce: leer alerta crítica de memoria](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/managed-alerts-for-adobe-commerce/managed-alerts-on-magento-commerce-redis-memory-critical-alert)

### Los errores de limpieza de caché hacen referencia a Redis en una caché configurada con Valkey

Un error de limpieza de caché previa a la implementación puede mostrar el código de error `[107]` (`clean-redis-cache`) y un mensaje `Connection to Redis` incluso cuando el servicio `cache` está configurado como Valkey. `ece-tools` usa este mensaje y código de error heredados orientados a Redis para el paso de limpieza de caché independientemente del servicio que respalde la relación `cache`, de modo que la redacción no indica que Redis esté instalado.

Si el error subyacente es un error de DNS, como `Name or service not known` para el host de relación, el paso de implementación se ejecutó antes de que la relación de servicio estuviera disponible o el nombre de relación de `.magento.app.yaml` no coincide con el ID de servicio de `.magento/services.yaml`. Consulte [Verificar la relación de servicio](#verify-the-service-relationship).
