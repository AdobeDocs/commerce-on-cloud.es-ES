---
title: Variables de compilación
description: Consulte la lista de variables de entorno que controlan las acciones en la fase de creación de la infraestructura en la nube de Adobe Commerce.
feature: Cloud, Configuration, Build, SCD, Upgrade
recommendations: noDisplay, catalog
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '920'
ht-degree: 0%

---

# Variables de compilación

Las siguientes variables _build_ controlan acciones en la fase de compilación y pueden heredar y anular valores de las [variables globales](variables-global.md). Inserte estas variables en la fase `build` del archivo `.magento.env.yaml`:

```yaml
stage:
  build:
    BUILD_VARIABLE_NAME: value
```

Para obtener más información sobre cómo personalizar el proceso de generación e implementación:

- [Configuración de implementación](configure-env-yaml.md)
- [Proceso de implementación](../deploy/process.md)

Las siguientes variables se eliminaron en la versión 2.2:

- `skip_di_clearing`
- `skip_di_compilation`

## `ERROR_REPORT_DIR_NESTING_LEVEL`

- **Predeterminado**—`1`
- **Versión**: Adobe Commerce 2.1.4 y posterior

Defina el nivel de anidamiento de directorios para guardar archivos de informes de errores a fin de evitar rellenar el directorio de informes con decenas de miles de archivos, lo que puede dificultar la administración y revisión de los datos. El valor predeterminado de esta configuración es `1`. Normalmente, no es necesario cambiar el valor predeterminado a menos que tenga problemas para administrar los archivos de informe de errores en el directorio `<magento_root>/var/report/`.

```yaml
stage:
  build:
    ERROR_REPORT_DIR_NESTING_LEVEL: 2
```

## `QUALITY_PATCHES`

- **Predeterminado**—_No establecido_
- **Versión**: Adobe Commerce 2.1.4 y posterior

Especifique una lista de parches de calidad de Adobe Commerce para aplicar durante la implementación.

```yaml
stage:
  build:
    QUALITY_PATCHES: [ ]
```

El ejemplo siguiente especifica tres parches que se deben aplicar durante la implementación.

```yaml
stage:
  build:
    QUALITY_PATCHES:
      - MC-31387
      - MDVA-4567
      - MC-456345
```

Ver [Aplicar parches](../development/apply-patches.md).

## `SCD_COMPRESSION_LEVEL`

- **Predeterminado**—`6`
- **Versión**: Adobe Commerce 2.1.4 y posterior

Especifica el nivel de compresión [gzip](https://www.gnu.org/software/gzip) (`0` a `9`) que se usará para comprimir contenido estático; `0` deshabilita la compresión.

```yaml
stage:
  build:
    SCD_COMPRESSION_LEVEL: 4
```

## `SCD_COMPRESSION_TIMEOUT`

- **Predeterminado**—`600`
- **Versión**: Adobe Commerce 2.1.4 y posterior

Cuando el tiempo necesario para comprimir los recursos estáticos supera el límite de tiempo de espera de compresión, interrumpe el proceso de implementación. Establezca el tiempo máximo de ejecución, en segundos, para el comando de compresión de contenido estático.

```yaml
stage:
  build:
    SCD_COMPRESSION_TIMEOUT: 800
```

## `SCD_NO_PARENT`

- **Predeterminado**—`false`
- **Versión**: Adobe Commerce 2.4.2 y posterior

Se establece en `true` para evitar la generación de contenido estático para las temáticas principales durante la fase de compilación.

Establezca `SCD_NO_PARENT: false` durante la fase de compilación para que la generación de contenido estático para los temas principales no afecte a la implementación del sitio ni cause un tiempo de inactividad innecesario del sitio. Consulte [Implementación de contenido estático](../deploy/static-content.md).

```yaml
stage:
  build:
    SCD_NO_PARENT: false
```

## `SCD_MATRIX`

- **Predeterminado**—_No establecido_
- **Versión**: Adobe Commerce 2.1.4 y posterior

Puede configurar varias configuraciones regionales por tema. Esta personalización ayuda a acelerar el proceso de compilación al reducir el número de archivos de temas innecesarios. Por ejemplo, puede generar el tema _magento/backend_ en inglés y un tema personalizado en otros idiomas.

En el siguiente ejemplo se crea el tema `Magento/backend` con tres configuraciones regionales:

```yaml
stage:
  build:
    SCD_MATRIX:
      "Magento/backend":
        language:
          - en_US
          - fr_FR
          - af_ZA
```

El siguiente ejemplo crea tres temas con tres configuraciones regionales:

```yaml
stage:
  build:
    SCD_MATRIX:
      "Magento/backend":
        language:
          - en_US
          - fr_FR
          - af_ZA
      "Magento/blank":
        language:
          - en_US
          - fr_FR
          - af_ZA
      "Magento/luma":
        language:
          - en_US
          - fr_FR
          - af_ZA
```

O bien, puede elegir _no_ implementar un tema:

```yaml
stage:
  build:
    SCD_MATRIX:
      "Magento/backend": [ ]
```

## `SCD_MAX_EXECUTION_TIME`

- **Predeterminado**—_No establecido_
- **Versión**—Adobe Commerce 2.2.0 y posterior

Permite aumentar el tiempo de ejecución máximo esperado para la implementación de contenido estático.

De forma predeterminada, Adobe Commerce en la infraestructura en la nube establece la ejecución máxima esperada en 900 segundos, pero en algunos casos puede necesitar más tiempo para completar la implementación de contenido estático para un proyecto en la nube.

```yaml
stage:
  build:
    SCD_MAX_EXECUTION_TIME: 3600
```

{{scd-timing-warning}}

## `SCD_STRATEGY`

- **Predeterminado**—`quick`
- **Versión**—Adobe Commerce 2.2.0 y posterior

Personalice la [estrategia de implementación](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-strategy.html?lang=es) para el contenido estático. Consulte [Implementar archivos de vista estática](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html?lang=es).

Utilice estas opciones _solamente_ si tiene más de una configuración regional:

- `standard`: implementa todos los archivos de vista estática para todos los paquetes.
- `quick`—(_default_) minimiza el tiempo de implementación.
- `compact`: conserva espacio en disco en el servidor. En la versión 2.2.4 y anteriores de Adobe Commerce, esta configuración anula el valor de `scd_threads` con un valor de `1`.

```yaml
stage:
  build:
    SCD_STRATEGY: "compact"
```

## `SCD_THREADS`

- **Predeterminado**—Automático
- **Versión**: Adobe Commerce 2.1.4 y posterior

Establece el número de subprocesos para la implementación de contenido estático. El valor predeterminado se establece en función del recuento de subprocesos de CPU detectado y no supera un valor de 4. El aumento del número de subprocesos acelera la implementación de contenido estático; reducir el número de subprocesos hace que se ralentice. Puede establecer el valor del subproceso, por ejemplo:

```yaml
stage:
  build:
    SCD_THREADS: 2
```

Para reducir aún más el tiempo de implementación, use [Administración de configuración](../store/store-settings.md) con el comando `scd-dump` para mover la implementación estática a la fase de compilación.

## `SCD_USE_BALER`

- **Predeterminado**—_No establecido_
- **Versión**: Adobe Commerce 2.3.0 y posterior

[Baler](https://github.com/magento/baler) analiza el código de JavaScript generado y crea un paquete de JavaScript optimizado. La implementación del paquete optimizado en el sitio puede reducir el número de solicitudes de red al cargar el sitio y mejorar los tiempos de carga de las páginas.

Se establece en `true` para ejecutar Baler después de realizar la implementación de contenido estático.

```yaml
stage:
  build:
    SCD_USE_BALER: true
```

>[!NOTE]
>
>Como Baler está en versión alfa, no es aconsejable utilizarlo en entornos de producción.

## `SKIP_COMPOSER_DUMP_AUTOLOAD`

- **Predeterminado**— _No establecido_
- **Versión**: Adobe Commerce 2.1.4 y posterior

Establezca el valor en `true` para omitir el comando `composer dump-autoload` durante una instalación de Cloud Docker. Esta variable solo es relevante para los contenedores de Cloud Docker con sistemas de archivos editables. En estos casos, omitir el comando evita errores de otros comandos que intentan obtener acceso al código desde el directorio `generated` eliminado.

Cuando Adobe Commerce ejecuta `composer dump-autoload`, crea archivos de carga automática con vínculos a clases generadas en la carpeta `generated`, lo que no supone un problema en los entornos de producción con sistemas de archivos de solo lectura. Sin embargo, para las instalaciones de Cloud Docker con sistemas de archivos editables (creados únicamente para pruebas y desarrollo con `./vendor/bin/ece-docker build:compose --with-test`), puede ejecutar el comando `bin/magento -n setup:upgrade` sin la opción `--keep-generated`, que elimina el directorio `generated`. Si se elimina el directorio, el comando `composer dump-autoload` produce un error porque la carga automática contiene vínculos a los archivos del directorio eliminado.

```yaml
stage:
  build:
    SKIP_COMPOSER_DUMP_AUTOLOAD: true
```

## `SKIP_SCD`

- **Predeterminado**— _No establecido_
- **Versión**: Adobe Commerce 2.1.4 y posterior

Se establece en `true` para omitir la implementación de contenido estático durante la fase de compilación.

Si ya implementa contenido estático durante la fase de compilación con [Administración de configuración](../store/store-settings.md), puede omitir la implementación de contenido estático para una prueba de compilación rápida.

En la fase de compilación, establezca `SKIP_SCD: false` de modo que la compilación de contenido estático se produzca durante la fase de compilación en la que el proceso no afecte a la implementación del sitio ni cause un tiempo de inactividad innecesario en el sitio. Consulte [Implementación de contenido estático](../deploy/static-content.md).

```yaml
stage:
  build:
    SKIP_SCD: false
```

## `VERBOSE_COMMANDS`

- **Predeterminado**—_No establecido_
- **Versión**: Adobe Commerce 2.1.4 y posterior

Habilite o deshabilite el nivel de detalle de depuración de [Symfony](https://symfony.com/doc/current/console/verbosity.html) para los comandos CLI de `bin/magento` ejecutados durante la fase de implementación.

>[!NOTE]
>
>Para usar VERBOSE_COMMANDS para controlar los detalles de la salida del comando para los comandos CLI `bin/magento` correctos y fallidos, debe establecer [MIN_LOGGING_LEVEL](variables-global.md#minlogginglevel) `debug`.

Elija el nivel de detalle proporcionado en los registros:

- `-v`= salida normal
- `-vv`= salida más detallada
- `-vvv` = resultado detallado ideal para la depuración

```yaml
stage:
  build:
    VERBOSE_COMMANDS: "-vv"
```
