---
title: Configuración del servicio Valkey
description: Aprenda a configurar y optimizar Valkey como solución de caché back-end para Adobe Commerce en la infraestructura en la nube, incluido el reemplazo de Redis y la personalización de la configuración del back-end de la caché.
feature: Cloud, Cache, Services
exl-id: f8933e0d-a308-4c75-8547-cb26ab6df947
TQID: https://experienceleague.adobe.com/-aBnwClJGQlRkEfugtChxbjLObLzTu0xl1IvkYUVRsk
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
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: d5d947f9858ab15e2e5daed7848163846580f883
workflow-type: tm+mt
source-wordcount: 701
ht-degree: 0%

---

# Configuración del servicio Valkey

[Valkey](https://valkey.io) es una solución de caché backend opcional para Adobe Commerce en la infraestructura en la nube. Valkey es necesario cuando se anula la configuración de caché predeterminada en Adobe Commerce 2.4.9 y versiones posteriores, o en versiones de parches posteriores a 2.4.5-p16, 2.4.6-p14, 2.4.7-p9 y 2.4.8-p4.

{{service-instruction}}

## Configuración de Valkey

Para reemplazar Redis por Valkey, actualice los siguientes archivos:

- `.magento/services.yaml`
- `.magento.app.yaml`

### Configuración del servicio

En `.magento/services.yaml`, reemplace la definición del servicio Redis por una definición del servicio Valkey. Reemplace `<version>` con una versión de Valkey compatible con su versión de Adobe Commerce y la plantilla de nube actual.

```yaml
cache:
  type: valkey:<version>
```

**Ejemplo**

```yaml
cache:
  type: valkey:8.0
```

La versión de ejemplo no es universal. Las versiones reales del servicio predeterminado y admitido dependen de la versión de Adobe Commerce y de la plantilla de nube actual. Utilice la versión especificada por la plantilla de proyecto actual. Consulte [Configurar servicios](services-yaml.md#service-versions) para obtener más información.

>[!WARNING]
>
>Si cambia el ID de servicio, se elimina el servicio existente y se crea un nuevo servicio. Los datos existentes en el servicio eliminado se eliminan de forma permanente. Haga una copia de seguridad del entorno antes de cambiar el nombre de un servicio.

No dé por hecho que los datos de caché y de sesión persisten al cambiar el valor de `type` de `redis:<version>` a `valkey:<version>`, aunque mantenga el mismo Id. de servicio. Considere la migración como la creación de una nueva caché: no se garantiza que se conserven la caché existente y los datos de sesión, y se cierra la sesión de los usuarios una vez finalizada la migración.

### Configuración de la relación de servicio

En `.magento.app.yaml`, configure la relación entre la aplicación y el servicio Valkey:

```yaml
relationships:
  valkey: "cache:valkey"
```

La clave de relación `valkey` es el nombre que usa la aplicación para obtener acceso al servicio. El valor `cache:valkey` hace referencia al identificador y tipo de servicio definidos en `.magento/services.yaml`.

>[!TIP]
>
>Adobe Commerce se comunica con Valkey a través de la biblioteca de cliente `credis`, que funciona sobre sockets PHP simples de forma predeterminada. Para mejorar el rendimiento, habilite la extensión PHP `redis` en `.magento.app.yaml`. `credis` utiliza la extensión compilada automáticamente cuando está disponible.
>
>```yaml
>runtime:
>   extensions:
>       - redis
>```

### Confirmar e implementar los cambios

Añada, confirme e inserte los cambios de configuración:

```terminal
git add .magento/services.yaml .magento.app.yaml
git commit -m "Enable Valkey service"
git push origin <branch-name>
```

Una vez finalizada la implementación, compruebe que la relación de servicio de Valkey esté disponible.

{{service-change-tip}}

{{valkey-newrelic}}

## Personalización de la configuración de Valkey

Para las recomendaciones de caché, sesión, L2 y conexión de réplica, consulte [Prácticas recomendadas para la configuración de los servicios Valkey y Redis](https://experienceleague.adobe.com/es/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration) en la _Guía de prácticas recomendadas de implementación del libro de estrategias_.

## Verificar la relación de servicio

Para mostrar el objeto `MAGENTO_CLOUD_RELATIONSHIPS` descodificado, ejecute el siguiente comando desde un contenedor de aplicaciones después de implementar la configuración:

Utilice SSH para conectarse al entorno remoto de la nube y, a continuación, ejecute:

```terminal
echo "$MAGENTO_CLOUD_RELATIONSHIPS" | base64 -d | json_pp
```

El comando muestra todas las relaciones de servicio configuradas. Para identificar los detalles de conexión de Valkey, busque la relación de Valkey.

**Ejemplo de salida**

En el siguiente ejemplo abreviado se muestra la relación `valkey`. No es un esquema universal.

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
   "valkey" : [
      {
         "host" : "valkey.internal",
         "port" : 6379,
         "path" : null,
         "scheme" : "valkey"
      }
   ]
}
```

La salida varía según el entorno y la configuración del servicio. No codifique nombres de host, puertos, direcciones IP, nombres de clúster, versiones de servicio, nombres de usuario o contraseñas desde este ejemplo. Use los valores devueltos por `MAGENTO_CLOUD_RELATIONSHIPS` en el entorno de destino.

Si `jq` está disponible, mostrar solo la relación de Valkey:

```terminal
printf '%s' "$MAGENTO_CLOUD_RELATIONSHIPS" \
  | base64 -d \
  | jq '{valkey: .valkey}'
```

Para obtener más información acerca de las relaciones de servicio, vea [Configurar servicios](services-yaml.md).

## Uso de la CLI de Valkey

Si su relación de Valkey se llama `valkey`, use el host y el puerto que `MAGENTO_CLOUD_RELATIONSHIPS` devolvió para conectarse a Valkey:

```terminal
valkey-cli -h <host> -p <port>
```

**Ejemplo**

```terminal
valkey-cli -h valkey.internal -p 6379
```

## Obtener la versión de Valkey instalada

>[!BEGINTABS]

>[!TAB Entorno de integración]

En un entorno de integración, use el host y el puerto devueltos por la relación `valkey` para ejecutar:

```terminal
valkey-cli -h <host> -p <port> info | grep version
```

**Respuesta de ejemplo**

```text
valkey_version:<installed-version>
gcc_version:<gcc-version>
```

La versión y los detalles de la compilación varían según el entorno. No trate una versión de ejemplo mostrada como una versión de servicio obligatorio o universal.

>[!TAB Ensayo y producción profesional]

En entornos de ensayo y producción profesionales, ejecute:

```terminal
valkey-server -v
```

**Respuesta de ejemplo**

```text
Valkey server v=<installed-version> ...
```

La versión y los detalles de la compilación varían según el entorno. No trate una versión de ejemplo mostrada como una versión de servicio obligatorio o universal.

>[!ENDTABS]

## Solución de problemas Valkey

### Los errores de limpieza de caché hacen referencia a Redis en una caché configurada con Valkey

Un error de limpieza de caché previa a la implementación puede mostrar el código de error `[107]` (`clean-redis-cache`) y un mensaje `Connection to Redis` incluso cuando el servicio `cache` está configurado como Valkey. `ece-tools` utiliza este código y mensaje de error para el paso de limpieza de caché, independientemente de si el servicio de caché de copia de seguridad es Redis o Valkey.

Si el error subyacente es un error de DNS, como `Name or service not known` para el host de relación, el paso de implementación se ejecutó antes de que la relación de servicio estuviera disponible o el nombre de relación de `.magento.app.yaml` no coincide con el ID de servicio de `.magento/services.yaml`. Consulte [Verificar la relación de servicio](#verify-the-service-relationship).
