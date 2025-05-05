---
title: Propiedad Crons
description: Vea ejemplos sobre cómo configurar la propiedad crons en el archivo de configuración de la aplicación  [!DNL Commerce] .
feature: Cloud, Configuration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1069'
ht-degree: 0%

---

# Propiedad Crons

Adobe Commerce usa la propiedad `crons` para programar actividades repetitivas. Es ideal para programar una tarea específica para que se ejecute a determinadas horas del día. Solo se puede ejecutar un trabajo cron a la vez en la instancia web para Adobe Commerce en proyectos de infraestructura en la nube debido a la naturaleza de los entornos de solo lectura. Se recomienda desglosar las tareas de larga duración en tareas más pequeñas en cola. También puede crear una [instancia de trabajador](workers-property.md).

El Adobe recomienda ejecutar `crons` como [propietario del sistema de archivos](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html?lang=es). _no_ ejecuta `crons` como `root` o como usuario del servidor web.

Esta configuración es diferente de las implementaciones locales de Adobe Commerce, que tienen varios trabajos cron predeterminados. Consulte [Configurar trabajos cron](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/configure-cron-jobs.html?lang=es) en la _guía de configuración_.

## Configuración de trabajos cron

La propiedad `crons` describe los procesos que se desencadenan según una programación. Cada trabajo requiere un nombre y las siguientes opciones:

- `spec`: la expresión cron utilizada para la programación.
- `cmd`: el comando que se ejecutará en `start` y `stop`.
- `shutdown_timeout`—(_Opcional_) Si se cancela un trabajo cron, este es el número de segundos después de los cuales se envía una señal SIGKILL para detener el trabajo o proceso. El valor predeterminado es 10 segundos.
- `timeout`—(_Opcional_) La cantidad máxima de tiempo que un trabajo cron puede ejecutarse antes del tiempo de espera. El valor predeterminado es el máximo permitido de 86400 segundos (24 horas).

De manera predeterminada, cada proyecto de nube de Commerce tiene la siguiente configuración predeterminada `crons` en el archivo `.magento.app.yaml`:

```yaml
crons:
    cronrun:
        spec: "* * * * *"
        cmd: "php bin/magento cron:run"
```

Si el proyecto requiere trabajos cron personalizados, puede agregarlos a la configuración predeterminada de `crons`. Ver [Crear un trabajo cron](#build-a-cron-job).

### `crontab`

Adobe Commerce agregó una opción de configuración de autocrons solo a proyectos Pro para admitir la configuración de autoservicio `crons` en los entornos de ensayo y producción. Si esta opción está habilitada, puede usar `crontab` para revisar la configuración de cron. _no_ está disponible con proyectos iniciales.

Aunque puede usar `crontab` para revisar la configuración en proyectos Pro, Adobe Commerce no usa `crontab` para ejecutar trabajos cron en sitios implementados en la infraestructura en la nube.

**Para revisar la configuración de cron en entornos Pro**:

1. Use [SSH](../development/secure-connections.md#use-an-ssh-command) para iniciar sesión en el entorno remoto.

1. Enumerar los procesos cron programados.

   ```shell
   crontab -l
   ```

   >[!NOTE]
   >
   >Si el comando `crontab -l` devuelve un error `Command not found` (solo en los entornos de ensayo y producción de Pro), debe [enviar un vale de soporte de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=es#submit-ticket) para habilitar la opción de configuración de autoservicio auto-crons en su proyecto.

El siguiente ejemplo muestra la salida `crontab` para un entorno que sólo tiene la configuración predeterminada `crons`:

```
username@hostname:~$ crontab -l
# Crontab is managed by the system, attempts to edit it directly will fail.
SHELL=/etc/platform/6fck2obu3244c/cron-run
MAILTO=""

# m h  dom mon dow  job_name

* * * * *           cronrun
```

## Creación de un trabajo cron

Un trabajo cron incluye la especificación de programación y tiempo y el comando para ejecutarse a la hora programada. Para entornos Starter y entornos Pro `integration`, el intervalo mínimo es de una vez cada cinco minutos. Para los entornos de ensayo y producción profesionales, el intervalo mínimo es de una vez por minuto. En Adobe Commerce en la infraestructura de la nube, se agregan trabajos cron personalizados al archivo `.magento.app.yaml` en la sección `crons`. El formato general es `spec` para la programación y `cmd` para especificar el comando o script personalizado que se va a ejecutar.

### Especificación

Adobe Commerce usa una expresión de cinco valores para una especificación `crons` (especificación): `* * * * *`

1. Minuto (0 a 59) Para todos los entornos Starter y Pro, la frecuencia mínima admitida para los trabajos cron es de cinco minutos. Es posible que tenga que configurar los ajustes de su administrador.
2. Hora (de 0 a 23)
3. Día del mes (del 1 al 31)
4. Mes (del 1 al 12)
5. Día de la semana (0 a 6) (de domingo a sábado; 7 también es domingo en algunos sistemas)

Algunos ejemplos:

- `00 */3 * * *` se ejecuta cada tres horas en el primer minuto (12:00 am, 3:00 am, 6:00 am)
- `20 */8 * * *` se ejecuta cada 8 horas en el minuto 20 (12:20 am, 8:20 am, 4:20 pm)
- `00 00 * * *` se ejecuta una vez al día a medianoche
- `00 * * * 1` se ejecuta una vez a la semana los lunes a medianoche.

>[!NOTE]
>
>La hora `crons` especificada en el archivo `.magento.app.yaml` se basa en la zona horaria del servidor, no en la especificada en los valores de configuración del almacén de la base de datos.

Al determinar la programación, tenga en cuenta el tiempo que se tarda en completar la tarea. Por ejemplo, si ejecuta un trabajo cada tres horas y la tarea tarda 40 minutos en completarse, puede considerar la posibilidad de cambiar el horario programado.

### Comando

`cmd` especifica el comando o script personalizado que se va a ejecutar. El formato del script de comandos puede incluir lo siguiente:

```text
<path-to-php-binary> <project-dir>/<script-command>
```

Por ejemplo:

```yaml
crons:
    spec: "00 */8 * * *"
    cmd: "/usr/bin/php /app/abc123edf890/bin/magento export:start catalog_category_product"
```

En este ejemplo, `<path-to-php-binary>` es `/usr/bin/php`. El directorio de instalación, que incluye el id. de proyecto es `/app/abc123edf890/bin/magento`, y la acción del script es `export:start catalog_category_product`.

### Agregar trabajos cron personalizados al proyecto

En Adobe Commerce en la plataforma de infraestructura en la nube, puede agregar personalizaciones a la sección `crons` del archivo [`.magento.app.yaml`](../application/configure-app-yaml.md).

>[!NOTE]
>
>Para entornos Starter y entornos Pro `integration`, el intervalo mínimo es de una vez cada cinco minutos. Para los entornos de ensayo y producción profesionales, el intervalo mínimo es de una vez por minuto. No puede configurar intervalos más frecuentes que los mínimos predeterminados.

En proyectos de Adobe Commerce Pro, la característica [auto-crons](#set-up-cron-jobs) debe estar habilitada en su proyecto para poder agregar trabajos cron personalizados a entornos de Ensayo y Producción usando el archivo `.magento.app.yaml`. Si esta característica no está habilitada, [envíe un vale de soporte técnico de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=es#submit-ticket) para habilitar los crons automáticos.

**Para agregar trabajos cron personalizados**:

1. En su entorno de desarrollo local, edite el archivo `.magento.app.yaml` en el directorio de Adobe Commerce `/app`.

1. En la sección `crons`, agregue la personalización con el siguiente formato:

   ```yaml
   crons:
       <cron_name_1>:
           spec: "<schedule_time>"
           cmd: "<schedule_command>"
       <cron_name_2>:
           spec: "<schedule_time>"
           cmd: "<schedule_command>"
   ```

   En el ejemplo siguiente, el trabajo de `productcatalog` exporta el catálogo de productos cada ocho horas, 20 minutos después de la hora.

   ```yaml
   crons:
       magento:
           spec: '* * * * *'
           cmd: 'php bin/magento cron:run'
       productcatalog:
           spec: '20 */8 * * *'
           cmd: 'bin/magento export:start catalog_product_category'
   ```

1. Agregar, confirmar y enviar cambios de código.

   ```bash
   git add .magento.app.yaml && git commit -m "cron config updates" && git push origin <branch-name>
   ```

### Actualizar trabajos cron

Para agregar, quitar o actualizar un trabajo personalizado, cambie la configuración en la sección `crons` del archivo `.magento.app.yaml`. A continuación, pruebe las actualizaciones en el entorno remoto `integration` antes de insertar los cambios en los entornos de ensayo y producción.

## Deshabilitar trabajos cron

Puede deshabilitar manualmente los trabajos cron antes de completar las tareas de mantenimiento, como la reindexación o la limpieza de la caché, para evitar problemas de rendimiento. Puede usar el comando `ece-tools` de CLI `cron:disable` para deshabilitar todos los trabajos de cron y detener cualquier proceso de cron activo.

**Para deshabilitar los trabajos cron**:

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. Utilice SSH para iniciar sesión en el entorno remoto.

   ```bash
   magento-cloud ssh
   ```

1. Deshabilite los trabajos cron y detenga los procesos cron activos.

   ```shell
   ./vendor/bin/ece-tools cron:disable
   ```

1. Después de completar cualquier tarea de mantenimiento necesaria, asegúrese de volver a habilitar los trabajos cron.

   ```shell
   ./vendor/bin/ece-tools cron:enable
   ```

## Solución de problemas de trabajos cron

Adobe ha actualizado el paquete de Adobe Commerce sobre la infraestructura en la nube para optimizar el procesamiento cron en Adobe Commerce sobre la plataforma de infraestructura en la nube y solucionar los problemas relacionados con cron. Si encuentra problemas con el procesamiento cron, asegúrese de que el proyecto está utilizando la versión más actual del paquete `ece-tools`. Consulte [Actualizar ECE-Tools](../dev-tools/update-package.md).

Puede revisar la información de procesamiento de cron en los archivos de registro de nivel de aplicación para cada entorno. Ver [registros de aplicaciones](../test/log-locations.md#application-logs).

Consulte los siguientes artículos de soporte de Adobe Commerce para obtener ayuda con la resolución de problemas relacionados con cron:

- [Las tareas cron bloquean tareas de otros grupos](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/cron-tasks-lock-tasks-from-other-groups.html?lang=es)

- [Restablecer los trabajos cron bloqueados manualmente en la nube](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/reset-stuck-magento-cron-jobs-manually-on-cloud.html?lang=es)
