---
title: Configuración del servicio Valkey
description: Aprenda a configurar y optimizar Valkey como solución de caché back-end para Adobe Commerce en infraestructura en la nube.
feature: Cloud, Cache, Services
exl-id: f8933e0d-a308-4c75-8547-cb26ab6df947
source-git-commit: 242582ea61d0d93725a7f43f2ca834db9e1a7c29
workflow-type: tm+mt
source-wordcount: '188'
ht-degree: 0%

---

# Configuración del servicio Valkey

[Valkey](https://valkey.io) es una solución de caché back-end opcional que reemplaza a `Zend Framework Zend_Cache_Backend_File`, que Adobe Commerce usa de forma predeterminada.

Consulte [Configurar Valkey](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/valkey/config-valkey.html){target="_blank"} en la _guía de configuración_.

{{service-instruction}}

**Para habilitar Valkey**:

1. Agregue el nombre y el tipo necesarios al archivo `.magento/services.yaml`.

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

1. Agregue, confirme e inserte los cambios de código.

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable valkey service" && git push origin <branch-name>
   ```

1. [Compruebe las relaciones de servicio](services-yaml.md#service-relationships).

{{service-change-tip}}

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
