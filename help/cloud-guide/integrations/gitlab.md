---
title: Integración de GitLab
description: Aprenda a integrar su proyecto de Adobe Commerce en la nube con GitLab.
feature: Cloud, Integration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '777'
ht-degree: 0%

---

# Integración de GitLab

Puede configurar un repositorio de GitLab para que cree e implemente automáticamente un entorno cuando inserte cambios en el código. Esta integración sincroniza su repositorio de GitLab con su cuenta de Adobe Commerce en la nube.

{{private-repository}}

Esta integración le permite:

- Crear un entorno al crear una rama
- Volver a implementar el entorno al combinar una solicitud de extracción
- Elimine el entorno cuando elimine la rama

Debe obtener un token de GitLab y un webhook para continuar con el proceso.

## Requisitos previos

- Acceso de administrador al proyecto de Adobe Commerce en la nube
- [`magento-cloud` herramienta CLI](../dev-tools/cloud-cli-overview.md) en su entorno local
- Una cuenta de GitLab
- Un token de acceso personal de GitLab con acceso de escritura al repositorio de GitLab. Los ámbitos seleccionados deben ser al menos: `api` y `read_repository`.

## Preparación del repositorio

Clone su proyecto de Adobe Commerce en la nube desde un entorno existente y migre las ramas del proyecto a un nuevo repositorio de GitLab vacío, preservando los mismos nombres de rama. Es **esencial** conservar un árbol Git idéntico, de modo que no se pierda ningún entorno o rama existente en su proyecto de Adobe Commerce en la nube.

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
   magento-cloud project:get <project-id>
   ```

1. Añada su repositorio de GitLab como remoto (suponiendo que GitLab se utilice en su versión SaaS).

   ```bash
   git remote add origin git@gitlab.com:<user-name>/<repo-name>.git
   ```

   El nombre predeterminado de la conexión remota puede ser `origin` o `magento`. Si existe `origin`, puede elegir un nombre diferente o puede cambiar el nombre de la referencia existente o eliminarla. Consulte [documentación remota de git](https://git-scm.com/docs/git-remote).

1. Compruebe que ha agregado correctamente el control remoto de GitLab.

   ```bash
   git remote -v
   ```

   Respuesta esperada:

   ```
   origin git@gitlab.com:<user-name>/<repo-name>.git (fetch)
   origin git@gitlab.com:<user-name>/<repo-name>.git (push)
   ```

1. Inserte los archivos del proyecto en el nuevo repositorio de GitLab. Recuerde mantener todos los nombres de rama iguales.

   ```bash
   git push -u origin master
   ```

   Si está empezando con un nuevo repositorio de GitLab, es posible que tenga que utilizar la opción `-f`, ya que el repositorio remoto no coincide con la copia local.

1. Compruebe que su repositorio de GitLab contenga todos los archivos de proyecto.

## Habilitar la integración con GitLab

Utilice el comando `magento-cloud integration` para habilitar la integración de GitLab y obtener la URL de carga útil del gancho web de GitLab para enviar actualizaciones de GitLab a su proyecto de infraestructura en la nube de Adobe Commerce.

```bash
magento-cloud integration:add --type=gitlab --project=<project-ID> --token=<your-GitLab-token> [--base-url=<GitLab-url> --server-project=<GitLab-project> --build-merge-requests={true|false} --merge-requests-clone-parent-data={true|false} --fetch-branches={true|false} --prune-branches={true|false}]
```

| Opción | Descripción |
| ------ | ----------- |
| `<project-ID>` | Su ID de proyecto de Adobe Commerce en la infraestructura en la nube |
| `<your-GitLab-token>` | El token de acceso personal que generó para GitLab. |
| `--base-url` | URL de GitLab (`https://gitlab.com/` si GitLab se usa en su versión SaaS) |
| `--server-project` | Nombre del proyecto en GitLab (parte posterior a la URL base) |
| `--build-merge-requests` | Un parámetro _optional_ que indica a Adobe Commerce en la infraestructura en la nube que cree un nuevo entorno para cada solicitud de combinación (`true` de forma predeterminada) |
| `--merge-requests-clone-parent-data` | Un parámetro _optional_ que indica a Adobe Commerce en la infraestructura en la nube que clone los datos del entorno principal para las solicitudes de combinación (`true` de forma predeterminada) |
| `--fetch-branches` | Un parámetro _optional_ que hace que Adobe Commerce en la infraestructura en la nube recupere todas las ramas del remoto (como entornos inactivos) (`true` de forma predeterminada) |
| `--prune-branches` | Un parámetro _optional_ que indica a Adobe Commerce en la infraestructura en la nube que elimine las ramas que no existen en el remoto (`true` de forma predeterminada) |

>[!WARNING]
>
>El comando `magento-cloud integration` sobrescribe el código _all_ de su proyecto de infraestructura en la nube de Adobe Commerce con el código del repositorio de GitLab. Esto incluye todas las ramas, incluida la rama `production`. Esta acción se produce de forma inmediata y no se puede deshacer. Como práctica recomendada, es importante clonar todas las ramas del proyecto de infraestructura de Adobe Commerce en la nube e insertarlas en el repositorio de GitLab antes de añadir la integración de GitLab.

**Para habilitar la integración de GitLab**:

1. Desde el terminal, añada la integración de GitLab a su proyecto de infraestructura de Adobe Commerce en la nube:

   ```bash
   magento-cloud integration:add --type gitlab --project=3txxjf32gtryos --token=qVUfeEn4ouze7A7JH --base-url=https://gitlab.com/ --server-project=my-agency/project-name --build-merge-requests=false --merge-requests-clone-parent-data=false --fetch-branches=true --prune-branches=true
   ```

1. Cuando se le pida, escriba `y` para agregar la integración.

   ```
   Warning: adding a 'gitlab' integration will automatically synchronize code from the external Git repository.
   This means it can overwrite all the code in your project.
   Are you sure you want to continue? [y/N] y
   ```

1. Copie la **URL de enlace** mostrada por el resultado devuelto.

   ```
   Hook URL: https://eu-3.magento.cloud/api/projects/3txxjf32gtryos/integrations/eolmpfizzg9lu/hook
   Created integration eolmpfizzg9lu (type: gitlab)
   +----------------------------------+---------------------------------------------------------------------------------------+
   | Property                         | Value                                                                                 |
   +----------------------------------+---------------------------------------------------------------------------------------+
   | id                               | <integration-id>                                                                      |
   | type                             | gitlab                                                                                |
   | token                            | ******                                                                                |
   | base_url                         | https://gitlab.com/                                                                   |
   | project                          | my-agency/project-name                                                                |
   | fetch_branches                   | true                                                                                  |
   | prune_branches                   | true                                                                                  |
   | build_merge_requests             | false                                                                                 |
   | merge_requests_clone_parent_data | false                                                                                 |
   | hook_url                         | https://eu-3.magento.cloud/api/projects/<project-id>/integrations/<integration-id>/hook |
   +----------------------------------+---------------------------------------------------------------------------------------+
   ```

### Añadir el webhook en GitLab

Para comunicar eventos, como solicitudes push o de combinación, con su servidor Cloud Git, debe [crear un webhook](https://docs.gitlab.com/ee/user/project/integrations/webhooks.html#overview) para su repositorio GitLab

1. En su repositorio de GitLab, haga clic en la ficha **Configuración**.

1. En la barra de navegación izquierda, haga clic en **Webhooks**.

1. En el formulario _Webhooks_, edite los campos siguientes:

   - **URL**: escriba el `Hook URL` que devolvió al habilitar la integración de GitLab.
   - **Token secreto**: escriba un secreto de verificación si es necesario.
   - **Déclencheur**: Compruebe `Merge request events` y/o `Push events` según sus necesidades.
   - **Habilitar verificación SSL**: debe seleccionar esta opción.

1. Haga clic en **Agregar gancho web**.

### Prueba de la integración

Después de configurar la integración de GitLab, puede verificar que la integración esté operativa mediante la CLI `magento-cloud`:

```bash
magento-cloud integration:validate
```

O puede probarlo insertando un cambio simple en su repositorio de GitLab.

1. Cree un archivo de prueba.

   ```bash
   touch test.md
   ```

1. Confirme e inserte el cambio en el repositorio de GitLab.

   ```bash
   git add . && git commit -m "Testing GitLab integration" && git push
   ```

1. Inicie sesión en [[!DNL Cloud Console]](../project/overview.md) y compruebe que se muestra el mensaje de confirmación y que se implementa el proyecto.

## Crear una rama de nube

Utilice el comando `magento-cloud` CLI `environment:push` para crear y activar un entorno nuevo. Consulte [Crear una rama de nube](bitbucket.md#create-a-cloud-branch).

## Eliminación de la integración

Utilice el comando `magento-cloud` CLI `integration:delete` para eliminar la integración. Consulte [Quitar la integración](bitbucket.md#remove-the-integration).
