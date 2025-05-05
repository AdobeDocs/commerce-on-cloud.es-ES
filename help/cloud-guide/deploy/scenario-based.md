---
title: Implementación basada en escenarios
description: Obtenga información sobre cómo personalizar Adobe Commerce en implementaciones de infraestructura en la nube mediante archivos de configuración personalizados.
feature: Cloud, Configuration, Deploy, Build
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '808'
ht-degree: 0%

---

# Implementación basada en escenarios

Con `ece-tools` 2002.1.0 y versiones posteriores, puede utilizar la característica de implementación basada en escenarios para personalizar el comportamiento de implementación predeterminado.
Esta característica usa **escenarios** y **pasos** en la configuración:

- **Configuración de escenario**-Cada vínculo de implementación es un *escenario*, que es un archivo de configuración XML que describe la secuencia y los parámetros de configuración para completar las tareas de implementación. Usted configura los escenarios en la sección `hooks` del archivo `.magento.app.yaml`.

- **Configuración del paso**: cada escenario usa una secuencia de *pasos* que describen mediante programación las operaciones necesarias para completar las tareas de implementación. Los pasos se configuran en un archivo de configuración de escenario basado en XML.

Adobe Commerce en la infraestructura en la nube proporciona un conjunto de [escenarios predeterminados](https://github.com/magento/ece-tools/tree/2002.1/scenario) y [pasos predeterminados](https://github.com/magento/ece-tools/tree/2002.1/src/Step) en el paquete `ece-tools`. Puede personalizar el comportamiento de la implementación creando archivos de configuración XML personalizados para anular o personalizar la configuración predeterminada. También puede utilizar escenarios y pasos para ejecutar código desde módulos personalizados.

## Adición de escenarios mediante los vínculos de generación e implementación

Agregue los escenarios para crear e implementar Adobe Commerce en la sección `hooks` del archivo `.magento.app.yaml`. Cada vínculo especifica los escenarios que se ejecutarán durante cada fase. El siguiente ejemplo muestra la configuración de escenario predeterminada.

> `magento.app.yaml` enlaces

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

>[!NOTE]
>
>Con la versión de `ece-tools` 2002.1.x, hay un nuevo formato de [configuración de enlaces](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/configure/app/properties/hooks-property.html?lang=es). El formato heredado de `ece-tools` versiones 2002.0.x sigue siendo compatible. Sin embargo, debe actualizar al nuevo formato para utilizar la función de implementación basada en escenarios.

## Revisar pasos del escenario

En la configuración del vínculo, cada escenario es un archivo XML que contiene los pasos para ejecutar las tareas de generación, implementación o posteriores a la implementación. Por ejemplo, el archivo `scenario/transfer` incluye tres pasos: `compress-static-content`, `clear-init-directory` y `backup-data`

> `scenario/transfer.xml`

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
    <step name="compress-static-content" type="Magento\MagentoCloud\Step\Build\CompressStaticContent" priority="100"/>
    <step name="clear-init-directory" type="Magento\MagentoCloud\Step\Build\ClearInitDirectory" priority="200"/>
    <step name="backup-data" type="Magento\MagentoCloud\Step\Build\BackupData" priority="300">
        <arguments>
            <argument name="logger" xsi:type="object">Psr\Log\LoggerInterface</argument>
            <argument name="steps" xsi:type="array">
                <item name="static-content" xsi:type="object" priority="100">Magento\MagentoCloud\Step\Build\BackupData\StaticContent</item>
                <item name="writable-dirs" xsi:type="object"  priority="200">Magento\MagentoCloud\Step\Build\BackupData\WritableDirectories</item>
            </argument>
        </arguments>
    </step>
</scenario>
```

## Ampliación de un escenario predeterminado

El siguiente ejemplo amplía el escenario de implementación predeterminado añadiendo archivos de configuración de implementación adicionales a la configuración de enlace.

```yaml
hooks:
  deploy: |
    php ./vendor/bin/ece-tools run scenario/deploy.xml vendor/<vendor-name>/<module-name>/deploy.xml vendor/<vendor-name>/<module-name>/deploy2.xml
```

Durante la implementación, los escenarios personalizados se combinan con el escenario predeterminado en función de las siguientes reglas:

- Los escenarios se priorizan en función de su secuencia en la definición del vínculo, y el último escenario enumerado tiene la prioridad más alta.

  En el ejemplo, los escenarios tienen la siguiente prioridad:

   1. `vendor/vendor-name/module-name/deploy2.xml`
   1. `vendor/vendor-name/module-name/deploy.xml`
   1. `scenario/deploy.xml` (escenario predeterminado o de línea de base)

- Los pasos del escenario de mayor prioridad anulan los pasos que tienen el mismo nombre en los demás escenarios. Se añaden nuevos pasos a la configuración. Las mismas reglas se aplican a más de dos escenarios, y cada escenario se prioriza de derecha a izquierda, por ejemplo (C → B → A).

### Eliminar pasos predeterminados

Se eliminan pasos de los escenarios predeterminados mediante el parámetro `skip`.

Por ejemplo, para omitir los pasos `enable-maintenance-mode` y `set-production-mode` en el escenario de implementación predeterminado, cree un archivo de configuración que incluya la siguiente configuración.

> `vendor/vendor-name/module-name/deploy-custom-mode-config.xml`

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
    <step name="enable-maintenance-mode" skip="true" />
    <step name="set-production-mode"  skip="true" />
</scenario>
```

Para usar el archivo de configuración personalizada, actualice el archivo predeterminado `.magento.app.yaml`.

> `.magento.app.yaml` con escenario de implementación personalizado

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml vendor/vendor-name/module-name/deploy-custom-mode-config.xml
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

### Reemplazar pasos predeterminados

Los escenarios personalizados pueden reemplazar los pasos predeterminados para proporcionar una implementación personalizada. Para ello, utilice el nombre de paso predeterminado como nombre para el paso personalizado.

Por ejemplo, en el [escenario de implementación predeterminado], el paso `enable-maintenance-mode` ejecuta el [script PHP EnableMaintenanceMode predeterminado].

```xml
<step name="enable-maintenance-mode" type="Magento\MagentoCloud\Step\EnableMaintenanceMode" priority="300"/>
```

Para anular este paso, cree un archivo de configuración de escenario personalizado para ejecutar un script diferente cuando se ejecute el paso `enable-maintenance-mode`.

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
<scenario>
    <step name="enable-maintenance-mode" type="VendorName\VendorModule\Step\CustomEnableMaintenanceMode" priority="300"/>
</scenario>
```

### Cambio de la prioridad de paso

Los escenarios personalizados pueden cambiar la prioridad de los pasos predeterminados. El paso siguiente cambia la prioridad del paso `enable-maintenance-mode` de `300` a `10` para que el paso se ejecute antes en el escenario de implementación.

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
<scenario>
    <step name="enable-maintenance-mode" type="Magento\MagentoCloud\Step\EnableMaintenanceMode" priority="10"/>
</scenario>
```

En este ejemplo, el paso `enable-maintenance-mode` se mueve al principio del escenario porque tiene una prioridad inferior a todos los demás pasos del escenario de implementación predeterminado.

### Ejemplo: Ampliación del escenario de implementación

El ejemplo siguiente personaliza el [escenario de implementación predeterminado] con los cambios siguientes:

- Reemplaza el paso `remove-deploy-failed-flag` por un paso personalizado
- Omite el subpaso `clean-redis-cache` del paso previo a la implementación
- Omite el paso `unlock-cron-jobs`
- Omite el paso `validate-config` para deshabilitar los validadores esenciales
- Agrega un nuevo paso previo a la implementación

> `vendor/vendor-name/module-name/deploy-extended.xml`

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
    <!-- Replace "remove-deploy-failed-flag" step with custom step -->
    <step name="remove-deploy-failed-flag" type="Vendor\ModuleName\Step\Deploy\RemoveDeployFailedFlag" priority="100"/>

    <!-- Skip "clean-redis-cache" sub-step in pre-deploy step -->
    <step name="pre-deploy" type="Magento\MagentoCloud\Step\Deploy\PreDeploy" priority="200">
        <arguments>
            <argument name="logger" xsi:type="object">Psr\Log\LoggerInterface</argument>
            <argument name="steps" xsi:type="array">
                <item name="clean-redis-cache" xsi:type="object" skip="true"/>
            </argument>
        </arguments>
    </step>

    <!-- Skip step "unlock-cron-jobs" -->
    <step name="unlock-cron-jobs" skip="true"/>

    <!-- Skip critical validators -->
    <step name="validate-config" type="Magento\MagentoCloud\Step\ValidateConfiguration" priority="300">
        <arguments>
            <argument name="logger" xsi:type="object">Psr\Log\LoggerInterface</argument>
            <argument name="validators" xsi:type="array">
                <item name="critical" xsi:type="array">
                    <item name="database-configuration" xsi:type="object" skip="true"/>
                    <item name="search-configuration" xsi:type="object" skip="true"/>
                </item>
            </argument>
        </arguments>
    </step>

    <!-- Add new step into the beginning of the deploy scenario -->
    <step name="new-pre-deploy-step" type="Vendor\ModuleName\Step\Deploy\PreDeploy" priority="10"/>
</scenario>
```

Para usar este script en su proyecto, agregue la siguiente configuración al archivo `.magento.app.yaml` para su proyecto de Adobe Commerce en la nube:

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml vendor/vendor-name/module-name/deploy-extended.xml
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

>[!TIP]
>
>Puede revisar los [escenarios predeterminados](https://github.com/magento/ece-tools/tree/2002.1/scenario) y la [configuración predeterminada de los pasos](https://github.com/magento/ece-tools/tree/2002.1/src/Step) en el repositorio de GitHub `ece-tools` para determinar qué escenarios y pasos personalizar para las tareas de generación, implementación y posteriores a la implementación del proyecto.

## Agregar un módulo personalizado para ampliar `ece-tools`

El paquete `ece-tools` proporciona interfaces de API predeterminadas que siguen los estándares de la versión semántica. Todas las interfaces API están marcadas con la anotación **@api**. Puede reemplazar la implementación de API predeterminada con la suya propia creando un módulo personalizado y modificando el código predeterminado según sea necesario.

Para utilizar el módulo personalizado con Adobe Commerce en la infraestructura de la nube, debe registrar el módulo en la lista de extensiones del paquete `ece-tools`. El proceso de registro es similar al proceso que se utiliza para registrar módulos en Adobe Commerce.

**Para registrar un módulo con el paquete `ece-tools`**:

1. Cree o extienda el archivo `registration.php` en la raíz del módulo.

   ```php?start_inline=1
   \Magento\MagentoCloud\ExtensionRegistrar::register('module-name', __DIR__);
   ```

1. Actualice la sección `autoload` del archivo de configuración del módulo para incluir el archivo `registration.php` y cargar automáticamente los archivos del módulo en `composer.json`.

   ```json
   {
     "name": "vendor/ece-tools-extend",
     "description": "Extension for ece-tools",
     "type": "magento2-component",
     "version": "1.0.0",
     "license": "OSL-3.0",
     "autoload": {
       "files": [ "registration.php" ],
       "psr-4": {
         "Vendor\\EceToolExtend\\": "src/"
       }
     }
   }
   ```

1. Agregue el archivo `config/services.xml` a su módulo. Esta configuración se combinó sobre `config/services.xml` desde el paquete `ece-tools`.

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <container xmlns="http://symfony.com/schema/dic/services"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://symfony.com/schema/dic/services https://symfony.com/schema/dic/services/services-1.0.xsd">
       <services>
           <defaults autowire="true" autoconfigure="true" public="true"/>
   
           <prototype namespace="Vendor\EceToolExtend\" resource="../src/*" exclude="../src/{Test}"/>
   
           <!-- Use your own implementation of EnvironmentDataInterface -->
           <service id="Magento\MagentoCloud\Config\EnvironmentDataInterface" alias="Vendor\EceToolExtend\Config\CustomEnvironmentData" />
       </services>
   </container>
   ```

Para obtener más información acerca de la inyección de dependencia, consulte [Inyección de dependencia de Symfony](https://symfony.com/doc/current/components/dependency_injection.html).

<!-- link definitions -->

[escenario de implementación predeterminado]: https://github.com/magento/ece-tools/blob/develop/scenario/deploy.xml
[EnableMaintenanceMode: script PHP]: https://github.com/magento/ece-tools/blob/develop/src/Step/EnableMaintenanceMode.php
