---
title: Implementación de contenido estático
description: Obtenga información acerca de las estrategias para implementar contenido estático, como imágenes, scripts y CSS, en Adobe Commerce en proyectos de infraestructura en la nube.
feature: Cloud, Build, Deploy, SCD
exl-id: 8f30cae7-a3a0-4ce4-9c73-d52649ef4d7a
source-git-commit: 325b7584daa38ad788905a6124e6d037cf679332
workflow-type: tm+mt
source-wordcount: '836'
ht-degree: 0%

---

# Estrategias de implementación de contenido estático

La implementación de contenido estático (SCD) tiene un impacto significativo en el proceso de implementación de tiendas que depende de la cantidad de contenido que se genere (como imágenes, scripts, CSS, vídeos, temáticas, configuraciones regionales y páginas web) y de cuándo se genere el contenido. Por ejemplo, la estrategia predeterminada genera contenido estático durante la [fase de implementación](process.md#deploy-phase-deploy-phase) cuando el sitio se encuentra en modo de mantenimiento; sin embargo, esta estrategia de implementación tarda tiempo en escribir el contenido directamente en el directorio `pub/static` montado. Tiene varias opciones o estrategias para ayudarle a mejorar el tiempo de implementación según sus necesidades.

## Optimización del contenido de JavaScript y HTML

Puede utilizar el agrupamiento y la minificación para crear contenido optimizado de JavaScript y HTML durante la implementación de contenido estático.

### Minimizar contenido

Puede mejorar el tiempo de carga del SCD durante el proceso de implementación si omite copiar los archivos de vista estática en el directorio `var/view_preprocessed` y genera HTML _minificado_ cuando se solicita. Puede activarlo si establece la variable de entorno global [SKIP_HTML_MINIFICATION](../environment/variables-global.md#skiphtmlminification) en `true` en el archivo `.magento.env.yaml`.

>[!NOTE]
>
>A partir de la versión 2002.0.13 del paquete `ece-tools`, el valor predeterminado de la variable SKIP_HTML_MINIFICATION se establece en `true`.

Puede ahorrar **más** tiempo de implementación y espacio en disco si reduce el número de archivos de temas innecesarios. Por ejemplo, puede implementar el tema `magento/backend` en inglés y un tema personalizado en otros idiomas. Puede configurar estos parámetros del tema con la variable de entorno [SCD_MATRIX](../environment/variables-deploy.md#scdmatrix).

## Elección de una estrategia de implementación

Las estrategias de implementación difieren según si elige generar contenido estático durante la fase _build_, la fase _deploy_ o _on-demand_. Como se ve en el gráfico siguiente, la generación de contenido estático durante la fase de implementación es la opción menos óptima. Incluso con HTML minificado, cada archivo de contenido debe copiarse en el directorio `~/pub/static` montado, lo que puede llevar mucho tiempo. La generación de contenido estático bajo demanda parece la opción óptima. Sin embargo, si el archivo de contenido no existe en la caché que genera en el momento en que se solicita, lo que añade tiempo de carga a la experiencia del usuario. Por lo tanto, la generación de contenido estático durante la fase de compilación es la mejor opción.

![Comparación de carga de SCD](../../assets/scd-load-times.png)

### Configuración del SCD durante la compilación

La generación de contenido estático durante la fase de compilación con HTML minificado es la configuración óptima para [**implementaciones sin tiempo de inactividad**](reduce-downtime.md), también conocida como **estado ideal**. En lugar de copiar archivos en una unidad montada, crea un enlace simbólico desde el directorio `./init/pub/static`.

La generación de contenido estático requiere acceso a temáticas y configuraciones regionales. Adobe Commerce almacena las temáticas en el sistema de archivos, al que se puede acceder durante la fase de compilación; sin embargo, Adobe Commerce almacena las configuraciones regionales en la base de datos. La base de datos _no_ está disponible durante la fase de compilación. Para generar el contenido estático durante la fase de compilación, debe utilizar el comando `config:dump` en el paquete `ece-tools` para mover configuraciones regionales al sistema de archivos. Lee las configuraciones regionales y las guarda en el archivo `app/etc/config.php`.

>[!NOTE]
>Después de ejecutar el comando `config:dump` en el paquete `ece-tools`, las configuraciones que se descargan en el archivo `config.php` [están bloqueadas (atenuadas) en el panel de administración](https://experienceleague.adobe.com/es/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/locked-fields-in-magento-admin). la única manera de actualizar esas configuraciones en el Administrador es eliminarlas del archivo localmente y volver a implementar el proyecto.
>&#x200B;>Además, cada vez que agregue un nuevo almacén/grupo de almacén/sitio web a la instancia, recuerde ejecutar el comando `config:dump` para asegurarse de que la base de datos esté sincronizada. También puede elegir [qué configuraciones se deben volcar](https://experienceleague.adobe.com/es/docs/commerce-operations/configuration-guide/cli/configuration-management/export-configuration?lang=en) en el archivo `config.php`.
>&#x200B;>Si elimina la configuración de tienda/grupo de tienda/sitio web del archivo `config.php` porque los campos están atenuados, pero no realiza este paso, las nuevas entidades que no se descargaron se eliminarán de la base de datos en la siguiente implementación.

**Para configurar el proyecto de modo que genere un SCD en la compilación**:

1. En la estación de trabajo local, cambie al directorio del proyecto.
1. Utilice SSH para iniciar sesión en el entorno remoto.

   ```bash
   magento-cloud ssh
   ```

1. Mueva configuraciones regionales al sistema de archivos y, a continuación, actualice el archivo [`config.php`](../development/commerce-version.md#create-a-configphp-file).

1. El archivo de configuración `.magento.env.yaml` debe contener los siguientes valores:

   - [SKIP_HTML_MINIFICATION](../environment/variables-global.md#skip_html_minification) es `true`
   - [SKIP_SCD](../environment/variables-build.md#skip_scd) en la fase de compilación es `false`
   - [SCD_STRATEGY](../environment/variables-build.md#scd_strategy) es `compact`

1. Compruebe la configuración del [vínculo posterior a la implementación](../application/hooks-property.md) en el archivo `.magento.app.yaml`.

1. Comprueba tu configuración ejecutando el [Asistente inteligente para el estado ideal](smart-wizards.md).

   ```bash
   php ./vendor/bin/ece-tools wizard:ideal-state
   ```

### Configuración del SCD bajo demanda

La generación de SCD bajo demanda es óptima para un flujo de trabajo de desarrollo en el entorno de integración. Reduce el tiempo de implementación para que pueda revisar rápidamente las implementaciones y ejecutar las pruebas de integración. Habilite la variable de entorno [SCD_ON_DEMAND](../environment/variables-global.md#scdondemand) en la fase global del archivo `.magento.env.yaml`. La variable SCD_ON_DEMAND anula todas las demás configuraciones relacionadas con SCD y borra el contenido existente del directorio `~/pub/static`.

Al utilizar la estrategia bajo demanda de SCD, ayuda precargar la caché con páginas que espera solicitar, como la página de inicio. Agregue su lista de páginas esperadas en la variable de entorno [WARM_UP_PAGES](../environment/variables-post-deploy.md#warmuppages) en la fase posterior a la implementación del archivo `.magento.env.yaml`.

>[!WARNING]
>
>No utilice la estrategia bajo demanda de SCD en el entorno de producción.

### Omitiendo SCD

A veces puede optar por omitir la generación de contenido estático por completo. Puede establecer la variable de entorno [SKIP_SCD](../environment/variables-build.md#skipscd) en la fase global para que ignore otras configuraciones relacionadas con SCD. Esto no afecta al contenido existente en el directorio `~/pub/static`.
