---
title: Realizar copia de seguridad de la base de datos
description: Aprenda a utilizar ECE-tools para crear una copia de seguridad de la base de datos para un proyecto de infraestructura en la nube de Adobe Commerce.
feature: Cloud, Iaas, Storage
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '339'
ht-degree: 0%

---

# Realizar copia de seguridad de la base de datos

Puede crear una copia de la base de datos utilizando el comando `ece-tools db-dump` sin capturar todos los datos de entorno de los servicios y montajes. De forma predeterminada, este comando crea copias de seguridad en el directorio `/app/var/dump-main` para todas las conexiones de base de datos especificadas en la configuración del entorno. La operación de volcado de la base de datos cambia la aplicación al modo de mantenimiento, detiene los procesos de cola de los consumidores y deshabilita los trabajos cron antes de que comience el volcado.

Tenga en cuenta las siguientes directrices para el volcado de la base de datos:

- En los entornos de producción, Adobe recomienda completar las operaciones de volcado de la base de datos durante las horas de menor actividad para minimizar las interrupciones del servicio que se producen cuando el sitio está en modo de mantenimiento.
- Si se produce un error durante la operación de volcado, el comando elimina el archivo de volcado para conservar espacio en disco. Revise los registros para obtener detalles (`var/log/cloud.log`).
- Para entornos de Pro Production, este comando solo volca de _uno_ de los tres nodos de alta disponibilidad, por lo que es posible que no se copien los datos de producción escritos en un nodo diferente durante el volcado. El comando genera un archivo `var/dbdump.lock` para evitar que se ejecute en más de un nodo.
- Para una copia de seguridad de todos los servicios de entorno, Adobe recomienda crear una [copia de seguridad](snapshots.md).

Puede elegir realizar copias de seguridad de varias bases de datos adjuntando los nombres de las bases de datos al comando. El siguiente ejemplo realiza una copia de seguridad de dos bases de datos: `main` y `sales`:

```bash
php vendor/bin/ece-tools db-dump main sales
```

Utilice el comando `php vendor/bin/ece-tools db-dump --help` para obtener más opciones:

- `--dump-directory=<dir>`: elija un directorio de destino para el volcado de la base de datos
- `--remove-definers`: quita las instrucciones DEFINER del volcado de la base de datos

**Para crear un volcado de la base de datos en el entorno de ensayo o producción**:

1. [Use SSH para iniciar sesión o crear un túnel para conectarse al entorno remoto](../development/secure-connections.md) que contiene la base de datos que se va a copiar.

1. Enumere las relaciones de entorno y anote la información de inicio de sesión de la base de datos.

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   o

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"]))->database);'
   ```

1. Cree una copia de seguridad de la base de datos. Para elegir un directorio de destino para el volcado de la base de datos, utilice la opción `--dump-directory`.

   ```bash
   php vendor/bin/ece-tools db-dump -- main
   ```

   Respuesta de ejemplo:

   ```
   The db-dump operation switches the site to maintenance mode, stops all active cron jobs and consumer queue processes, and disables cron jobs before starting the dump process.
   Your site will not receive any traffic until the operation completes.
   Do you wish to proceed with this process? (y/N)? y
   2020-01-28 16:38:08] INFO: Starting backup.
   [2020-01-28 16:38:08] NOTICE: Enabling Maintenance mode
   [2020-01-28 16:38:10] INFO: Trying to kill running cron jobs and consumers processes
   [2020-01-28 16:38:10] INFO: Running Magento cron and consumers processes were not found.
   [2020-01-28 16:38:10] INFO: Waiting for lock on db dump.
   [2020-01-28 16:38:10] INFO: Start creation DB dump for main database...
   [2020-01-28 16:38:10] INFO: Finished DB dump for main database, it can be found here: /tmp/qxmtlseakof6y/dump-main-1580229490.sql.gz
   [2020-01-28 16:38:10] INFO: Backup completed.
   [2020-01-28 16:38:11] NOTICE: Maintenance mode is disabled.
   ```

1. El comando `db-dump` crea un archivo `dump-<timestamp>.sql.gz` en el directorio del proyecto remoto.

>[!TIP]
>
>Si desea insertar estos datos en un entorno específico, consulte [Migrar datos y archivos estáticos](../deploy/staging-production.md#migrate-static-files).
