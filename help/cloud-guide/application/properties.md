---
title: Propiedades
description: Utilice la lista de propiedades como referencia al configurar la aplicación  [!DNL Commerce] para compilarla e implementarla en la infraestructura de la nube.
feature: Cloud, Configuration, Build, Deploy, Roles/Permissions, Storage
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '816'
ht-degree: 0%

---

# Propiedades para la configuración de aplicaciones

El archivo `.magento.app.yaml` utiliza propiedades para administrar la compatibilidad con el entorno para la aplicación [!DNL Commerce].

| Nombre | Descripción | Predeterminado | Requerido |
| ------ | --------------------------------- | ------- | -------- |
| [`access`](#access) | Personalizar funciones de usuario | — | No |
| [`crons`](crons-property.md) | Actualizar especificaciones y programar trabajos cron | — | No |
| [`dependencies`](#dependencies) | Habilitar dependencias adicionales | `php:composer/composer: '2.2.4'` | No |
| [`disk`](#disk) | Definir el tamaño del disco persistente | `5120` | Sí |
| [`firewall`](firewall-property.md) | (Solo Starter) Controlar el tráfico saliente | — | No |
| [`hooks`](hooks-property.md) | Personalice los comandos del shell para las fases de compilación, implementación y posterior a la implementación | — | No |
| [`mounts`](#mounts) | Definir rutas | Rutas:<ul><li>`"var": "shared:files/var"`</li><li>`"app/etc": "shared:files/etc"`</li><li>`"pub/media": "shared:files/media"`</li><li>`"pub/static": "shared:files/static"`</li></ul> | No |
| [`name`](#name) | Definición del nombre de la aplicación | `mymagento` | Sí |
| [`relationships`](#relationships) | Servicios de mapa | Servicios:<ul><li>`database: "mysql:mysql"`</li><li>`redis: "redis:redis"`</li><li>`opensearch: "opensearch:opensearch"`</li></ul> | No |
| [`runtime`](#runtime) | La propiedad Runtime incluye extensiones requeridas por la aplicación [!DNL Commerce]. | Extensiones:<ul><li>`xsl`</li><li>`newrelic`</li><li>`sodium`</li></ul> | Sí |
| [`type`](#type-and-build) | Establecer la imagen del contenedor base | `php:8.3` | Sí |
| [`variables`](variables-property.md) | Aplicar una variable de entorno para una versión específica de Commerce | — | No |
| [`web`](web-property.md) | Gestión de solicitudes externas | — | Sí |
| [`workers`](workers-property.md) | Gestión de solicitudes externas | — | Sí, si no se utiliza la propiedad web |

{style="table-layout:auto"}

## `name`

La propiedad `name` proporciona el nombre de aplicación utilizado en el archivo [`routes.yaml`](../routes/routes-yaml.md) para definir el flujo ascendente HTTP (de forma predeterminada, `mymagento:http`). Por ejemplo, si el valor de `name` es `app`, debe utilizar `app:http` en el campo ascendente.

>[!WARNING]
>
>No cambie el nombre de la aplicación después de implementarla. Al hacerlo, se pierden datos.

## `type` y `build`

Las propiedades `type` y `build` proporcionan información sobre la imagen del contenedor base para generar y ejecutar el proyecto.

El idioma `type` admitido es PHP. Especifique la versión de PHP de la siguiente manera:

```yaml
type: php:<version>
```

La propiedad `build` determina qué sucede de forma predeterminada al crear el proyecto. `flavor` especifica un conjunto predeterminado de tareas de generación para ejecutar. El siguiente ejemplo muestra la configuración predeterminada para `type` y `build` de `magento-cloud/.magento.app.yaml`:

```yaml
# The toolstack used to build the application.
type: php:8.3
build:
    flavor: none

dependencies:
    php:
        composer/composer: '2.7.2'
```

### Instalación y uso de Composer 2

La propiedad `build: flavor:` no se usa para Composer 2.x; por lo tanto, debe instalar Composer manualmente durante la fase de compilación. Para instalar y usar Composer 2.x en sus proyectos de Starter y Pro, debe realizar tres cambios en la configuración de `.magento.app.yaml`:

1. Elimine `composer` como `build: flavor:` y agregue `none`. Este cambio evita que Cloud use la versión predeterminada 1.x de Composer para ejecutar tareas de compilación.
1. Agregue `composer/composer: '^2.0'` como una dependencia `php` para instalar Composer 2.x.
1. Agregue las tareas de compilación de `composer` a un vínculo `build` para ejecutar las tareas de compilación mediante Composer 2.x.

Use los siguientes fragmentos de configuración en su propia configuración de `.magento.app.yaml`:

```yaml
# 1. Change flavor to none.
build:
    flavor: none

# 2. Add Composer ^2.0 as a php dependency.
dependencies:
    php:
        composer/composer: '^2.0'

# 3. Add a build hook to run the build tasks using Composer 2.x.
hooks:
    build: |
        set -e
        composer --no-ansi --no-interaction install --no-progress --prefer-dist --optimize-autoloader
```

Consulte [Paquetes requeridos](../development/overview.md#required-packages) para obtener más información sobre Composer.

## `dependencies`

Especifique las dependencias que la aplicación pueda necesitar durante el proceso de generación.

Adobe Commerce admite dependencias en los siguientes idiomas:

- PHP
- Rubí
- Node.js

Estas dependencias son independientes de las dependencias finales de la aplicación y están disponibles en `PATH`, durante el proceso de compilación y en el entorno de tiempo de ejecución de la aplicación.

Puede especificar esas dependencias de la siguiente manera:

```yaml
ruby:
   sass: "~3.4"
nodejs:
   grunt-cli: "~0.3"
```

## `runtime`

Use para modificar la configuración de PHP en tiempo de ejecución, como habilitar extensiones. Se requieren las siguientes extensiones:

```yaml
runtime:
    extensions:
        - xsl
        - newrelic
        - sodium
```

Consulte [Configuración de PHP](php-settings.md) para obtener detalles sobre cómo habilitar extensiones.

## `disk`

Define el tamaño del disco persistente de la aplicación en MB.

```yaml
disk: 5120
```

El tamaño mínimo de disco recomendado es de 256 MB. Si ve el error `UserError: Error building the project: Disk size may not be smaller than 128MB`, aumente el tamaño a 256 MB.

>[!NOTE]
>
>Para los entornos de ensayo y producción de Pro, debe [enviar un vale de soporte de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para actualizar la configuración de `mounts` y `disk` de su aplicación. Cuando envíe el ticket, indique los cambios de configuración necesarios e incluya una versión actualizada del archivo `.magento.app.yaml`.
>
>No es posible aumentar temporalmente el almacenamiento en disco en Ensayo o Producción; este proceso no es reversible.

## `relationships`

Define la asignación de servicios en la aplicación.

La relación `name` está disponible para la aplicación en la variable de entorno `MAGENTO_CLOUD_RELATIONSHIPS`. La relación `<service-name>:<endpoint-name>` se asigna a los valores de nombre y tipo definidos en el archivo `.magento/services.yaml`.

```yaml
relationships:
    <name>: "<service-name>:<endpoint-name>"
```

A continuación se muestra un ejemplo de las relaciones predeterminadas:

```yaml
relationships:
    database: "mysql:mysql"
    redis: "redis:redis"
    opensearch: "opensearch:opensearch"
    rabbitmq: "rabbitmq:rabbitmq"
```

Consulte [Servicios](../services/services-yaml.md) para obtener una lista completa de los tipos de servicios y extremos admitidos actualmente.

## `mounts`

Un objeto cuyas claves son rutas relativas a la raíz de la aplicación. El montaje es un área de escritura en el disco para archivos. A continuación se muestra una lista predeterminada de montajes configurados en el archivo `magento.app.yaml` con la sintaxis `volume_id[/subpath]`:

```yaml
 # The mounts that will be performed when the package is deployed.
mounts:
    "var": "shared:files/var"
    "app/etc": "shared:files/etc"
    "pub/media": "shared:files/media"
    "pub/static": "shared:files/static"
```

El formato para agregar el montaje a esta lista es el siguiente:

```bash
"/public/sites/default/files": "shared:files/files"
```

- `shared`: comparte un volumen entre las aplicaciones dentro de un entorno.
- `disk`: define el tamaño disponible para el volumen compartido.

>[!NOTE]
>
>Para los entornos de ensayo y producción de Pro, debe [enviar un vale de soporte de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para actualizar la configuración de `mounts` y `disk` de su aplicación. Cuando envíe el ticket, indique los cambios de configuración necesarios e incluya una versión actualizada del archivo `.magento.app.yaml`.

Puede hacer accesible el Web de montaje agregándolo al bloque de ubicaciones [`web`](web-property.md).

>[!WARNING]
>
>Una vez que el sitio tenga datos, no cambie la parte `subpath` del nombre del montaje. Este valor es el identificador único del área `files`. Si cambia este nombre, perderá todos los datos del sitio almacenados en la ubicación antigua.

## `access`

La propiedad `access` indica un nivel mínimo de rol de usuario al que se permite el acceso SSH a los entornos. Las funciones de usuario disponibles son:

- `admin`: puede cambiar la configuración y ejecutar acciones en el entorno; tiene derechos de _colaborador_ y _visualizador_.
- `contributor`: puede insertar código en este entorno y bifurcar desde el entorno; tiene derechos de _visor_.
- `viewer`: solo puede ver el entorno.

La función de usuario predeterminada es `contributor`, que restringe el acceso SSH de los usuarios con solo _derechos de visor_. Puede cambiar la función del usuario a `viewer` para permitir el acceso SSH a los usuarios con solo _derechos de visor_:

```yaml
access:
    ssh: viewer
```
