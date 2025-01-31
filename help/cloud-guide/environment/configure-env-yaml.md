---
title: Configurar el entorno
description: Aprenda a configurar acciones de compilación e implementación en todos los entornos de infraestructura en la nube de Commerce, incluidos el ensayo y la producción profesionales, mediante variables de entorno.
feature: Cloud, Build, Configuration, Deploy, SCD
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '697'
ht-degree: 0%

---

# Configuración de variables de entorno para la implementación

El archivo `.magento.env.yaml` utiliza variables de entorno para centralizar la administración de acciones de generación e implementación en todos los entornos, incluidos el ensayo y la producción de Pro. Para configurar acciones únicas en cada entorno, debe modificar este archivo en cada entorno.

>[!TIP]
>
>Los archivos YAML distinguen entre mayúsculas y minúsculas y no permiten tabulaciones. Tenga cuidado de utilizar una sangría uniforme en todo el archivo `.magento.env.yaml`; de lo contrario, es posible que la configuración no funcione según lo esperado. Los ejemplos de la documentación y del archivo de muestra utilizan la sangría _two-space_. Use [ece-tools validate command](#validate-configuration-file) para comprobar la configuración.

## Estructura de archivos

El archivo `.magento.env.yaml` contiene dos secciones: `stage` y `log`. La sección `stage` controla las acciones que se producen durante las fases del [proceso de implementación en la nube](../deploy/process.md).

- `stage`: utilice la sección de fase para definir determinadas acciones para las siguientes fases de la implementación:
   - `global`: controla las acciones en las fases de compilación, implementación y posterior a la implementación. Puede anular esta configuración en las secciones compilación, implementación y posterior a la implementación.
   - `build`: controla solo las acciones en la fase de compilación. Si no especifica la configuración en esta sección, la fase de compilación utiliza la configuración de la sección global.
   - `deploy`: controla solo las acciones en la fase de implementación. Si no especifica la configuración en esta sección, la fase de implementación utiliza la configuración de la sección global.
   - `post-deploy`: controla las acciones _después de_ implementar la aplicación y _después de_ el contenedor comienza a aceptar conexiones.
- `log`: utilice la sección de registro para configurar [notificaciones](set-up-notifications.md), incluidos los tipos de notificación y el nivel de detalle.
   - `slack`: configure un mensaje para enviarlo a un bot de Slack.
   - `email`: configure un correo electrónico para enviarlo a uno o varios destinatarios de correo electrónico.
   - [controladores de registro](log-handlers.md): configure los mensajes de aplicaciones de hardware y software enviados a un servidor de registro remoto.

### Variables de entorno

El paquete `ece-tools` establece valores en el archivo `env.php` en función de los valores de [variables de nube](variables-cloud.md), las variables establecidas en [!DNL Cloud Console] y el archivo de configuración `.magento.env.yaml`. Las variables de entorno del archivo `.magento.env.yaml` personalizan el entorno de la nube al anular la configuración de Commerce existente. Si un valor predeterminado es `Not Set`, el paquete `ece-tools` toma la acción **NO** y usa el valor predeterminado [!DNL Commerce] o el valor de la configuración MAGENTO_CLOUD_RELATIONSHIPS. Si se establece el valor predeterminado, el paquete `ece-tools` actuará para establecerlo.

Los temas siguientes contienen definiciones detalladas, como si se establece o no un valor predeterminado, de todas las variables que puede utilizar en el archivo `.magento.env.yaml`:

- [Global](variables-global.md): las variables controlan las acciones en cada fase: generar, implementar y después de la implementación
- [Build](variables-build.md): las variables controlan las acciones de compilación
- [Implementar](variables-deploy.md): las variables controlan las acciones de implementación
- [Después de la implementación](variables-post-deploy.md): las variables controlan las acciones después de la implementación

### Crear archivo de configuración desde CLI

Puede generar un archivo de configuración de `.magento.env.yaml` para un entorno de nube mediante los siguientes `ece-tools` comandos.

>Crea un archivo de configuración

```bash
php ./vendor/bin/ece-tools cloud:config:create `<configuration-json>`
```

>Actualizar valores en el archivo de configuración

```bash
php ./vendor/bin/ece-tools cloud:config:update `<configuration-json>`
```

Ambos comandos requieren un solo argumento: una matriz con formato JSON que especifique un valor para al menos una variable de compilación, implementación o posterior a la implementación. Por ejemplo, el comando siguiente establece valores para las variables `SCD_THREADS` y `CLEAN_STATIC_FILES`:

```bash
php vendor/bin/ece-tools cloud:config:create '{"stage":{"build":{"SCD_THREADS":5}, "deploy":{"CLEAN_STATIC_FILES":false}}}'
```

Y crea un archivo `.magento.env.yaml` con la siguiente configuración:

```yaml
stage:
  build:
    SCD_THREADS: 5
  deploy:
    CLEAN_STATIC_FILES: false
```

Puede usar el comando `cloud:config:update` para actualizar el nuevo archivo. Por ejemplo, el comando siguiente cambia el valor `SCD_THREADS` y agrega la configuración `SCD_COMPRESSION_TIMEOUT`:

```bash
php vendor/bin/ece-tools cloud:config:update '{"stage":{"build":{"SCD_THREADS":3, "SCD_COMPRESSION_TIMEOUT":1000}}}'
```

El archivo actualizado contiene la siguiente configuración:

```yaml
stage:
  build:
    SCD_THREADS: 3
    SCD_COMPRESSION_TIMEOUT: 1000
  deploy:
    CLEAN_STATIC_FILES: false
```

### Validar archivo de configuración

Utilice el siguiente comando `ece-tools` para validar el archivo de configuración `.magento.env.yaml` antes de insertar cambios en el entorno de nube remoto.

```bash
php ./vendor/bin/ece-tools cloud:config:validate
```

La siguiente respuesta de ejemplo proporciona una lista de elementos para corregir:

```
Environment configuration is not valid. Correct the following items in your .magento.env.yaml file:
The SCD_THREADS variable contains an invalid value of type string. Use the following type: integer.
The SCD_STRATEGY variable contains an invalid value fast. Use one of the available value options: compact, quick, standard.
The NOT_EXIST_OPTION variable is not allowed in configuration.
```

## Constantes de PHP

Puede utilizar constantes de PHP en definiciones de archivo de `.magento.env.yaml` en lugar de valores de codificación. El ejemplo siguiente define `driver_options` mediante una constante PHP:

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      connection:
        default:
          driver_options:
            !php/const:\PDO::MYSQL_ATTR_LOCAL_INFILE : 1
        indexer:
          driver_options:
            !php/const:\PDO::MYSQL_ATTR_LOCAL_INFILE : 1
      _merge: true
```

>[!WARNING]
>
>El análisis constante no funciona cuando se usa una versión del paquete `symfony/yaml` anterior a la 3.2.

## Control de errores

Cuando se produce un error debido a un valor inesperado en el archivo de configuración de `.magento.env.yaml`, recibe un mensaje de error. Por ejemplo, el siguiente mensaje de error presenta una lista de cambios sugeridos para cada elemento con un valor inesperado, a veces proporcionando opciones válidas:

```
- Environment configuration is not valid. Please correct .magento.env.yaml file with next suggestions:
  Item CRON_CONSUMERS_RUNNER is not supposed to be in stage build. Please move it to one of possible stages: global, deploy
  Item SKIP_SCD has unexpected type string. Please use one of next types: boolean
  Item VERBOSE_COMMANDS has unexpected type boolean. Please use one of next types: string
  Item SKIP_HTML_MINIFICATION has unexpected type string. Please use one of next types: boolean
  Item CRON_CONSUMERS_RUNNER has unexpected type boolean. Please use one of next types: array
  Item VAR_WARM_UP_PAGES is not allowed in configuration.
  Item WARM_UP_PAGES has unexpected type string. Please use one of next types: array
```

Realice las correcciones necesarias, confirme y presione los cambios. Si no recibe un mensaje de error, los cambios realizados en el archivo de configuración superan la validación.

## Optimización de administración de configuración

Si ha habilitado la administración de la configuración después de volcar las configuraciones, debe mover las variables SCD_* de la implementación a la fase de compilación. Consulte [Estrategias de implementación de contenido estático](../deploy/static-content.md).

>Antes de la administración de configuración:

```yaml
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      consumers: []
    SCD_STRATEGY: compact
    SCD_MATRIX:
      ...
    REDIS_USE_SLAVE_CONNECTION: 1
```

>Después de habilitar la administración de la configuración, mueva las variables SCD_* a la fase de compilación:

```yaml
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      consumers: []
    REDIS_USE_SLAVE_CONNECTION: 1
  build:
    SCD_STRATEGY: compact
    SCD_MATRIX:
      ...
```
