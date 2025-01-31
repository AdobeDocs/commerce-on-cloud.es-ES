---
title: Configuración de PHP
description: Obtenga información sobre la configuración óptima de PHP para la configuración de aplicaciones de Commerce en la infraestructura en la nube.
feature: Cloud, Configuration, Extensions
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '536'
ht-degree: 0%

---

# Configuración de PHP

Puede elegir la [versión de PHP](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) que se ejecutará en el archivo `.magento.app.yaml`:

```yaml
name: mymagento
type: php:<version>
```

>[!TIP]
>
>Si actualiza a PHP 8.1 y versiones posteriores, elimine JSON de la propiedad [`runtime: extensions:` ](properties.md#runtime) en el archivo `.magento.app.yaml` y vuelva a implementar. La extensión JSON viene instalada en el entorno de la nube desde PHP 8.0.

## Configuración de PHP

Puede personalizar la configuración de PHP para su entorno usando un archivo de `php.ini` que se anexa a la configuración mantenida por Adobe Commerce.

En el repositorio, agregue el archivo `php.ini` a la raíz de la aplicación (la raíz del repositorio).

>[!TIP]
>
>Configurar PHP incorrectamente puede causar problemas, por lo que solo los administradores avanzados deben configurar estas opciones.

### Aumentar el límite de memoria PHP

Para aumentar el límite de memoria PHP, agregue el siguiente ajuste al archivo `php.ini`:

```ini
memory_limit = 1G
```

Para la depuración, aumente el valor a 2G.

### Optimizar configuración de realpath_cache

Establezca la siguiente configuración de `realpath_cache` para mejorar el rendimiento de la aplicación.

```conf
;
; Increase realpath cache size
;
realpath_cache_size = 10M

;
; Increase realpath cache ttl
;
realpath_cache_ttl = 7200
```

Estos ajustes permiten a los procesos de PHP almacenar en caché las rutas a los archivos en lugar de buscarlos para cada carga de página. Consulte [Ajuste del rendimiento](https://www.php.net/manual/en/ini.core.php) en la documentación de PHP.

>[!NOTE]
>
>Para obtener una lista de los ajustes de configuración de PHP recomendados, consulte [Ajustes de PHP requeridos](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/php-settings.html) en la _Guía de instalación_.

### Compruebe la configuración personalizada de PHP

Después de insertar los cambios de `php.ini` en su entorno de nube, puede comprobar que la configuración personalizada de PHP se haya agregado a su entorno. Por ejemplo, utilice SSH para iniciar sesión en el entorno remoto y ver el archivo con algo similar a lo siguiente:

```bash
cat /etc/php/<php-version>/fpm/php.ini
```

>[!WARNING]
>
>Si usa Cloud Docker para Commerce para el desarrollo local, consulte [Contenedores del servicio Docker](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#fpm-container) para obtener información sobre el uso de un archivo `php.ini` personalizado en un entorno Docker.

## Habilitar extensiones

Puede habilitar o deshabilitar las extensiones PHP en la sección `runtime:extension`. Además, las extensiones especificadas están disponibles en los contenedores Docker PHP.

>[!IMPORTANT]
>
>Antes de habilitar las extensiones, es importante comprender que la versión de PHP debe ser compatible con el sistema operativo que aloja el proyecto. Es posible que el entorno del proyecto requiera una actualización del sistema operativo por parte del equipo de infraestructura para poder continuar.

Ejemplo en archivo `.magento.app.yaml`:

```yaml
runtime:
    extensions:
        - sockets
        - sodium
        - ssh2
    disabled_extensions:
        - bcmath
        - bz2
        - calendar
        - exif
```

Utilice SSH para iniciar sesión en un entorno y enumerar las extensiones PHP.

```bash
php -m
```

Para obtener detalles sobre una extensión PHP específica, consulte la [Lista de extensiones PHP](https://www.php.net/manual/en/extensions.alphabetical.php).

La siguiente tabla muestra las extensiones PHP compatibles al implementar Adobe Commerce en la plataforma Cloud.

{{$include /help/_includes/templated/php-extensions-cloud.md}}

Los requisitos del módulo PHP están vinculados a la versión de Adobe Commerce. Ver [requisitos de PHP](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/php-settings.html).

### Compatibilidad con extensiones

Para proyectos Pro, las siguientes extensiones requieren compatibilidad adicional para su instalación:

- `ioncube`
- `sourceguardian`

Por ejemplo, para configurar PHP para que ejecute únicamente scripts protegidos por SourceGuardian en todos los entornos, se debe establecer la siguiente opción en el archivo `php.ini`:

```ini
[SourceGuardian]
sourceguardian.restrict_unencoded = "1"
```

Consulte [sección 3.5 de la documentación de SourceGuardian](https://sourceguardian.com/demofiles/files/SourceGuardian%20for%20Linux%20User%20Manual.pdf). _Esto es un enlace a un PDF_.

[Envíe un ticket de soporte de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para obtener ayuda con la instalación de estas extensiones PHP en todos los entornos de producción y entornos de ensayo profesional. Incluya su archivo `.magento/services.yaml` actualizado, archivo `.magento.app.yaml` con la versión actualizada de PHP y cualquier extensión adicional de PHP. Para realizar cambios en un entorno de producción activo, debe proporcionar un aviso mínimo de 48 horas. El equipo de infraestructura en la nube puede tardar hasta 48 horas en actualizar el proyecto.

>[!WARNING]
>
>No se admite PHP compilado con depuración y el Sondeo puede entrar en conflicto con [!DNL XDebug] o [!DNL XHProf]. Deshabilite esas extensiones al habilitar el sondeo. El sondeo entra en conflicto con algunas extensiones de PHP como [!DNL Pinba] o IonCube.
