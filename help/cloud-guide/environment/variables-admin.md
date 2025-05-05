---
title: Variables de ADMINISTRACIÓN
description: Consulte una lista de variables de entorno utilizadas al instalar Adobe Commerce en la infraestructura en la nube.
feature: Cloud, Configuration, Install, Roles/Permissions
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 0%

---

# Variables de administración

Los usuarios que tienen acceso administrativo al proyecto de infraestructura de Adobe Commerce en la nube pueden utilizar las siguientes variables de entorno de proyecto para anular los ajustes de configuración de la cuenta de usuario administrativa y acceder a la IU de administración.

## Credenciales de administrador

Puede anular las credenciales de usuario de administrador durante la instalación de Commerce con las variables ADMIN de la siguiente tabla.

Si desea cambiar los valores después de la instalación, conéctese a su entorno usando SSH y use el comando [`admin:user` de la CLI de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/admin.html?lang=es) para crear o editar las credenciales del usuario administrador.

| Variable | Predeterminado | Descripción |
| -------------- | --------------------------- | ----------- |
| `ADMIN_USERNAME` | Dirección de correo electrónico del propietario de licencia | Un nombre de usuario para el usuario administrativo con la capacidad de crear otros usuarios, incluidos los usuarios administrativos. |
| `ADMIN_EMAIL` |                             | Dirección de correo electrónico del usuario administrativo. Esta dirección se utiliza para enviar notificaciones de restablecimiento de contraseña. |
| `ADMIN_PASSWORD` |                             | Contraseña del usuario administrativo. Cuando se crea el proyecto, se genera una contraseña aleatoria y se envía un correo electrónico al propietario de la licencia. Durante la creación del proyecto, el propietario de la licencia ya debería haber cambiado la contraseña. Póngase en contacto con el propietario de la licencia para obtener la contraseña actualizada. |
| `ADMIN_LOCALE` | `en_US` | La configuración regional predeterminada utilizada por el administrador. |

## URL de administrador

Utilice la siguiente variable de entorno para proteger el acceso a la IU del administrador. Si se especifica, este valor anula la dirección URL predeterminada durante la instalación.

`ADMIN_URL`: la dirección URL relativa para acceder a la IU de administración. La dirección URL predeterminada es `/admin`. Por motivos de seguridad, Adobe recomienda cambiar el valor predeterminado a una URL de administrador única y personalizada que no sea fácil de adivinar.

### Cambio de la URL de administración

El Adobe recomienda cambiar la variable de nivel de entorno para la URL de administración después de la instalación. Configure esta opción por motivos de seguridad antes de ramificarse desde el entorno `master` clonado. Todas las ramas creadas a partir de la rama `master` heredan las variables de nivel de entorno y sus valores.

Utilice el comando `magento-cloud variable:update` para actualizar el valor de la variable. (El comando `variable:set` ha quedado obsoleto y no está disponible). El siguiente ejemplo actualiza ADMIN_URL a `newAdmin_A8v10`:

```bash
magento-cloud variable:update ADMIN_URL --value newAdmin_A8v10 -e master
```

>[!NOTE]
>
>El valor `ADMIN_URL` acepta letras (a-z o A-Z), números (0-9) y el carácter de subrayado (_) para una ruta de administración personalizada. Se aceptan espacios u otros caracteres **no**.

**Para cambiar la dirección URL usando[!DNL Cloud Console]**:

1. Inicie sesión en [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Seleccione un proyecto de la lista _Todos los proyectos_.

1. En la descripción general del proyecto, seleccione el entorno y haga clic en el icono de configuración.

   ![Configuración del proyecto](../../assets/icon-configure.png){width="36"}

1. Seleccione la ficha **Variables**.

1. Haga clic en **Crear variable**.

1. Introduzca lo siguiente:

   - **Nombre de variable** = `ADMIN_URL`
   - **value** = Nueva URL. Por ejemplo, establezca la URL de administración en `magento_A8v10`.

   De manera predeterminada, `Available during runtime` y `Make inheritable` están seleccionados.

1. Haga clic en **Crear variable** y espere hasta que finalice la implementación. Este botón solo está visible cuando los campos obligatorios contienen valores.
