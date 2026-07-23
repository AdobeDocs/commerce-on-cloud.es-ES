---
title: Configuración del servicio Valkey
description: Aprenda a configurar y optimizar Valkey como solución de caché back-end para Adobe Commerce en infraestructura en la nube.
feature: Cloud, Cache, Services
exl-id: f8933e0d-a308-4c75-8547-cb26ab6df947
TQID: https://experienceleague.adobe.com/-aBnwClJGQlRkEfugtChxbjLObLzTu0xl1IvkYUVRsk
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: a12aa37d41a68df2fd4c84b589a08e128e5ec103
workflow-type: tm+mt
source-wordcount: 286
ht-degree: 0%

---

# Configuración del servicio Valkey

[Valkey](https://valkey.io) es una solución de caché back-end opcional que reemplaza a `Zend Framework Zend_Cache_Backend_File`, que Adobe Commerce usa de forma predeterminada. Si reemplaza el valor predeterminado en las versiones de Commerce 2.4.9+ o versiones de parches posteriores a las líneas de versión 2.4.5-p16, 2.4.6-p14, 2.4.7-p9 y 2.4.8-p4, debe utilizar Valkey.

Consulte [Configurar Valkey](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration){target="_blank"} en la _Guía de prácticas recomendadas de implementación del manual_.

{{service-instruction}}

**Para reemplazar Redis por Valkey, actualice la configuración en los tres archivos siguientes**:

1. Reemplace la configuración de Redis con el nombre de Valkey necesario y escriba en el archivo `.magento/services.yaml`.

   ```yaml
   cache:
       type: valkey:<version>
   ```

   Para proporcionar su propia configuración de Valkey, agregue una clave `core_config` a su archivo `.magento/services.yaml`:

   ```yaml
   cache:
       type: valkey:8.0
   ```

1. Configure las relaciones en el archivo `.magento.app.yaml`.

   ```yaml
   relationships:
       valkey: "cache:valkey"
   ```

1. Configure `.magento.env.yaml` para reemplazar la configuración de Redis de la siguiente manera:.

   ```yaml
    stage:
        deploy:
        VALKEY_USE_SLAVE_CONNECTION: true
        VALKEY_BACKEND: '\Magento\Framework\Cache\Backend\RemoteSynchronizedCache'
   ```

   >[!TIP]
   >
   >Para Adobe Commerce 2.4.9 y versiones posteriores, puede utilizar la implementación moderna de caché L2 basada en Symfony Cache en lugar de `RemoteSynchronizedCache` configurando `VALKEY_BACKEND: symfony_l2`. Ver [`VALKEY_BACKEND`](../environment/variables-deploy.md#valkey_backend) en la referencia de _Implementar variables_.

1. Agregue, confirme e inserte los cambios de código.

   ```bash
   git add .magento/services.yaml .magento.app.yaml .magento.env.yaml && git commit -m "Enable valkey service" && git push origin <branch-name>
   ```

1. [Compruebe las relaciones de servicio](services-yaml.md#service-relationships).

{{service-change-tip}}

{{valkey-newrelic}}

## Uso de la CLI de Valkey

Suponiendo que su relación de Valkey se llame `valkey`, puede acceder a ella con la herramienta `valkey-cli`.

1. Utilice SSH para conectarse al entorno de integración con Valkey instalado y configurado.

1. Abra un túnel SSH a un host.

   ```bash
   valkey-cli -h valkey.internal
   ```

## Obtener la versión instalada de Valkey

Utilice el siguiente comando para obtener la versión de Valkey instalada en un entorno de integración:

```bash
valkey-cli -h valkey.internal info | grep version
```

Respuesta:

```
valkey_version:8.0.1
gcc_version:12.2.0
```

### Valkey en Pro ensayo y producción

Para obtener la versión de Valkey instalada en un entorno de ensayo o producción, use el comando `valkey-server`:

```bash
valkey-server -v
```

```bash
Valkey server v=8.0.1 ...
```

Utilice el siguiente comando para instalar la configuración de Valkey en un entorno de ensayo o producción de Pro:

```bash
echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
```

Respuesta:

```json
"valkey" : [
    {
        "cluster" : "project-master-abc0003",
        "epoch" : 0,
        "fragment" : null,
        "host" : "valkeycache.internal",
        "host_mapped" : false,
        "hostname" : "oblahblahblahblahe.cache.service._.magentosite.cloud",
        "instance_ips" : [
        "123.456.789.012"
        ],
        "ip" : "123.456.789.012",
        "password" : null,
        "path" : null,
        "port" : 6379,
        "public" : false,
        "query" : {},
        "rel" : "valkey",
        "scheme" : "valkey",
        "service" : "cache",
        "type" : "valkey:8.0",
        "username" : null
    }
]
```
