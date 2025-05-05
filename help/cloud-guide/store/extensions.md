---
title: Administración de extensiones
description: Obtenga información sobre cómo instalar y administrar extensiones en Adobe Commerce en la infraestructura en la nube.
feature: Cloud, Extensions, Upgrade
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '646'
ht-degree: 0%

---

# Administración de extensiones

Puede ampliar las funcionalidades de la aplicación de Adobe Commerce agregando una extensión desde el [Commerce Marketplace](https://marketplace.magento.com). Por ejemplo, puede agregar una temática para cambiar la apariencia de su tienda, o puede agregar un paquete de idioma para localizar su tienda y administrador.

>[!NOTE]
>
>Para evitar problemas de instalación, todas las compras de Marketplace deben completarse con la misma cuenta (MAGEID) que posee el proyecto en la nube.

## Nombre del compositor de una extensión

Aunque en esta sección se explica cómo obtener el nombre y la versión del Compositor de una extensión del Commerce Marketplace, puede encontrar el nombre y la versión de _cualquier_ módulo en el archivo Compositor del módulo. Abra el archivo `composer.json` en un editor de texto y anote los valores `"name"` y `"version"`.

**Para obtener el nombre del Compositor de un módulo del Commerce Marketplace**:

1. Inicie sesión en [Commerce Marketplace](https://marketplace.magento.com) con el nombre de usuario y la contraseña que utilizó para adquirir el componente.

1. En la esquina superior derecha, haz clic en tu nombre de usuario y selecciona **Mi perfil**.

   ![Acceda a su cuenta de Marketplace](../../assets/marketplace/my-profile.png)

1. En la página _Mi cuenta_, haga clic en **Mis compras**.

   ![Historial de compras en el mercado](../../assets/marketplace/my-purchases.png)

1. En la página _Mis compras_, seleccione un módulo que haya adquirido y haga clic en **Detalles técnicos**.

1. Haga clic en **Copiar** para copiar [!UICONTROL Component name] en el portapapeles.

1. Abra un editor de texto, pegue el nombre del componente y agregue dos puntos (`:`).

1. En **Detalles técnicos**, haga clic en **Copiar** para copiar [!UICONTROL Component version] en el portapapeles.

1. En el editor de texto, anexe el número de versión al nombre del componente después de dos puntos. Por ejemplo:

   ```text
   extension-name/magento2:1.0.1
   ```

## Instalación de una extensión

Adobe recomienda trabajar en una rama de desarrollo al añadir una extensión a la implementación. Al instalar una extensión, el nombre de la extensión (`<VendorName>_<ComponentName>`) se inserta automáticamente en el archivo [`app/etc/config.php`](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/deployment-files.html?lang=es). No es necesario editar el archivo directamente.

**Para instalar una extensión**:

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. Cree o desproteja una rama de desarrollo. Consulte [ramificación](../development/cli-branches.md).

1. Usando el nombre y la versión del Compositor, agregue la extensión a la sección `require` del archivo `composer.json`.

   ```bash
   composer require <extension-name>:<version> --no-update
   ```

1. Actualice las dependencias del proyecto.

   ```bash
   composer update
   ```

1. Agregar, confirmar y enviar cambios de código.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Install <extension-name>"
   ```

   ```bash
   git push origin <branch-name>
   ```

   >[!WARNING]
   >
   >Al instalar una extensión, debe incluir el archivo `composer.lock` cuando inserte cambios de código en el entorno remoto. El comando `composer install` lee el archivo `composer.lock` para habilitar las dependencias definidas en el entorno remoto.

1. Una vez finalizada la generación y la implementación, utilice un SSH para iniciar sesión en el entorno remoto y comprobar la extensión instalada.

   ```bash
   bin/magento module:status <extension-name>
   ```

   Un nombre de extensión usa el formato: `<VendorName>_<ComponentName>`.

   Respuesta de ejemplo:

   ```
   Module is enabled
   ```

   Si encuentra errores de implementación, consulte [error de implementación de extensión](../deploy/recover-failed-deployment.md).

## Administración de extensiones

Al añadir una extensión mediante Composer, el proceso de implementación habilita automáticamente la extensión. Si ya tiene la extensión instalada, puede habilitarla o deshabilitarla mediante la CLI. Al administrar las extensiones, use el formato: `<VendorName>_<ComponentName>`

Nunca habilite ni deshabilite una extensión mientras esté conectado a entornos remotos.

**Para habilitar o deshabilitar una extensión**:

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. Habilite o deshabilite un módulo. El comando `module` actualiza el archivo `config.php` con el estado solicitado del módulo.

   >Habilite un módulo.

   ```bash
   bin/magento module:enable <module-name>
   ```

   >Deshabilite un módulo.

   ```bash
   bin/magento module:disable <module-name>
   ```

1. Si habilitó un módulo, use `ece-tools` para actualizar la configuración.

   ```bash
   ./vendor/bin/ece-tools module:refresh
   ```

1. Compruebe el estado de un módulo.

   ```bash
   bin/magento module:status <module-name>
   ```

1. Agregar, confirmar y enviar cambios de código.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Disable <extension-name>"
   ```

   ```bash
   git push origin <branch-names>
   ```

## Actualización de una extensión

Antes de continuar, necesita el nombre y la versión del compositor para la extensión de. Además, confirme que la extensión es compatible con el proyecto y la versión de Adobe Commerce. En particular, [compruebe la versión de PHP necesaria](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html?lang=es) antes de comenzar.

**Para actualizar una extensión**:

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. Cree o desproteja una rama de desarrollo. Consulte [ramificación](../development/cli-branches.md).

1. Abra el archivo `composer.json` en un editor de texto.

1. Busque la extensión y actualice la versión.

1. Guarde los cambios y salga del editor de texto.

1. Actualice las dependencias del proyecto.

   ```bash
   composer update
   ```

1. Agregue, confirme e inserte los cambios de código.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Update <extension-name>"
   ```

   ```bash
   git push origin <branch-names>
   ```

Si encuentra errores, consulte [Error al recuperar del componente](../deploy/recover-failed-deployment.md). Para obtener más información acerca del uso de extensiones con Adobe Commerce, consulte [Extensiones](https://experienceleague.adobe.com/docs/commerce-admin/start/resources/extensions.html?lang=es) en la _Guía de administración_.
