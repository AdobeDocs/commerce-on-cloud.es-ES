---
title: Administrar ramas con la CLI
description: Obtenga información sobre cómo administrar las ramas de entorno para Adobe Commerce en la infraestructura en la nube mediante la CLI de la nube.
role: Developer
feature: Cloud, Install
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '676'
ht-degree: 0%

---

# Administrar ramas con la CLI

Para instalar la CLI `magento-cloud`, consulte la [referencia de CLI de nube](../dev-tools/cloud-cli-overview.md). Después de instalar la CLI `magento-cloud` y de configurar las claves SSH para el acceso remoto a su infraestructura en la nube, puede usar los comandos CLI `magento-cloud` para administrar los entornos de sus proyectos. Para obtener información acerca de la arquitectura del entorno, consulte [Arquitectura inicial](../architecture/starter-architecture.md) o [Arquitectura profesional](../architecture/pro-architecture.md).

Para administrar las ramas y los entornos con [!DNL Cloud Console], consulte [Administrar ramas con [!DNL Cloud Console]](../project/console-branches.md).

## Utilizar comandos CLI

Los comandos CLI `magento-cloud` son similares a los comandos Git. Puede utilizarlas para conectarse al proyecto y administrar los entornos. Aunque puede ejecutar los comandos desde cualquier directorio, se recomienda ejecutarlos desde un directorio de proyecto. Cuando se ejecuta desde un directorio de proyecto, puede omitir el parámetro `-p <project-ID>`. Consulte la [referencia de CLI de nube](../dev-tools/cloud-cli-overview.md).

## Clonar el proyecto

Las siguientes instrucciones utilizan una combinación de `magento-cloud` comandos CLI y comandos Git para clonar el proyecto en la estación de trabajo local. Para ver una lista completa de `magento-cloud` comandos CLI, use el comando `magento-cloud list`.

>[!IMPORTANT]
>
>Algunos comandos Git no pueden completar una acción en su proyecto de infraestructura en la nube de Adobe Commerce. Por ejemplo, puede crear una rama con un comando Git, pero no puede crear y activar un entorno nuevo. Debe crear un entorno usando el comando `magento-cloud environment:branch <branch-name>` para que el entorno se vuelva _activo_. También puede usar [!DNL Cloud Console] para crear entornos activos. Consulte [Referencia de CLI de nube](../dev-tools/cloud-cli-overview.md#git-commands).

**Para clonar un entorno de proyecto `master`**:

1. Inicie sesión en la estación de trabajo local con una cuenta de [propietario del sistema de archivos](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html).

1. Cambie al directorio _docroot_ del servidor web o del host virtual.

1. Inicie sesión con la CLI `magento-cloud`.

   ```bash
   magento-cloud login
   ```

1. Enumere sus proyectos.

   ```bash
   magento-cloud project:list
   ```

1. Clonar un proyecto.

   ```bash
   magento-cloud project:get <project-ID>
   ```

   Cuando se le solicite, proporcione un nombre de directorio.

1. Cambie al directorio `magento2`.

1. Enumerar los entornos disponibles para el proyecto.

   ```bash
   magento-cloud environment:list
   ```

   >[!IMPORTANT]
   >
   >El comando `magento-cloud environment:list` muestra jerarquías de entorno, mientras que el comando `git branch` no lo hace.

1. Recupere las ramas remotas.

   ```bash
   git fetch origin
   ```

1. Extraer código actualizado.

   ```bash
   git pull origin <environment-ID>
   ```

>[!TIP]
>
>Consulte [Integraciones](../integrations/overview.md) para obtener información sobre cómo usar los servicios de alojamiento basados en Git con Adobe Commerce en la infraestructura en la nube.

## Crear una rama para desarrollo

Después de clonar el proyecto y actualizar la configuración de cuenta de administrador de Adobe Commerce, puede ramificar para desarrollo. Como se dijo anteriormente, debe crear un entorno usando el comando `magento-cloud environment:branch <branch-name>` o el [!DNL Cloud Console] para que el entorno se vuelva _activo_.

- Para [Starter](../architecture/starter-develop-deploy-workflow.md#clone-and-branch), considere la posibilidad de crear una rama para `staging` y después cree una rama de desarrollo basada en la rama `staging`.
- Para [Pro](../architecture/pro-develop-deploy-workflow.md#development-workflow), cree ramas de desarrollo basadas en la rama `Integration`.

**Para crear una rama de desarrollo**:

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. Cree un entorno basado en la rama recomendada para el flujo de trabajo del proyecto.

   ```bash
   magento-cloud branch <new-environment-name> integration
   ```

1. Actualizar dependencias.

   ```bash
   composer --no-ansi --no-interaction install --no-progress --prefer-dist --optimize-autoloader
   ```

1. [_opcional_] Crear una [copia de seguridad](../storage/snapshots.md) del entorno.

### Combinar una rama

Después de completar el desarrollo, combine esta rama con la principal:

1. Confirmar y enviar cambios de código:

   ```bash
   git add -A && git commit -m "Add message here"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. Combinar con el entorno principal:

   ```bash
   magento-cloud environment:merge <environment-ID>
   ```

### Eliminar un entorno

Solo elimine un entorno si está seguro de que ya no lo necesita. No puede recuperar un entorno después de eliminarlo.

>[!WARNING]
>
>No puede eliminar la rama `master` de ningún proyecto.

Debe ser administrador de proyecto, administrador de entorno o propietario de cuenta para realizar esta tarea. Consulte [Administrar el acceso de los usuarios a los proyectos en la nube](../project/user-access.md).

Cuando elimina un entorno, el entorno se establece en _inactivo_. El código sigue disponible en la rama Git, pero ya no contiene los servicios ni la base de datos. Para eliminar el entorno por completo, también debe eliminar la rama Git remota correspondiente.

**Para eliminar un entorno**:

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. Buscar actualizaciones del servidor remoto.

   ```bash
   git fetch
   ```

1. Elimine la rama de entorno.

   ```bash
   magento-cloud environment:delete <environment-ID>
   ```

   De forma opcional, puede eliminar más de un entorno a la vez si agrega varios ID de entorno al comando Eliminar.

   ```bash
   magento-cloud environment:delete <environment-1-ID> <environment-2-ID>
   ```

1. Responda a las solicitudes para eliminar el entorno local y el entorno remoto correspondiente.

   ```
   The environment <environment-ID> is currently active: deleting it will delete all associated data.
   Are you sure you want to delete the environment <environment-ID>? [Y/n]
   ```

   Al eliminar el entorno, se coloca en un estado _inactivo_.

   ```
   Delete the remote Git branch too? [Y/n]
   ```

   Al eliminar la rama Git remota, se elimina el entorno del proyecto.

1. Espere a que se elimine el entorno.

   ```
   Deleting environment <environment-ID>
   Waiting for the activity...
     Deleting environment <project-id>-<environment-ID>-xxxxxx
   
     [============================]  1 min (complete)
   Activity ID succeeded
   Deleted remote Git branch <environment-ID>
   Run git fetch --prune to remove deleted branches from your local cache.
   ```

>[!TIP]
>
>Para activar un entorno inactivo, use el comando `magento-cloud environment:activate`.

## Interacción con entornos remotos

Una vez que haya [configurado las claves SSH](../development/secure-connections.md), podrá [conectarse desde su área de trabajo local a un entorno remoto](../development/secure-connections.md#connect-to-a-remote-environment), interactuar con los servicios del proyecto y modificar la configuración.
