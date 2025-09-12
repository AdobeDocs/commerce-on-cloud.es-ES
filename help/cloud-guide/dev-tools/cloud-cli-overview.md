---
title: CLI de nube
description: Obtenga información acerca de la CLI de magento en la nube y cómo le ayuda a administrar los entornos de desarrollo local para su proyecto de infraestructura de Adobe Commerce en la nube.
exl-id: 71a705f2-8672-4125-b539-b7b1621f2f64
source-git-commit: 82d89f442792baec995dd0be40f2a49cba168f76
workflow-type: tm+mt
source-wordcount: '805'
ht-degree: 0%

---

# CLI de nube

`magento-cloud` CLI es una herramienta de línea de comandos que permite a los desarrolladores y administradores de sistemas administrar Adobe Commerce en proyectos y entornos de infraestructura en la nube desde su estación de trabajo local.

Esta herramienta amplía la funcionalidad de [[!DNL Cloud Console]](../../get-started/cloud-console.md) al proporcionar capacidades de automatización adicionales y acceso directo a las características de administración de proyectos. Después de instalar la herramienta localmente, puede utilizarla para administrar los entornos de integración de Starter y Pro.

>[!NOTE]
>
>Se trata de una herramienta local y solo es compatible con sistemas operativos basados en Unix. Windows no es compatible. No se puede instalar en el entorno de nube (que es de solo lectura) mediante el método descrito en esta página. Solo puede instalar módulos en el entorno de la nube mediante uno de los **flujos de trabajo de implementación** siguientes.
>
>- [Flujo de trabajo de implementación Pro](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/architecture/pro-develop-deploy-workflow#deployment-workflow)
>- [Flujo de trabajo de implementación inicial](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/architecture/starter-develop-deploy-workflow)

**Para instalar la CLI`magento-cloud` de**:

1. En su _estación de trabajo local_, cambie al directorio donde quiere clonar el proyecto de Cloud y donde el [propietario del sistema de archivos](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html?lang=es) tiene acceso de _escritura_.

1. Instale la CLI `magento-cloud`.

   ```bash
   curl -sS https://accounts.magento.cloud/cli/installer | php
   ```

1. Agregar CLI `magento-cloud` al perfil bash.

   ```bash
   export PATH=$PATH:$HOME/.magento-cloud/bin
   ```

1. Vuelva a cargar el perfil de bash actualizado.

   ```bash
   . ~/.bash_profile
   ```

1. Para iniciar la CLI, llame a `magento-cloud` e indique las credenciales de su cuenta de Cloud cuando se le solicite.

   ```bash
   magento-cloud
   ```

   ```
   Welcome to Magento Cloud!
   Please log in using your Magento Cloud account.
   Your email address or username:
   ```

1. Compruebe que el comando `magento-cloud` se encuentra en la ruta de acceso. En el ejemplo siguiente se enumeran los comandos disponibles.

   ```bash
   magento-cloud list
   ```

## Comandos comunes

Adobe diseñó estos comandos para administrar los entornos de integración en la nube y recomienda que ejecute la CLI `magento-cloud` desde un directorio de proyecto para poder omitir el parámetro `-p <project-ID>`.

La siguiente lista de `magento-cloud` comandos CLI usados con frecuencia incluye solamente las opciones requeridas. Puede utilizar la opción `--help` con cualquier comando para ver más información.

| Comando | Descripción |
| ------------------------------------ | -------------------------------------------------- |
| `magento-cloud login` | Inicie sesión en el proyecto. |
| `magento-cloud list` | Enumerar los comandos disponibles para la herramienta CLI. |
| `magento-cloud environment:list` | Enumerar los entornos del proyecto actual. |
| `magento-cloud environment:checkout` | Consulte un entorno existente. |
| `magento-cloud environment:merge -e` | Combinar cambios en este entorno con su elemento principal. |
| `magento-cloud variables` | Variables de lista en este entorno. |
| `magento-cloud ssh` | Utilice SSH para conectarse al entorno remoto. |
| `magento-cloud url` | Abra la tienda de Adobe Commerce en un explorador. |
| `magento-cloud web` | Abra [!DNL Cloud Console]. |

## Comandos de entorno

El entorno _name_ es diferente del entorno _ID_ solo si el nombre del entorno contiene espacios o mayúsculas. Un ID de entorno consta de todas las letras minúsculas, números y símbolos permitidos. Las letras mayúsculas en un nombre de entorno se convierten a minúsculas en el ID.; los espacios en un nombre de entorno se convierten en guiones.

Un nombre de entorno _no puede_ incluir caracteres reservados para su shell de Linux o para expresiones regulares. Los caracteres prohibidos incluyen llaves (`{ }`), paréntesis, asteriscos (`*`), corchetes angulares (`< >`), signo &amp; (`&`), porcentaje (`%`) y otros caracteres.

El comando `magento-cloud environment:list` muestra jerarquías de entorno, mientras que `git branch` no lo hace. Si tiene entornos anidados, utilice lo siguiente:

```bash
magento-cloud environment:list
```

### Volver a implementar el entorno

Déclencheur una reimplementación sin utilizar una notificación push. Compruebe y confirme el entorno que desea volver a implementar. No utilice volver a implementar si hay una compilación en estado pendiente.

```bash
magento-cloud environment:redeploy
```

Respuesta de ejemplo:

```
Are you sure you want to redeploy the environment <environment-name>? [Y/n]
```

{{redeploy-warning}}

## Comandos Git

Puede observar que algunos de estos comandos son similares a los comandos Git. Los comandos de `magento-cloud` se conectan directamente al proyecto de nube basado en Git con características adicionales. Si crea una rama sin usar la CLI `magento-cloud`, no se &quot;activa&quot; y no se genera automáticamente al insertar cambios en el entorno remoto. El comando CLI `magento-cloud` incluye la activación.

Para crear una rama, use el comando `magento-cloud` para que se active la rama.

```bash
magento-cloud environment:branch <new-name> <parent-branch>
```

Para el estado de sucursal:

- Utilice el comando `magento-cloud env` para ver una lista de las ramas del entorno y su estado: activo o inactivo.
- Utilice el comando `magento-cloud environment:activate` para activar una rama de entorno.

Inserte un compromiso Git vacío para almacenar en déclencheur una implementación. Por ejemplo:

```bash
git commit --allow-empty -m "redeploy" && git push <branch-name>
```

Algunas acciones, como agregar un usuario, no resultan en una implementación.

### Crear una rama de entorno

Los siguientes pasos muestran el uso de los comandos CLI y Git de forma intercambiable para administrar el entorno local:

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. Cambiar al [propietario del sistema de archivos](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html?lang=es).

1. Inicie sesión en el proyecto.

   ```bash
   magento-cloud login
   ```

1. Enumere sus proyectos.

   ```bash
   magento-cloud project:list
   ```

1. Enumerar entornos en el proyecto. Cada entorno incluye una rama de Git activa que contiene el código, la base de datos, las variables de entorno, las configuraciones y los servicios.

   ```bash
   magento-cloud environment:list
   ```

   >[!NOTE]
   >
   >Es importante usar el comando `magento-cloud environment:list` porque muestra jerarquías de entorno, mientras que el comando `git branch` no lo hace.

1. Recupere las ramas de origen para obtener el código más reciente.

   ```bash
   git fetch origin
   ```

1. Cierre la compra o cambie a una rama y un entorno específicos.

   ```bash
   magento-cloud environment:checkout <environment-ID>
   ```

   Los comandos Git solo comprueban la rama Git. El comando `magento-cloud checkout` extrae la rama y cambia al entorno activo.

   >[!TIP]
   >
   >Puede crear una rama de entorno utilizando la sintaxis de comando `magento-cloud environment:branch <environment-name> <parent-environment-ID>`. Puede llevar algún tiempo adicional crear y activar una rama de entorno.

1. Utilice el ID de entorno para extraer cualquier código actualizado de su local de. Esto no es necesario si la rama de entorno es nueva.

   ```bash
   git pull origin <environment-ID>
   ```

1. (_Opcional_) Cree una [instantánea](../storage/snapshots.md) del entorno como copia de seguridad.

   ```bash
   magento-cloud snapshot:create -e <environment-ID>
   ```

## Actualizar la CLI

La CLI `magento-cloud` comprueba las actualizaciones disponibles cuando inicia sesión, pero puede buscar actualizaciones mediante el comando `self:update`. Si hay una actualización disponible, siga las instrucciones para actualizar la CLI.

Si la CLI de `magento-cloud` está actualizada, verá la siguiente respuesta:

```bash
magento-cloud update
```

```
Checking for Magento Cloud CLI updates (current version: X.XX.X)
No updates found
```
