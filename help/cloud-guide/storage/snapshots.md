---
title: Administración de backup
description: Obtenga información sobre cómo crear y restaurar manualmente una copia de seguridad para su proyecto de Adobe Commerce en la nube.
feature: Cloud, Paas, Snapshots, Storage
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '711'
ht-degree: 0%

---

# Administración de backup

Puede realizar una copia de seguridad manual de los entornos de inicio activos en cualquier momento mediante el botón **[!UICONTROL Backup]** en [!DNL Cloud Console] o mediante el comando `magento-cloud snapshot:create`.

Una copia de seguridad de _snapshot_ es una copia de seguridad completa de los datos del entorno que incluye todos los datos persistentes de los servicios en ejecución (base de datos MySQL) y cualquier archivo almacenado en los volúmenes montados (var, pub/media, app/etc). La instantánea _no_ incluye código, ya que el código ya está almacenado en el repositorio basado en Git. No se puede descargar una copia de una instantánea.

La función de copia de seguridad/instantánea **no** se aplica a los entornos de ensayo y producción de Pro, que reciben copias de seguridad regulares para la recuperación ante desastres de forma predeterminada. Consulte [Copia de seguridad Pro y recuperación ante desastres](../architecture/pro-architecture.md#backup-and-disaster-recovery) para obtener más información. A diferencia de las copias de seguridad automáticas en los entornos de ensayo y producción de Pro, las copias de seguridad son **no** automáticas. Es _su responsabilidad_ crear manualmente una copia de seguridad o configurar un trabajo cron para crear periódicamente una copia de seguridad de sus entornos de integración de Starter o Pro.

## Creación de una copia de seguridad manual

Puede crear una copia de seguridad manual de cualquier entorno de inicio activo y de integración Pro desde [!DNL Cloud Console] o crear una instantánea desde la CLI de la nube. Debe tener un [rol de administrador](../project/user-access.md) para el entorno.

**Para crear una copia de seguridad de cualquier entorno de inicio con el[!DNL Cloud Console]**:

1. Inicie sesión en [[!DNL Cloud Console]](https://console.adobecommerce.com).
1. Seleccione un entorno de la barra de navegación del proyecto. El entorno debe estar activo.
1. En la vista _Copias de seguridad_, haga clic en **[!UICONTROL Backup]**. Esta opción no está disponible para un entorno Pro.

   ![Copia de seguridad](../../assets/button-backup.png){width="150"}

**Para crear una copia de seguridad de un entorno de integración con[!DNL Cloud Console]**:

1. Inicie sesión en [[!DNL Cloud Console]](https://console.adobecommerce.com).
1. Seleccione un entorno de integración/desarrollo en la barra de navegación del proyecto. El entorno debe estar activo.
1. Seleccione la opción **[!UICONTROL Backup]** en el menú superior derecho. Esta opción está disponible tanto para entornos Starter como Pro.
1. Haga clic en el botón **[!UICONTROL Yes]**.

**Para crear una instantánea utilizando la CLI`magento-cloud`**:

1. En la estación de trabajo local, cambie al directorio del proyecto.
1. Consulte la rama de entorno para acceder a la instantánea.
1. Cree la instantánea.

   ```bash
   magento-cloud snapshot:create --live
   ```

   También puede usar el comando corto `magento-cloud backup`. La opción `--live` deja el entorno en ejecución para evitar el tiempo de inactividad. Para obtener una lista completa de opciones, escriba `magento-cloud snapshot:create --help`.

   Respuesta de ejemplo:

   ```
   Creating a snapshot of develop-branch
   Waiting for the activity ID (User created a backup of develop-branch):
   
   Creating backup of develop-branch
   Created backup my-snapshot
   [============================] 45 secs (complete)
   Activity ID succeeded
   Snapshot name: my-snapshot
   ```

1. Compruebe las instantáneas más recientes.

   ```bash
   magento-cloud snapshot:list
   ```

   La lista devuelve información sobre el estado de la instantánea:

   ```
   Snapshots on the project (project-id), environment develop-branch (type: development):
   +---------------------------+----------------------+------------+
   | Created                   | Snapshot ID          | Restorable |
   +---------------------------+----------------------+------------+
   | 2023-03-08T17:07:01+00:00 | my-snapshot          | true       |
   +---------------------------+----------------------+------------+
   ```

## Restaurar una copia de seguridad manual

Debe tener [acceso de administrador](../project/user-access.md) al entorno. Tiene hasta **siete días** para _restaurar_ una copia de seguridad manual. La restauración de una copia de seguridad no cambia el código de la rama de Git actual. La restauración de una copia de seguridad de esta manera no se aplica a los entornos de ensayo y producción Pro; consulte [Copia de seguridad Pro y recuperación ante desastres](../architecture/pro-architecture.md#backup-and-disaster-recovery).

Los tiempos de restauración varían según el tamaño de la base de datos:

- una base de datos de gran tamaño (más de 200 GB) puede tardar 5 horas
- la base de datos media (150 GB) puede tardar 2 horas y media
- una base de datos pequeña (60 GB) puede tardar una hora

>[!TIP]
>
>Restauración sin copia de seguridad:
>
>- Para revertir al código anterior o quitar las extensiones agregadas en un entorno, consulte [Revertir código](#roll-back-code).
>- Para restaurar un entorno inestable que _no_ tiene una copia de seguridad, consulte [Restaurar un entorno](../development/restore-environment.md).

**Para restaurar una copia de seguridad mediante[!DNL Cloud Console]**:

1. Inicie sesión en [[!DNL Cloud Console]](https://console.adobecommerce.com).
1. Seleccione un entorno de la barra de navegación del proyecto.
1. En la vista _Copias de seguridad_, elija una copia de seguridad de la lista _Almacenada_. La función de copia de seguridad **no** se aplica a los entornos Pro.
1. En el menú ![Más](../../assets/icon-more.png){width="32"} (_más_), haga clic en **Restaurar**.
1. Revise la información de Restaurar a partir de la copia de seguridad y haga clic en **Sí, restaurar**.

**Para restaurar una instantánea mediante la CLI de nube**:

1. En la estación de trabajo local, cambie al directorio del proyecto.
1. Consulte la rama de entorno para restaurar.
1. Enumerar todas las instantáneas disponibles.

   ```bash
   magento-cloud snapshot:list
   ```

   La lista devuelve información sobre las instantáneas disponibles:

   ```
   Snapshots on the project (project-id), environment develop-branch (type: development):
   +---------------------------+----------------------+------------+
   | Created                   | Snapshot ID          | Restorable |
   +---------------------------+----------------------+------------+
   | 2023-03-08T17:07:01+00:00 | my-snapshot          | true       |
   +---------------------------+----------------------+------------+
   ```

1. Restaure una instantánea con el ID de instantánea de la lista.

   ```bash
   magento-cloud snapshot:restore <snapshot-id>
   ```

## Restaurar una instantánea de recuperación ante desastres

Para restaurar la instantánea de recuperación ante desastres en entornos de ensayo y producción de Pro, [Importe el volcado de la base de datos directamente desde el servidor](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/how-to/restore-a-db-snapshot-from-staging-or-production#meth3).

## Revertir código

Las copias de seguridad e instantáneas _no_ incluyen una copia de su código. El código ya está almacenado en el repositorio basado en Git, por lo que puede utilizar comandos basados en Git para revertir código. Por ejemplo, use `git log --oneline` para desplazarse por confirmaciones anteriores; a continuación, use [`git revert`](https://git-scm.com/docs/git-revert) para restaurar el código de una confirmación específica.

Además, puede elegir almacenar código en una rama _inactiva_. Use comandos git para crear una rama en lugar de usar `magento-cloud` comandos. Consulte acerca de [comandos Git](../dev-tools/cloud-cli-overview.md#git-commands) en el tema sobre la CLI de la nube.
