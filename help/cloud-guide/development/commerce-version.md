---
title: Actualizar la versión de Commerce
description: Obtenga información sobre cómo actualizar la versión de Adobe Commerce en el entorno de la infraestructura en la nube.
feature: Cloud, Upgrade
exl-id: 0cc070cf-ab25-4269-b18c-b2680b895c17
source-git-commit: fe1da39c1d00d74d3f116423e06d11cefd3c2659
workflow-type: tm+mt
source-wordcount: '919'
ht-degree: 0%

---

# Actualizar la versión de Commerce

Puede actualizar el código base de Adobe Commerce a una versión más reciente. Antes de actualizar el entorno, revise los [requisitos del sistema](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html?lang=es) en la guía de _Instalación_ para conocer los requisitos de la última versión de software.

Según el tipo de entorno (desarrollo, ensayo o producción), las tareas de actualización pueden incluir las siguientes:

- Actualice el archivo `.magento/services.yaml` con nuevas versiones para MariaDB (MySQL), OpenSearch, RabbitMQ y Redis para comprobar la compatibilidad con las nuevas versiones de Adobe Commerce. Para los proyectos Pro, debe enviar un ticket de asistencia de Adobe Commerce para instalar o actualizar servicios en entornos de ensayo y producción.
- Actualice el archivo `.magento.app.yaml` con la nueva configuración para los vínculos y las variables de entorno.
- Actualice las extensiones de terceros a la última versión compatible.

{{upgrade-tip}}

{{pro-update-service}}

## Archivos de configuración

Antes de actualizar la aplicación, debe actualizar los archivos de configuración del proyecto para tener en cuenta los cambios en los valores de configuración predeterminados de Adobe Commerce en la infraestructura en la nube o en la aplicación. Los valores predeterminados más recientes se encuentran en el [repositorio de GitHub de Magento en la nube](https://github.com/magento/magento-cloud).

### composer.json

Antes de actualizar, compruebe siempre que las dependencias del archivo `composer.json` sean compatibles con la versión de Adobe Commerce.

Para actualizar el archivo `composer.json` para Adobe Commerce versión 2.4.4 y posterior**:

1. Agregar los(as) siguientes `allow-plugins` a la sección `config`:

   ```json
   "config": {
      "allow-plugins": {
         "dealerdirect/phpcodesniffer-composer-installer": true,
         "laminas/laminas-dependency-plugin": true,
         "magento/*": true
      }
   },
   ```

1. Agregue el siguiente complemento a la sección `require`:

   ```json
   "require": {
       "magento/composer-root-update-plugin": "^2.0.3"
   },
   ```

1. Agregue el siguiente componente a la sección `extra:component_paths`:

   ```json
   "extra": {
      "component_paths": {
         "tinymce/tinymce": "lib/web/tiny_mce_5"
      },
   },
   ```

1. Guarde el archivo. Aún no confirme ni inserte cambios en la rama.

1. Continúe con el proceso de actualización.

## Copia de seguridad de entorno

Se recomienda crear una copia de seguridad de la instancia antes de una actualización. Siga estos pasos para realizar una copia de seguridad de los entornos de integración, ensayo y producción.

**Para hacer una copia de seguridad de la base de datos y el código del entorno de integración**:

1. Cree una copia de seguridad local de la base de datos remota.

   ```bash
   magento-cloud db:dump
   ```

   >[!NOTE]
   >
   >El comando `magento-cloud db:dump` ejecuta el comando [mysqldump](https://dev.mysql.com/doc/refman/8.0/en/mysqldump.html) con el indicador `--single-transaction`, lo que le permite hacer una copia de seguridad de la base de datos sin bloquear las tablas.

1. Haga una copia de seguridad del código y los medios.

   ```bash
   php bin/magento setup:backup --code [--media]
   ```

   Opcionalmente, puede omitir `[--media]` si tiene un gran número de archivos estáticos que ya están en el control de código fuente.

**Para hacer una copia de seguridad de la base de datos del entorno de ensayo o producción antes de implementar**:

1. Utilice SSH para iniciar sesión en el entorno remoto.

1. Crear un [volcado de base de datos](../storage/database-dump.md). Para elegir un directorio de destino para el volcado de la base de datos, utilice la opción `--dump-directory`.

   ```bash
   vendor/bin/ece-tools db-dump
   ```

   La operación de volcado crea un archivo de `dump-<timestamp>.sql.gz` en el directorio del proyecto remoto. Consulte [Copia de seguridad de la base de datos](../storage/database-dump.md).

## Actualización de aplicación

Revise la información de [versiones de servicio](../services/services-yaml.md#service-versions) para conocer los requisitos de la versión de software más reciente antes de actualizar la aplicación.

**Para actualizar la versión de la aplicación**:

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. Establezca la [restricción de versión](overview.md#cloud-metapackage) para la versión de actualización de destino. Este paso solo es necesario si la versión de destino está fuera de la restricción existente.

   ```bash
   composer require-commerce "magento/magento-cloud-metapackage":">=CURRENT_VERSION <NEXT_VERSION" --no-update
   ```

   >[!NOTE]
   >
   >Debe usar la sintaxis de restricción de versión para actualizar correctamente el paquete `ece-tools`. Puede encontrar la restricción de versión en el archivo `composer.json` para la versión de la [plantilla de aplicación](https://github.com/magento/magento-cloud/blob/master/composer.json) que está utilizando para la actualización.

1. Actualice el archivo `composer.json` con la versión de actualización principal de Commerce.

   ```bash
   composer require-commerce magento/product-enterprise-edition 2.4.8 --no-update
   ```

1. Si utiliza B2B, actualice el archivo `composer.json` con la [versión compatible](https://experienceleague.adobe.com/es/docs/commerce-operations/release/product-availability#adobe-authored-extensions) para Commerce.

   ```bash
   composer require-commerce magento/extension-b2b 1.5.2 --no-update
   ```

1. Actualizar dependencias del proyecto.

   ```bash
   composer update
   ```

1. Revise los parches que se aplican actualmente:

   - Si hay parches instalados en el directorio `m2-hotfixes`, [envíe un ticket de soporte de Adobe Commerce](https://experienceleague.adobe.com/es/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#support-case) y trabaje con el soporte técnico de Adobe Commerce para comprobar qué parches se pueden seguir aplicando a la nueva versión. Quite los parches no aplicables del directorio `m2-hotfixes`.

   - Si hay [Parches de calidad] aplicados en el archivo `.magento.env.yaml`, compruebe si aún se pueden aplicar a la nueva versión. Quite los parches no aplicables de la sección `QUALITY_PATCHES` del archivo `.magento.env.yaml`.

   **Método 1**: [Compruebe las versiones aplicables en las notas de la versión de parches de calidad](https://experienceleague.adobe.com/es/docs/commerce-operations/tools/quality-patches-tool/release-notes)

   **Método 2**: [Ver parches y estado disponibles](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/develop/upgrade/apply-patches#view-available-patches-and-status)

   **Método 3**: [Buscar parches](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html?lang=es)


1. Agregar, confirmar y enviar cambios de código.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Upgrade"
   ```

   ```bash
   git push origin <branch-name>
   ```

   `git add -A` es necesario para agregar todos los archivos modificados al control de código fuente debido a la forma en que Composer calcula los paquetes base. Tanto `composer install` como `composer update` calculan las referencias de los archivos del paquete base (`magento/magento2-base` y `magento/magento2-ee-base`) en la raíz del paquete.

   Los archivos a los que Composer calcula las referencias pertenecen a la nueva versión de Adobe Commerce, para sobrescribir la versión obsoleta de esos mismos archivos. En la actualidad, el cálculo de referencias está deshabilitado en Adobe Commerce, por lo que debe agregar los archivos para calcular referencias al control de código fuente.

1. Espere a que se complete la implementación.

1. Compruebe la actualización en el entorno de integración, ensayo o producción utilizando SSH para iniciar sesión y comprobar la versión.

   ```bash
   php bin/magento --version
   ```

### Actualización de extensiones

Revise las páginas de módulos y extensiones de terceros en Marketplace u otros sitios de la empresa y compruebe la compatibilidad con Adobe Commerce y Adobe Commerce en la infraestructura en la nube. Si debe actualizar cualquier extensión y módulo de terceros, Adobe recomienda trabajar en una nueva rama de integración con las extensiones deshabilitadas.

**Para verificar y actualizar sus extensiones**:

1. Cree una rama en la estación de trabajo local.

1. Deshabilite las extensiones según sea necesario.

1. Cuando esté disponible, descargue las actualizaciones de extensión.

1. Instale la actualización según lo documentado por la documentación de terceros.

1. Habilite y pruebe la extensión de.

1. Agregue, confirme e inserte los cambios de código en el control remoto.

1. Envíe a y pruebe en su entorno de integración.

1. Insertar en el entorno de ensayo para realizar pruebas en un entorno de preproducción.

Adobe recomienda encarecidamente actualizar el entorno de producción _antes de_, incluidas las extensiones actualizadas en el proceso de lanzamiento del sitio.

>[!NOTE]
>
>Al actualizar la versión de la aplicación, el proceso de actualización se actualiza automáticamente a la última versión del [módulo CDN de Fastly](../cdn/fastly.md#fastly-cdn-module-for-magento-2).

## Solución de problemas de actualización

Si la actualización falla, recibe un mensaje de error en el explorador que indica que no puede acceder a la tienda o al panel Administración:

```
There has been an error processing your request
Exception printing is disabled by default for security reasons.
  Error log record number: <error-number>
```

**Para resolver el error**:

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. Utilice SSH para iniciar sesión en el entorno remoto.

   ```bash
   magento-cloud ssh
   ```

1. Abra el archivo `./app/var/report/<error number>`.

1. [Examine los registros](../test/log-locations.md) y determine el origen del problema.

1. Agregar, confirmar y enviar cambios de código.

   ```bash
   git add -A && git commit -m "Fixed deployment failure" && git push origin <branch-name>
   ```
