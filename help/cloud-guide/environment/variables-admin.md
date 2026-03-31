---
title: Variables de ADMINISTRACIÓN
description: Consulte una lista de variables de entorno utilizadas al instalar Adobe Commerce en la infraestructura en la nube.
feature: Cloud, Configuration, Install, Roles/Permissions
role: Developer
exl-id: d2746185-bc59-4d30-a088-73df1bd2c0b2
source-git-commit: ac1b2001294ba72304fc7ad3c760872dbd73e44f
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 0%

---

# Variables de administración

Los usuarios que tienen acceso administrativo al proyecto de infraestructura de Adobe Commerce en la nube pueden utilizar las siguientes variables de entorno de proyecto para anular los ajustes de configuración de la cuenta de usuario administrativa y acceder a la IU de administración.

## Credenciales de administrador

Puede anular las credenciales de usuario de administrador durante la instalación de Commerce con las variables ADMIN de la siguiente tabla.

Si desea cambiar los valores después de la instalación, conéctese a su entorno usando SSH y use el comando [`admin:user` de la CLI de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/admin.html) para crear o editar las credenciales del usuario administrador.

| Variable | Predeterminado | Descripción |
| -------------- | --------------------------- | ----------- |
| `ADMIN_USERNAME` | Nombre de usuario del propietario de licencia | Un nombre de usuario para el usuario administrativo con la capacidad de crear otros usuarios, incluidos los usuarios administrativos. |
| `ADMIN_FIRSTNAME` | Nombre del propietario de la licencia | El nombre del usuario administrativo. |
| `ADMIN_LASTNAME` | Apellidos del propietario de la licencia | Apellidos del usuario administrativo. |
| `ADMIN_EMAIL` | Correo electrónico del propietario de licencia | Dirección de correo electrónico del usuario administrativo. Esta dirección se utiliza para enviar notificaciones de restablecimiento de contraseña. |
| `ADMIN_PASSWORD` | Contraseña del propietario de la licencia | Contraseña del usuario administrativo. Cuando se crea el proyecto, se genera una contraseña aleatoria y se envía un correo electrónico al propietario de la licencia. Durante la creación del proyecto, el propietario de la licencia ya debería haber cambiado la contraseña. Póngase en contacto con el propietario de la licencia para obtener la contraseña actualizada. |
| `ADMIN_LOCALE` | `en_US` | La configuración regional predeterminada utilizada por el administrador. |

## URL de administrador

Utilice la siguiente variable de entorno para proteger el acceso a la IU del administrador. Si se especifica, este valor anula la dirección URL predeterminada durante la instalación. En [!DNL Adobe Commerce] en la infraestructura de nube, debe establecer o cambiar la dirección URL del administrador mediante la variable `ADMIN_URL` en ([!DNL Cloud Console] o [!DNL Cloud CLI]). Modificar la configuración de [!DNL Admin] solo es aplicable a instalaciones locales.

`ADMIN_URL`: la dirección URL relativa para acceder a la IU de administración. La dirección URL predeterminada es `/admin`.

### Cambio de la URL de administración

De manera predeterminada, la dirección URL de [Commerce Admin](https://experienceleague.adobe.com/docs/commerce-admin/start/admin/admin.html) está establecida en *&lt;domain_name>/admin*. Por motivos de seguridad, Adobe recomienda cambiarla a una URL de administrador única y personalizada que no sea fácil de adivinar.

**En [!DNL Adobe Commerce] en la infraestructura en la nube**, debe cambiar la dirección URL del administrador usando la variable de entorno `ADMIN_URL` en ([!DNL Cloud Console] o [!DNL Cloud CLI]). Modificar la configuración de [!DNL Admin] solo es aplicable a instalaciones locales. Para las instalaciones locales, siga [usar una URL de administrador personalizada](https://experienceleague.adobe.com/docs/commerce-admin/stores-sales/site-store/store-urls.html#use-a-custom-admin-url).

Adobe recomienda cambiar la variable de nivel de entorno para la URL de administración después de la instalación. Configure esta opción por motivos de seguridad antes de ramificarse desde el entorno `master` clonado. Todas las ramas creadas a partir de la rama `master` heredan las variables de nivel de entorno y sus valores a menos que establezca la herencia en False.

Use [!DNL Cloud Console] o [!DNL Cloud CLI] para establecer o actualizar `ADMIN_URL`.

#### Opción A: cambie la dirección URL del administrador usando [!DNL Cloud Console]

##### Entorno de integración

En [Cloud Console](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/project/overview.html), agregue una nueva variable con:

- **Nombre:** `ADMIN_URL`
- **Valor:** Su nueva URL de administrador (por ejemplo, `magento_A8v10`)

- Para ver los pasos detallados, consulte [agregar variables de entorno](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/project/overview.html#configure-environment) o [variables de entorno](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-admin.html) en nuestra documentación para desarrolladores.

##### Establecer la dirección URL del administrador en [!DNL Cloud Console]

1. Inicie sesión en [Cloud Console](https://console.adobecommerce.com/).
2. Seleccione un proyecto de la lista **[!UICONTROL All projects]**.
3. En la descripción general del proyecto, seleccione el entorno y haga clic en el icono de configuración.
4. Seleccione la ficha **[!UICONTROL Variables]**.
5. Haga clic en **[!UICONTROL Create Variable]** (o edite la variable `ADMIN_URL` existente si está presente).
6. Introduzca lo siguiente:
   - **Nombre de variable:** `ADMIN_URL`
   - **Valor:** Su nueva ruta de acceso de administrador (por ejemplo, `magento_A8v10`).

   De manera predeterminada, **[!UICONTROL Available during runtime]** y **[!UICONTROL Make inheritable]** están seleccionados. Para evitar que los entornos secundarios hereden este valor, borre **[!UICONTROL Make inheritable]** de esta variable.
7. Haga clic en **[!UICONTROL Create variable]** (o **[!UICONTROL Save]**) y espere hasta que finalice la implementación. El botón solo está visible cuando los campos obligatorios contienen valores.

##### Cuando Ensayo y Producción no están disponibles en [!DNL Cloud Console]

[Envíe un ticket de asistencia](https://experienceleague.adobe.com/en/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket) solicitando agregar la variable `ADMIN_URL` para su entorno de ensayo o producción. Si se puede acceder a Ensayo y Producción desde [!DNL Cloud Console], agregue la variable tal como se describe en [Entorno de integración](#integration-environment).

#### Opción B: cambiar la dirección URL del administrador con [!DNL Cloud CLI]

Utilice el comando `magento-cloud variable:update` para actualizar la variable. (El comando `variable:set` ha quedado obsoleto y no está disponible).

El siguiente ejemplo actualiza el entorno `master` `ADMIN_URL` a `newAdmin_A8v10` e impide que los entornos secundarios hereden el valor:

```bash
magento-cloud variable:update ADMIN_URL --value newAdmin_A8v10 -e master --inheritable false
```

- **Reimplementación:** Al cambiar la variable `ADMIN_URL` en [!DNL Cloud CLI] se déclencheur una reimplementación del entorno.
- **Herencia:** Las variables se heredan de forma predeterminada. Para evitar que los entornos secundarios hereden el valor, utilice la opción `--inheritable false` como se muestra. Para obtener más información, consulte [visibilidad a nivel de variable](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/env/variable-levels.html#visibility).

>[!NOTE]
>
>El valor `ADMIN_URL` acepta letras (a-z, A-Z), números (0-9) y el carácter de subrayado (_). No se aceptan espacios u otros caracteres.
