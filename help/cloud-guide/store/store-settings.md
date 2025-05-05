---
title: Administración de configuración de tienda
description: Obtenga información sobre cómo administrar y sincronizar los ajustes de configuración de la tienda en todos los entornos de Adobe Commerce en la nube.
feature: Cloud, Configuration, SCD
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1439'
ht-degree: 0%

---

# Administración de configuración de tienda

Las configuraciones predeterminadas para su tienda se almacenan en un `config.xml` para el módulo correspondiente. Cuando cambia la configuración en el administrador de Commerce o en el comando `bin/magento config:set` de CLI, los cambios se reflejan en la base de datos principal, específicamente en la tabla `core_config_data`. Esta configuración sobrescribe las configuraciones predeterminadas almacenadas en el archivo `config.xml`.

La configuración del almacén, que hace referencia a las configuraciones de la sección Administración **Almacenes** > **Configuración** > **Configuración**, se almacena en los archivos de configuración de implementación en función del tipo de configuración:

- `app/etc/config.php`: ajustes de configuración para tiendas, sitios web, módulos o extensiones, optimización de archivos estáticos y valores del sistema relacionados con la implementación de contenido estático. Consulte la [referencia de config.php](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-configphp.html?lang=es) en la _Guía de configuración_.
- `app/etc/env.php`: valores para invalidaciones específicas del sistema y configuración confidencial que deberían _NO_ almacenarse en el control de código fuente. Consulte la [referencia env.php](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-envphp.html?lang=es) en la _Guía de configuración_.

>[!NOTE]
>
>Como Adobe Commerce en la infraestructura en la nube solo admite los modos de producción y mantenimiento, no se puede acceder a la sección **Avanzado** > **Desarrollador** desde el administrador. Debe tener [privilegios de administrador del entorno](../project/user-access.md) para completar las tareas de administración de la configuración. Puede configurar opciones adicionales mediante [variables de entorno](../environment/configure-env-yaml.md).

La administración de la configuración proporciona una forma de implementar configuraciones de tienda coherentes en todos los entornos con un tiempo de inactividad mínimo mediante la implementación de canalización. El proyecto de infraestructura de Adobe Commerce en la nube incluye el servidor de compilación, los scripts de compilación e implementación y los entornos de implementación diseñados teniendo en cuenta la [estrategia de implementación de canalización](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/deployment/technical-details.html?lang=es).

## Esquema de anulación de configuración

Todas las configuraciones del sistema se establecen durante las fases de generación e implementación según el siguiente esquema de invalidación:

1. Si existe una variable de entorno, utilice la configuración personalizada e ignore la configuración predeterminada.
1. Si no existe una variable de entorno, use la configuración de un par nombre-valor `MAGENTO_CLOUD_RELATIONSHIPS` en el archivo [`.magento.app.yaml`](../application/configure-app-yaml.md). Omitir la configuración predeterminada.
1. Si no existe una variable de entorno y `MAGENTO_CLOUD_RELATIONSHIPS` no contiene un par nombre-valor, quite toda la configuración personalizada y use los valores de la configuración predeterminada.

En resumen, las variables de entorno anulan todos los demás valores.

>[!TIP]
>
>Consulte [Administración de configuración](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/deployment/technical-details.html?lang=es) en la _Guía de configuración_ para obtener más información sobre el esquema de invalidación para la implementación de canalización.

Si la misma configuración se configura en varios lugares, la aplicación depende de la siguiente jerarquía de configuración para determinar qué valor se aplicará al entorno

| Prioridad | Método de configuración <br> | Descripción |
| -------- | ------------------------ | ----------- |
| 1 | [!DNL Cloud Console]<br>variables de entorno | Valores agregados desde la ficha _Variables_ de la configuración del entorno en [!DNL Cloud Console]. Especifique aquí los valores para las configuraciones sensibles o específicas del entorno. La configuración especificada aquí no se puede editar desde el administrador. Consulte [Variables de configuración de entorno](../project/overview.md#configure-environment). |
| 2 | `.magento.app.yaml` | Valores agregados en la sección `variables` del archivo `.magento.app.yaml`. Especifique valores aquí para garantizar una configuración coherente en todos los entornos. **No especifique valores confidenciales en el archivo `.magento.app.yaml`.** Ver [configuración de la aplicación](../application/configure-app-yaml.md). |
| 3 | `app/etc/env.php` | Los valores de configuración específicos del entorno almacenados aquí se agregan mediante el comando `app:config:dump`. Establezca los valores confidenciales y específicos del sistema utilizando variables de entorno o la CLI. Ver [datos confidenciales](#sensitive-data). El archivo `env.php` es **no** incluido en el control de código fuente. |
| 4 | `app/etc/config.php` | Los valores almacenados aquí se agregan mediante el comando `app:config:dump`. Los valores de configuración compartidos se agregaron a `config.php`. Establezca la configuración compartida desde el administrador o utilizando la CLI. El archivo `config.php` se incluye en el control de código fuente. |
| 5 | Base de datos | Los valores almacenados aquí se agregan estableciendo configuraciones en el Administrador. Las configuraciones configuradas con cualquiera de los métodos anteriores están bloqueadas (atenuadas) y no se pueden editar desde el administrador. |
| 6 | `config.xml` | Muchas configuraciones tienen valores predeterminados establecidos en el archivo `config.xml` de un módulo. Si Adobe Commerce no encuentra ningún valor establecido por ninguno de los métodos anteriores, vuelve al valor predeterminado, si está establecido. |

{style="table-layout:auto"}

## Volcado de configuración

Puede usar el siguiente comando `ece-tools` para generar un archivo `config.php` que contenga todas las configuraciones de almacén actuales:

```bash
./vendor/bin/ece-tools config:dump
```

Los datos &quot;descargados&quot; en el archivo `app/etc/config.php` se convierten en _bloqueados_, lo que significa que el campo correspondiente en el administrador de Commerce se convierte en **de solo lectura**. El archivo `config.php` incluye únicamente la configuración que usted haya configurado. No bloquea los valores predeterminados. Bloquear solo los valores que actualice también garantiza que todas las extensiones utilizadas en los entornos de ensayo y producción no se rompan debido a configuraciones de solo lectura, especialmente Fastly.

>[!WARNING]
>
>El comando `ece-tools config:dump` no recupera configuraciones detalladas para módulos, como B2B. Si necesita un volcado de configuración completo, utilice el comando `app:config:dump`, pero este comando bloquea los valores de configuración en un estado de solo lectura.

### Datos confidenciales

Cualquier configuración confidencial se exporta al archivo `app/etc/env.php` cuando se usa el comando `bin/magento app:config:dump`. Puede establecer valores confidenciales mediante el comando CLI: `bin/magento config:sensitive:set`. Consulte [Configuración confidencial y específica del entorno](https://developer.adobe.com/commerce/php/development/configuration/sensitive-environment-settings/) en la guía de _Extensiones de Commerce PHP_ para obtener información sobre cómo designar las opciones de configuración como confidenciales o específicas del sistema.

Vea una lista de [configuraciones confidenciales o específicas del sistema](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/paths/config-reference-sens.html?lang=es) en la _Guía de configuración_.

### Rendimiento de SCD

Según el tamaño del almacén, puede tener un gran número de archivos de contenido estático para implementar. Normalmente, el contenido estático se implementa durante la fase de implementación cuando la aplicación está en modo de mantenimiento. La configuración más óptima es generar contenido estático durante la fase de compilación. Consulte [Elegir una estrategia de implementación](../deploy/static-content.md).

Si ha habilitado la administración de la configuración después de volcar las configuraciones, debe mover las variables SCD_* de la fase de implementación a la fase de compilación para habilitar correctamente la generación de contenido estático durante la fase de compilación. Consulte [Variables de entorno](../environment/configure-env-yaml.md#environment-variables).

**Antes de la administración de la configuración**:

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

**Después de habilitar la administración de configuración**:

Mueva las variables SCD_* a la fase de compilación:

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

>[!NOTE]
>
>Antes de implementar archivos estáticos, las fases de compilación e implementación comprimen el contenido estático mediante GZIP. La compresión de archivos estáticos reduce las cargas del servidor y aumenta el rendimiento del sitio. Consulte [opciones de compilación](../environment/variables-build.md) para obtener más información sobre cómo personalizar o deshabilitar la compresión de archivos.

## Procedimiento para administrar la configuración

A continuación se ofrece una descripción general de alto nivel de este proceso:

![Información general sobre la administración de la configuración de inicio](../../assets/starter/configuration-management-flow.png)

**Para configurar su tienda y generar un archivo de configuración**:

1. Complete todas las configuraciones de sus tiendas en Admin para uno de los entornos:

   - Starter: una rama de desarrollo activa
   - Pro: rama activa en el entorno de integración

   Estas configuraciones no incluyen los productos reales a menos que piense en descargar la base de datos de este entorno a los entornos de ensayo y producción. Normalmente, las bases de datos de desarrollo no incluyen todos los datos del almacén.

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. Cree un volcado local de la base de datos remota.

   ```bash
   magento-cloud db:dump
   ```

1. Agregue, confirme y envíe cambios en el código para actualizar un entorno remoto.

   ```bash
   git add app/etc/config.php
   ```

   ```bash
   git commit -m "Add system-specific configuration"
   ```

   ```bash
   git push origin <branch-name>
   ```

Una vez completada la implementación, inicie sesión en el administrador del entorno actualizado para comprobar la configuración. Continúe combinando las configuraciones adicionales en los entornos de ensayo y producción, según sea necesario.

### Actualizar configuraciones

Cuando modifique el entorno mediante el Administrador y ejecute de nuevo el comando, las nuevas configuraciones se anexarán al código del archivo `config.php`.

>[!WARNING]
>
>Aunque puede editar manualmente el archivo `config.php` en los entornos de ensayo y producción, **no se recomienda**. El archivo ayuda a mantener la coherencia de todas las configuraciones en todos los entornos. Nunca elimine el archivo `config.php` para volver a crearlo. Al eliminar el archivo, se pueden quitar configuraciones y valores específicos necesarios para los procesos de generación e implementación.

### Restaurar archivos de configuración

Se crearon copias de los archivos originales `app/etc/env.php` y `app/etc/config.php` durante el proceso de implementación y se almacenaron en la misma carpeta. A continuación se muestra el BAK (archivos de copia de seguridad) y PHP (archivos originales) en la misma carpeta `app/etc`:

```
...
config.php.bak
di.xml
env.php.bak
vendor_path.php
config.php
db_schema.xml
env.php
...
```

Configuraciones anteriores usaron el archivo `app/etc/config.local.php`. Ver [Migrar configuraciones anteriores](#migrate-older-configurations).

**Para restaurar los archivos de configuración**:

1. En la estación de trabajo local, utilice SSH para iniciar sesión en el proyecto y entorno remotos.

   ```bash
   magento-cloud ssh
   ```

1. Compruebe la ubicación y disponibilidad de los archivos de copia de seguridad.

   ```bash
   ./vendor/bin/ece-tools backup:list
   ```

   Respuesta de ejemplo:

   ```
   The list of backup files:
   app/etc/env.php
   app/etc/config.php
   ```

1. Restaurar archivos de copia de seguridad.

   ```bash
   ./vendor/bin/ece-tools backup:restore
   ```

### Migración de configuraciones antiguas

Si actualiza a Adobe Commerce en la infraestructura en la nube 2.2 o posterior, puede que desee migrar la configuración del archivo `config.local.php` al nuevo archivo `config.php`. Si las opciones de configuración del administrador coinciden con el contenido del archivo, siga las instrucciones para generar y agregar el archivo `config.php`.

Si difieren, puede anexar contenido del archivo `config.local.php` al nuevo archivo `config.php`:

1. Siga las instrucciones para generar el archivo `config.php`.

1. Abra el archivo `config.php` y elimine la última línea.

1. Abra el archivo `config.local.php` y copie el contenido.

1. Pegue el contenido en el archivo `config.php`, guárdelo y complete su adición a Git.

1. Implemente en todos sus entornos.

Solo puede completar esta migración una vez. Después de la migración, use el archivo `config.php`.

### Cambiar configuraciones regionales

Puede cambiar las configuraciones regionales de su tienda sin seguir un complejo proceso de importación y exportación de la configuración, _si_ tiene [SCD_ON_DEMAND](../environment/variables-global.md#scd_on_demand) habilitado. Puede actualizar las configuraciones regionales mediante el Administrador.

Puede agregar otra configuración regional al entorno de ensayo o producción habilitando `SCD_ON_DEMAND` en una rama de integración, generar un archivo `config.php` actualizado con la nueva información de configuración regional y copiar el archivo de configuración en el entorno de destino.

>[!WARNING]
>
>Este proceso **sobrescribe** la configuración del almacén; realice las siguientes acciones únicamente si los entornos contienen los mismos almacenes.

1. En el entorno de integración, habilite la variable `SCD_ON_DEMAND` mediante el archivo [`.magento.env.yaml`](../environment/configure-env-yaml.md).

1. Añada las configuraciones regionales necesarias con su administrador.

1. Use SSH para iniciar sesión en el entorno remoto y generar el archivo `/app/etc/config.php` que contiene todas las configuraciones regionales.

   ```bash
   ssh <SSH-URL> "./vendor/bin/ece-tools config:dump"
   ```

1. Copie el nuevo archivo de configuración del entorno de integración remota al directorio de entorno local.

   ```bash
   rsync <SSH-URL>:app/etc/config.php ./app/etc/config.php
   ```

1. Agregue, confirme y envíe cambios en el código para actualizar un entorno remoto.
