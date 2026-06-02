---
title: Claves de autenticación
description: Obtenga información sobre cómo aplicar claves de autenticación a un proyecto de desarrollo en Adobe Commerce en una infraestructura en la nube.
feature: Cloud, Security
topic: Security
exl-id: b5a24fcd-9b43-4ec9-8a0c-52956a74e45e
TQID: https://experienceleague.adobe.com/nYBr0uvw1SZPSQqAU6uHTiitjZ0kcudsLdWagiWRLP8
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: ba9e5be9-7de1-4f71-a5d2-baead0e425eeid: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 318
ht-degree: 0%

---

# Claves de autenticación

Debe tener una clave de autenticación para acceder al repositorio de Adobe Commerce y habilitar los comandos de instalación y actualización para su proyecto de Adobe Commerce en la nube. Existen dos métodos para especificar las credenciales de autorización del Compositor.

- **archivo de autenticación**: un archivo que contiene las [credenciales de autorización](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/authentication-keys.html) de Adobe Commerce en el directorio raíz de la infraestructura de Adobe Commerce en la nube.
- **variable de entorno**: una variable de entorno para configurar claves de autenticación en su proyecto de Adobe Commerce en la nube para evitar la exposición accidental.

>[!BEGINSHADEBOX]

**Nota de seguridad**

Adobe recomienda usar el método [variable de entorno](#composer-auth-environment-variable) con su proyecto de nube para evitar la exposición accidental de sus credenciales de autorización.

El método del archivo de autenticación es ideal cuando se usa Cloud Docker para Commerce como herramienta de desarrollo local, pero tenga cuidado de no cargar el archivo `auth.json` en un repositorio público basado en Git. Puede agregar el archivo `auth.json` al archivo [`.gitignore`](../project/file-structure.md#ignoring-files).

>[!ENDSHADEBOX]

## Archivo de autenticación

**Para crear un archivo de `auth.json`**:

1. Si no tiene un archivo `auth.json` en el directorio raíz del proyecto, cree uno.

   - Con un editor de texto, cree un archivo `auth.json` en el directorio raíz del proyecto.
   - Copie el contenido de [ejemplo `auth.json`](https://github.com/magento/magento2/blob/2.3/auth.json.sample) en el nuevo archivo `auth.json`.

1. Reemplace `<public-key>` y `<private-key>` por sus credenciales de autenticación de Adobe Commerce.

   ```json
   {
       "http-basic": {
           "repo.magento.com": {
               "username": "<public-key>",
               "password": "<private-key>"
           }
       }
   }
   ```

1. Guarde los cambios y salga del editor de texto.

## Variable de entorno de autenticación del compositor

El siguiente método es la mejor manera de evitar la exposición accidental de credenciales confidenciales en un repositorio público basado en Git.

**Para agregar claves de autenticación mediante una variable de entorno**:

1. En _[!DNL Cloud Console]_, haga clic en el icono de configuración en el lado derecho de la navegación del proyecto.

   ![Configurar proyecto](../../assets/icon-configure.png){width="36"}

1. En la lista _Configuración del proyecto_, haga clic en **[!UICONTROL Variables]**.

1. Haga clic en **[!UICONTROL Create variable]**.

1. En el campo **[!UICONTROL Variable name]**, escriba `env:COMPOSER_AUTH`.

1. En el campo _Value_, agregue lo siguiente y reemplace `<public-key>` y `<private-key>` con sus credenciales de autenticación de Adobe Commerce:

   ```json
   {
       "http-basic": {
           "repo.magento.com": {
               "username": "<public-key>",
               "password": "<private-key>"
           }
       }
   }
   ```

1. Seleccione **[!UICONTROL Available during buildtime]** y deseleccione **[!UICONTROL Available during runtime]**.

1. Haga clic en **[!UICONTROL Create variable]**.

1. Quitar el archivo `auth.json` de cada entorno.
