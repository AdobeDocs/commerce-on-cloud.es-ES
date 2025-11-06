---
title: Visualización y administración de registros
description: Comprenda los tipos de archivos de registro disponibles en la infraestructura de la nube y dónde encontrarlos.
last-substantial-update: 2023-05-23T00:00:00Z
exl-id: f0bb8830-8010-4764-ac23-d63d62dc0117
source-git-commit: afdc6f2b72d53199634faff7f30fd87ff3b31f3f
workflow-type: tm+mt
source-wordcount: '1205'
ht-degree: 0%

---

# Visualización y administración de registros

Los registros de Adobe Commerce en proyectos de infraestructura en la nube son útiles para solucionar problemas relacionados con [generar e implementar enlaces](../application/hooks-property.md), servicios en la nube y la aplicación de Adobe Commerce.

Puede ver los registros del sistema de archivos, [!DNL Cloud Console] y `magento-cloud` CLI.

- **Sistema de archivos**: el directorio del sistema `/var/log` contiene registros para todos los entornos. El directorio `var/log/` contiene registros específicos de la aplicación exclusivos de un entorno en particular. Estos directorios no se comparten entre los nodos de un clúster. En los entornos de ensayo y producción de Pro, debe comprobar los registros de cada nodo.

- **[!DNL Cloud Console]**: puede ver información de registro de generación, implementación y posterior a la implementación en la lista de _mensajes_ del entorno.

- **CLI de nube**: puede ver los registros del entorno local mediante el comando `magento-cloud log` o los registros del entorno remoto mediante el comando `magento-cloud ssh`.

## Ubicaciones de registro

Los registros del sistema se almacenan en las siguientes ubicaciones:

- Integración: `/var/log/<log-name>.log`
- Ensayo profesional: `/var/log/platform/<project-ID>_stg/<log-name>.log`
- Producción profesional: `/var/log/platform/<project-ID>/<log-name>.log`

El valor de `<project-ID>` depende del proyecto y de si el entorno es de ensayo o de producción. Por ejemplo, con un ID de proyecto de `yw1unoukjcawe`, el usuario del entorno de ensayo es `yw1unoukjcawe_stg` y el usuario del entorno de producción es `yw1unoukjcawe`.

En ese ejemplo, el registro de implementación es: `/var/log/platform/yw1unoukjcawe_stg/deploy.log`

### Ver registros de entorno remoto

La mayoría de los registros incluyen eventos que se producen en el entorno remoto. Para Pro, hay varios nodos y cada nodo tiene registros únicos. Utilice lo siguiente para ver una lista de todos los hosts:

```bash
magento-cloud ssh -p <project-ID> -e <environment-ID> --all
```

Respuesta de ejemplo:

```
1.ent-project-environment-id@ssh.region.magento.cloud
2.ent-project-environment-id@ssh.region.magento.cloud
3.ent-project-environment-id@ssh.region.magento.cloud
```

**Para ver una lista de registros de entorno remoto**:

```bash
magento-cloud ssh -e <environment-ID> "ls var/log"
```

Ejemplo de Pro:

```bash
ssh 1.ent-project-environment-id@ssh.region.magento.cloud "ls var/log | grep error"
```

**Para ver un registro remoto**:

```bash
magento-cloud ssh -e <environment-ID> "cat var/log/cron.log"
```

Ejemplo de Pro:

```bash
ssh 1.ent-project-environment-id@ssh.region.magento.cloud "cat var/log/cron.log"
```

>[!TIP]
>
>En los entornos Pro Staging y Pro Production, la rotación, compresión y eliminación automáticas de registros están habilitadas para los archivos de registro con un nombre de archivo fijo. Cada tipo de archivo de registro tiene un patrón giratorio y una duración.
>Se pueden encontrar todos los detalles sobre la rotación del registro del entorno y la duración de los registros comprimidos en: `/etc/logrotate.conf` y `/etc/logrotate.d/<various>`.
>Para los entornos Pro Staging y Pro Production, debe [enviar un vale de soporte de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=es#submit-ticket) para solicitar cambios en la configuración de rotación del registro.

>[!TIP]
>
>La rotación de registros no se puede configurar en entornos de Pro Integration.
>Para la integración Pro, debe implementar una solución/script personalizado y [configurar su cron](../application/crons-property.md) para ejecutar el script según sea necesario.

>[!NOTE]
>
>Los entornos del proyecto de inicio no tienen rotación de registro.

## Creación e implementación de registros

Después de insertar los cambios en su entorno, puede revisar el registro desde cada vínculo en el archivo `var/log/cloud.log`. El registro contiene mensajes de inicio y parada para cada vínculo. En el ejemplo siguiente, los mensajes son &quot;`Starting post-deploy.`&quot; y &quot;`Post-deploy is complete.`&quot;

Compruebe las marcas de tiempo en las entradas de registro, compruebe y busque los registros de una implementación específica. El siguiente es un ejemplo conciso de la salida de registro que puede utilizar para solucionar problemas:

```
Re-deploying environment project-integration-ID
  Executing post deploy hook for service `mymagento`
    [2019-01-03 19:44:11] NOTICE: Starting post-deploy.
    [2019-01-03 19:44:11] INFO: Validating configuration
    [2019-01-03 19:44:11] INFO: End of validation
    [2019-01-03 19:44:11] INFO: Enable cron
    [2019-01-03 19:44:11] INFO: Create backup of important files.
    [2019-01-03 19:44:11] INFO: Backup /app/app/etc/env.php.bak for /app/app/etc/env.php was created.
    [2019-01-03 19:44:11] INFO: Backup /app/app/etc/config.php.bak for /app/app/etc/config.php was created.
    [2019-01-03 19:44:11] INFO: php ./bin/magento cache:flush --ansi --no-interaction
    [2019-01-03 19:44:32] INFO: Warming up failed: http://integration-id-project.us.magentosite.cloud/
    [2019-01-03 19:44:32] NOTICE: Post-deploy is complete.
```

>[!TIP]
>
>Al configurar su entorno de nube, puede configurar [notificaciones de correo electrónico y Slack basadas en registros](../environment/set-up-notifications.md) para acciones de compilación e implementación.

Los siguientes registros tienen una ubicación común para todos los proyectos en la nube:

- **Registro de implementación**: `var/log/cloud.log`
- **Último registro de errores de implementación**: `var/log/cloud.error.log`
- **Registro de depuración**: `var/log/debug.log`
- **Registro de excepciones**: `var/log/exception.log`
- **Registro del sistema**: `var/log/system.log`
- **Registro de asistencia**: `var/log/support_report.log`
- **Informes**: `var/report/`

Aunque el archivo `cloud.log` contiene comentarios de cada fase del proceso de implementación, los registros creados por el vínculo de implementación son exclusivos de cada entorno. El registro de implementación específico del entorno se encuentra en los siguientes directorios:

- **Integración de Starter y Pro**: `/var/log/deploy.log`
- **Ensayo profesional**: `/var/log/platform/<project-ID>_stg/deploy.log`
- **Producción profesional**: `/var/log/platform/<project-ID>/deploy.log`

### Implementación del registro

El registro de cada implementación se concatena al archivo `deploy.log` específico. El siguiente ejemplo imprime el registro de implementación del entorno actual en el terminal:

```bash
magento-cloud log -e <environment-ID> deploy
```

Respuesta de ejemplo:

```
Reading log file projectID-branchname-ID--mymagento@ssh.zone.magento.cloud:/var/log/'deploy.log'

[2023-04-24 18:58:03.080678] Launching command 'b'php ./vendor/bin/ece-tools run scenario/deploy.xml\n''.

[2023-04-24T18:58:04.129888+00:00] INFO: Starting scenario(s): scenario/deploy.xml (magento/ece-tools version: 2002.1.14, magento/magento2-base version: 2.4.6)
[2023-04-24T18:58:04.364714+00:00] NOTICE: Starting pre-deploy.
...
```

{{scd-timing-warning}}

### Registro de errores

Los mensajes de error y advertencia generados durante el proceso de implementación se escriben en los archivos `var/log/cloud.log` y `var/log/cloud.error.log`. El archivo de registro de errores de Cloud solo contiene errores y advertencias de la implementación más reciente. Un archivo vacío indica una implementación correcta sin errores.

Puede ver el archivo de registro mediante [Cloud CLI SSH](#view-remote-environment-logs), o puede usar ECE-Tools para mostrar los errores con sugerencias:

```bash
magento-cloud ssh -e <environment-ID> "./vendor/bin/ece-tools error:show"
```

Respuesta de ejemplo:

```
errorCode: 1001
stage: build
step: validate-config
suggestion: Please run the following commands:
1. bin/magento module:enable --all
2. git add -f app/etc/config.php
3. git commit -m 'Adding config.php'
4. git push
title: File app/etc/config.php does not exist
type: warning
---------------

errorCode: 1006
stage: build
step: validate-config
suggestion: Your application does not have the "post_deploy" hook enabled.
  In order to minimize downtime, add the following to ".magento.app.yaml":
  hooks:
      post_deploy: |
          php ./vendor/bin/ece-tools run scenario/post-deploy.xml
title: The configured state is not ideal
type: warning
```

La mayoría de los mensajes de error contienen una descripción y una acción sugerida. Use la referencia de mensaje de error [para ECE-Tools](../dev-tools/error-reference.md) para buscar el código de error y obtener más instrucciones. Para obtener más información, use el [solucionador de problemas de implementación de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/deployment/magento-deployment-troubleshooter.html?lang=es).

## Registros de aplicaciones

De forma similar a los registros de implementación, los registros de aplicaciones son únicos para cada entorno:

| Archivo de registro | Integración de Starter y Pro | Descripción |
| ------------------- | --------------------------- | ------------------------------------------------- |
| **Implementar registro** | `/var/log/deploy.log` | Actividad desde el [vínculo de implementación](../application/hooks-property.md). |
| **Registro posterior a la implementación** | `/var/log/post_deploy.log` | Actividad desde el [vínculo posterior a la implementación](../application/hooks-property.md). |
| **Registro Cron** | `/var/log/cron.log` | Salida de trabajos cron. |
| **Registro de acceso de Nginx** | `/var/log/access.log` | Al inicio de Nginx, errores HTTP para los directorios que faltan y los tipos de archivo excluidos. |
| **Registro de errores Nginx** | `/var/log/error.log` | Mensajes de inicio útiles para depurar errores de configuración asociados a Nginx. |
| **Registro de acceso de PHP** | `/var/log/php.access.log` | Solicitudes al servicio PHP. |
| **Registro FPM de PHP** | `/var/log/app.log` | |

Para los entornos de ensayo y producción Pro, los registros de implementación, posterior a la implementación y Cron solo están disponibles en el primer nodo del clúster:

| Archivo de registro | Ensayo profesional | Producción profesional |
| ------------------- | --------------------------------------------------- | ----------------------------------------------- |
| **Implementar registro** | Solo el primer nodo: <br>`/var/log/platform/<project-ID>_stg*/deploy.log` | Solo el primer nodo: <br>`/var/log/platform/<project-ID>/deploy.log` |
| **Registro posterior a la implementación** | Solo el primer nodo: <br>`/var/log/platform/<project-ID>_stg*/post_deploy.log` | Solo el primer nodo: <br>`/var/log/platform/<project-ID>/post_deploy.log` |
| **Registro Cron** | Solo el primer nodo: <br>`/var/log/platform/<project-ID>_stg*/cron.log` | Solo el primer nodo: <br>`/var/log/platform/<project-ID>/cron.log` |
| **Registro de acceso de Nginx** | `/var/log/platform/<project-ID>_stg*/access.log` | `/var/log/platform/<project-ID>/access.log` |
| **Registro de errores Nginx** | `/var/log/platform/<project-ID>_stg*/error.log` | `/var/log/platform/<project-ID>/error.log` |
| **Registro de acceso de PHP** | `/var/log/platform/<project-ID>_stg*/php.access.log` | `/var/log/platform/<project-ID>/php.access.log` |
| **Registro FPM de PHP** | `/var/log/platform/<project-ID>_stg*/php5-fpm.log` | `/var/log/platform/<project-ID>/php5-fpm.log` |

### Archivos de registro archivados

Los registros de la aplicación se comprimen y archivan una vez al día y se conservan durante **30 días** de forma predeterminada (para los clústeres de ensayo y producción de Pro); la rotación del registro no está disponible en todos los entornos de integración/inicio. Los registros comprimidos reciben un nombre mediante un identificador único que corresponde al `Number of Days Ago + 1`. Por ejemplo, en entornos de producción Pro, se almacena un registro de acceso PHP de 21 días en el pasado y se le asigna el siguiente nombre:

```
/var/log/platform/<project-ID>/php.access.log.22.gz
```

Los archivos de registro archivados siempre se almacenan en el directorio en el que se encontraba el archivo original antes de la compresión.

Puede [enviar un ticket de asistencia](https://experienceleague.adobe.com/home?lang=es&support-tab=home#support) para solicitar cambios en el período de retención de registros o en la configuración de logrotate. Puede aumentar el período de retención hasta un máximo de 365 días, reducirlo para conservar la cuota de almacenamiento o agregar rutas de registro adicionales a la configuración de logrotate. Estos cambios están disponibles para los clústeres de ensayo y producción de Pro.

Por ejemplo, si crea una ruta de acceso personalizada para almacenar registros en el directorio `var/log/mymodule`, puede solicitar la rotación del registro para esta ruta de acceso. Sin embargo, la infraestructura actual requiere nombres de archivo coherentes para que Adobe configure correctamente la rotación del registro. Adobe recomienda mantener la coherencia de los nombres de registro para evitar problemas de configuración.

>[!NOTE]
>
>Los archivos de registro **Implementar** y **Posterior a la implementación** no se giran ni se archivan. Todo el historial de implementación se escribe dentro de esos archivos de registro.

## Registros de servicio

Dado que cada servicio se ejecuta en un contenedor independiente, los registros del servicio no están disponibles en el entorno de integración. Adobe Commerce en la infraestructura en la nube proporciona acceso al contenedor del servidor web solo en el entorno de integración. Las siguientes ubicaciones de registro de servicio son para los entornos de ensayo y producción de Pro:

- **Registro de redis**: `/var/log/platform/<project-ID>*/redis-server-<project-ID>*.log`
- **Registro de Elasticsearch**: `/var/log/elasticsearch/elasticsearch.log`
- **Registro de recolección de elementos no utilizados de Java**: `/var/log/elasticsearch/gc.log`
- **Registro de correo**: `/var/log/mail.log`
- **Registro de errores de MySQL**: `/var/log/mysql/mysql-error.log`
- **Registro lento de MySQL**: `/var/log/mysql/mysql-slow.log`
- **Registro de RabbitMQ**: `/var/log/rabbitmq/rabbit@host1.log`

Los registros de servicio se archivan y guardan durante diferentes períodos de tiempo, según el tipo de registro. Por ejemplo, los registros MySQL tienen la duración más corta: se eliminan después de siete días.

>[!TIP]
>
>Las ubicaciones de los archivos de registro en la arquitectura escalada dependen del tipo de nodo. Consulte [Ubicaciones de registro en el tema Arquitectura a escala](../architecture/scaled-architecture.md#log-locations).

## Datos de registro para Producción y ensayo profesionales

En entornos Pro Production y Staging, use la [administración de registros de New Relic](../monitor/log-management.md) integrada con su proyecto para administrar los datos de registro agregados de todos los registros asociados con su proyecto Adobe Commerce en la nube.

La aplicación New Relic Logs proporciona un panel de administración de registros centralizado para solucionar problemas y supervisar Adobe Commerce en entornos de ensayo y producción de infraestructuras en la nube. El tablero también proporciona acceso a los datos de registro para los servicios Fastly CDN, Image Optimization y Web application firewall (WAF). Consulte [Servicios de New Relic](../monitor/new-relic-service.md).
