---
title: Integración de Bitbucket
description: Aprenda a integrar su proyecto de Adobe Commerce en la nube con Bitbucket.
feature: Cloud, Integration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1020'
ht-degree: 0%

---

# Integración de Bitbucket

Puede configurar el repositorio de Bitbucket para que cree e implemente automáticamente un entorno cuando inserte cambios en el código. Esta integración sincroniza su repositorio de Bitbucket con su cuenta de Adobe Commerce en la infraestructura de la nube.

{{private-repository}}

## Requisitos previos

- Acceso de administrador al proyecto de Adobe Commerce en la nube
- [`magento-cloud` herramienta CLI](../dev-tools/cloud-cli-overview.md) en su entorno local
- Una cuenta de Bitbucket
- Acceso de administrador al repositorio de Bitbucket
- Una clave de acceso SSH para el repositorio de Bitbucket

## Preparación del repositorio

Clone su proyecto de Adobe Commerce en la nube desde un entorno existente y migre las ramas del proyecto a un nuevo repositorio de Bitbucket vacío, preservando los mismos nombres de rama. Es **esencial** conservar un árbol Git idéntico, de modo que no se pierda ningún entorno o rama existente en su proyecto de Adobe Commerce en la nube.

1. Desde el terminal, inicie sesión en su proyecto de infraestructura de Adobe Commerce en la nube.

   ```bash
   magento-cloud login
   ```

1. Enumere sus proyectos y copie el ID del proyecto.

   ```bash
   magento-cloud project:list
   ```

1. Clone el proyecto en el entorno local.

   ```bash
   magento-cloud project:get <project-ID>
   ```

1. Añada el repositorio de Bitbucket como remoto.

   ```bash
   git remote add origin git@bitbucket.org:<user-name>/<repo-name>.git
   ```

   El nombre predeterminado de la conexión remota puede ser `origin` o `magento`. Si existe `origin`, puede elegir un nombre diferente o puede cambiar el nombre de la referencia existente o eliminarla. Consulte [documentación remota de git](https://git-scm.com/docs/git-remote).

1. Compruebe que ha agregado correctamente el control remoto Bitbucket.

   ```bash
   git remote -v
   ```

   Respuesta esperada:

   ```
   origin git@bitbucket.org:<user-name>/<repo-name>.git (fetch)
   origin git@bitbucket.org:<user-name>/<repo-name>.git (push)
   ```

1. Inserte los archivos del proyecto en el nuevo repositorio de Bitbucket. Recuerde mantener todos los nombres de rama iguales.

   ```bash
   git push -u origin master
   ```

   Si está empezando con un nuevo repositorio de bloque de bits, es posible que tenga que utilizar la opción `-f`, ya que el repositorio remoto no coincide con la copia local.

1. Compruebe que el repositorio de Bitbucket contenga todos los archivos de proyecto.

## Crear un consumidor de OAuth

La integración de Bitbucket requiere un [consumidor de OAuth](https://support.atlassian.com/bitbucket-cloud/docs/use-oauth-on-bitbucket-cloud/). Necesita OAuth `key` y `secret` de este consumidor para completar la siguiente sección.

**Para crear un consumidor de OAuth en Bitbucket**:

1. Inicie sesión en su cuenta de [Bitbucket](https://id.atlassian.com/login).

1. Haga clic en **Configuración** > **Administración de acceso** > **OAuth**.

1. Haga clic en **Agregar consumidor** y configúrelo de la siguiente manera:

   ![Configuración de consumidor de OAuth de bloque de bits](../../assets/oauth-consumer-config.png)

   >[!WARNING]
   >
   >No se requiere una **URL de devolución de llamada** válida, pero debe escribir un valor en este campo para completar correctamente la integración.

1. Haga clic en **Guardar**.

1. Haga clic en el consumidor **Name** para mostrar su OAuth `key` y `secret`.

1. Copie sus OAuth `key` y `secret` para configurar la integración.

## Configuración de la integración

1. Desde el terminal, vaya a su proyecto de infraestructura de Adobe Commerce en la nube.

1. Cree un archivo temporal denominado `bitbucket.json` y agregue lo siguiente; reemplace las variables entre corchetes angulares por sus valores:

   ```json
   {
     "type": "bitbucket",
     "repository": "<bitbucket-user-name/bitbucket-repo-name>",
     "app_credentials": {
       "key": "<oauth-consumer-key>",
       "secret": "<oauth-consumer-secret>"
     },
     "prune_branches": true,
     "fetch_branches": true,
     "build_pull_requests": true,
     "resync_pull_requests": true
   }
   ```

   >[!TIP]
   >
   >Asegúrese de utilizar el nombre del repositorio de Bitbucket y no la dirección URL. La integración falla si utiliza una dirección URL.

1. Agregue la integración a su proyecto utilizando la herramienta CLI `magento-cloud`.

   >[!WARNING]
   >
   >El siguiente comando sobrescribe el código _all_ de su proyecto de infraestructura de Adobe Commerce en la nube con el código de su repositorio de Bitbucket. Esto incluye todas las ramas, incluida la rama `production`. Esta acción se produce de forma inmediata y no se puede deshacer. Como práctica recomendada, es importante clonar todas las ramas de su proyecto de infraestructura de Adobe Commerce en la nube e insertarlas en el repositorio de Bitbucket **antes** de agregar la integración de Bitbucket.

   ```bash
   magento-cloud project:curl -p '<project-ID>' /integrations -i -X POST -d "$(< bitbucket.json)"
   ```

   Esto devuelve una respuesta HTTP larga con encabezados. Una integración correcta devuelve un código de estado 200 o 201. Un estado de 400 o superior indica que se ha producido un error.

1. Eliminar el archivo temporal `bitbucket.json`.

1. Compruebe la integración del proyecto.

   ```bash
   magento-cloud integrations -p <project-ID>
   ```

   ```
   +----------+-----------+--------------------------------------------------------------------------------+
   | ID       | Type      | Summary                                                                        |
   +----------+-----------+--------------------------------------------------------------------------------+
   | <int-id> | bitbucket | Repository: bitbucket_Account/magento-int                                      |
   |          |           | Hook URL:                                                                      |
   |          |           | https://magento-url.cloud/api/projects/<project-id>/integrations/<int-id>/hook |
   +----------+-----------+--------------------------------------------------------------------------------+
   ```

   Tome nota de la **URL de enlace** para configurar un enlace web en BitBucket.

### Añadir un webhook en BitBucket

Para poder comunicar eventos (como una notificación push) con su servidor Cloud Git, es necesario tener un webhook para su repositorio BitBucket. El método de configuración de una integración de Bitbucket detallado en esta página, cuando se sigue correctamente, crea automáticamente un webhook. Es importante verificar el webhook para evitar la creación de múltiples integraciones.

1. Inicie sesión en su cuenta de [Bitbucket](https://id.atlassian.com/login).

1. Haga clic en **Repositorios** y seleccione el proyecto.

1. Haga clic en **Configuración del repositorio** > **Flujo de trabajo** > **Webhooks**.

1. Verifique el webhook antes de continuar.

   Si el vínculo está activo, omita los pasos restantes y [Pruebe la integración](#test-the-integration). El enlace debe tener un nombre similar a **&quot;Adobe Commerce en la infraestructura en la nube &lt;project_id>&quot;** y un formato de URL de enlace similar a: `https://<zone>.magento.cloud/api/projects/<project_id>/integrations/<id>/hook`

1. Haga clic en **Agregar gancho web**.

1. En la vista _Agregar nuevo gancho web_, edite los campos siguientes:

   - **Título**: Integración de Adobe Commerce
   - **URL**: utiliza la URL de enlace de tu lista de integración de `magento-cloud`
   - **Déclencheur**: El valor predeterminado es una _inserción de repositorio básica_

1. Haga clic en **Guardar**.

### Prueba de la integración

Después de configurar la integración de Bitbucket, puede verificar que la integración esté operativa mediante la CLI `magento-cloud`:

```bash
magento-cloud integration:validate
```

O puede probarlo insertando un cambio simple en su repositorio de Bitbucket.

1. Cree un archivo de prueba.

   ```bash
   touch test.md
   ```

1. Confirme y envíe el cambio al repositorio de Bitbucket.

   ```bash
   git add . && git commit -m "Testing Bitbucket integration" && git push
   ```

1. Inicie sesión en [[!DNL Cloud Console]](../project/overview.md) y compruebe que se muestra el mensaje de confirmación y que se implementa el proyecto.

   ![Probando la integración de Bitbucket](../../assets/bitbucket-integration.png)

## Crear una rama de nube

La integración de Bitbucket no puede activar nuevos entornos en su proyecto de Adobe Commerce en la nube. Si crea un entorno con Bitbucket, debe activarlo manualmente. Para evitar este paso adicional, se recomienda crear entornos utilizando la herramienta CLI `magento-cloud` o [!DNL Cloud Console].

**Para activar una rama creada con Bitbucket**:

1. Utilice la CLI `magento-cloud` para insertar la rama.

   ```bash
   magento-cloud environment:push from-bitbucket
   ```

   ```
   Pushing from-bitbucket to the new environment from-bitbucket
   Activate from-bitbucket after pushing? [Y/n] y
   Parent environment [master]: integration
   --- (Validation and activation messages)
   ```

1. Compruebe que el entorno esté activo.

   ```bash
   magento-cloud environment:list
   ```

   ```
   Your environments are:
   +---------------------+----------------+--------+
   | ID                  | Name           | Status |
   +---------------------+----------------+--------+
   | master              | Master         | Active |
   |  integration        | integration    | Active |
   |    from-bitbucket * | from-bitbucket | Active |
   +---------------------+----------------+--------+
   * - Indicates the current environment
   ```

Después de crear un entorno, puede insertar la rama correspondiente en el repositorio remoto de Bitbucket utilizando comandos normales de Git. Los cambios posteriores en la rama en Bitbucket generan e implementan automáticamente el entorno.

## Eliminación de la integración

Puede eliminar de forma segura la integración de Bitbucket de su proyecto sin afectar al código.

**Para quitar la integración de Bitbucket**:

1. Desde el terminal, inicie sesión en su proyecto de infraestructura de Adobe Commerce en la nube.

1. Enumere las integraciones. Necesita el ID de integración de Bitbucket para completar el siguiente paso.

   ```bash
   magento-cloud integration:list
   ```

1. Elimine la integración.

   ```bash
   magento-cloud integration:delete <int-ID>
   ```

Además, puede eliminar la integración de Bitbucket iniciando sesión en su cuenta de Bitbucket y revocando la concesión de OAuth en la página de la cuenta _Configuración_.

## Integración del servidor Bitbucket

Para utilizar la integración del servidor Bitbucket, necesita lo siguiente:

- [Token de acceso Bitbucket](https://confluence.atlassian.com/bitbucketserver/http-access-tokens-939515499.html)—Genere un token que conceda acceso al Proyecto `read` y acceso al Repositorio `admin`
- [URL del servidor Bitbucket](https://confluence.atlassian.com/bitbucketserver/specify-the-bitbucket-base-url-776640392.html)—Añada la URL base de su instancia de Bitbucket

Aunque puede utilizar la CLI de nube para recorrer los pasos de integración del servidor Bitbucket, el comando completo tiene un aspecto similar al siguiente:

```bash
magento-cloud integration:add --type=bitbucket_server --base-url=<bitbucket-url> --username=<username> --token=<bitbucket-access-token> --project=<project-ID>
```

Use el comando de ayuda para obtener más opciones y requisitos de uso: `magento-cloud integration:add --help`
