---
title: Proceso de implementación
description: Descubra cómo funciona la implementación de Adobe Commerce en proyectos de infraestructura en la nube.
feature: Cloud, Build, Deploy, SCD
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '396'
ht-degree: 0%

---

# Proceso de implementación

El proceso de implementación comienza cuando combina, inserta o sincroniza su entorno, o cuando almacena en déclencheur una [reimplementación manual](../dev-tools/cloud-cli-overview.md#redeploy-the-environment). El proceso de implementación lleva tiempo, pero hay formas de optimizar la implementación que dependen de si está desarrollando y probando o trabajando con un sitio activo. Lo más importante es que puede controlar la [implementación de contenido estático](static-content.md).

Existen tres fases distintas del proceso de implementación: generación, implementación y posterior a la implementación. Cada fase realiza acciones específicas con recursos limitados:

## ![Fase de compilación](../../assets/status-build.png) fase de compilación

La fase _build_ organiza contenedores para los servicios definidos en los archivos de configuración, instala dependencias basadas en el archivo `composer.lock` y ejecuta los vínculos de compilación definidos en el archivo `.magento.app.yaml`. Sin la capacidad de conectarse a ningún servicio ni acceder a la base de datos, la fase de compilación depende de los recursos limitados al entorno.

## ![Fase de implementación](../../assets/status-deploy.png) Fase de implementación

La fase _deploy_ suspende temporalmente las solicitudes entrantes y pasa el sitio a [modo de mantenimiento](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/setup/application-modes.html). La fase de implementación utiliza los nuevos contenedores y, después de montar el sistema de archivos, abre las conexiones de red, activa los servicios definidos en la sección `relationships` del archivo `.magento.app.yaml` y ejecuta los vínculos de implementación definidos en el archivo `.magento.app.yaml`. Todo es _solo lectura_, excepto los directorios definidos en el archivo `.magento.app.yaml`. De manera predeterminada, la propiedad [`mounts` ](../application/properties.md#mounts) incluye los siguientes directorios:

- `app/etc`: contiene los archivos de configuración `env.php` y `config.php`
- `pub/media`: contiene todos los datos multimedia, como productos o categorías
- `pub/static`: contiene archivos estáticos generados
- `var`: contiene archivos temporales creados durante el tiempo de ejecución

Todos los demás directorios tienen permisos de solo lectura. El nuevo sitio se activa al final de la fase de implementación, a medida que pasa del modo de mantenimiento al modo de mantenimiento y libera la suspensión temporal de las solicitudes entrantes.

En la fase de implementación, las copias de los archivos de configuración de implementación `app/etc/config.php` y `app/etc/env.php` se guardan con la extensión BACK. Consulte [Configuración de almacenamiento](../store/store-settings.md#restore-configuration-files) para obtener más información sobre cómo restaurar estos archivos.

## ![Fase posterior a la implementación](../../assets/status-post-deploy.png) Fase posterior a la implementación

La fase _post-deploy_ ejecuta los vínculos post-deploy definidos en el archivo `.magento.app.yaml`. Realizar cualquier acción en esta fase puede afectar el rendimiento del sitio; sin embargo, puede usar la variable de entorno [WARM_UP_PAGES](../environment/variables-post-deploy.md#warmuppages) para rellenar la caché.

## ![Comprobar estado](../../assets/status-verify.png) Comprobar configuraciones

Puede probar la configuración óptima para el estado del proyecto ejecutando los [asistentes inteligentes](smart-wizards.md).

>[!NOTE]
>
>Con `ece-tools` 2002.1.0 y versiones posteriores, puede utilizar la característica de implementación basada en escenarios para personalizar los procesos de compilación, implementación y posteriores a la implementación para su proyecto de infraestructura de Adobe Commerce en la nube. Consulte [Implementación basada en escenarios](scenario-based.md).
