---
title: Conexiones seguras
description: Aprenda a aplicar claves SSH a su proyecto de infraestructura de Adobe Commerce en la nube e inicie sesión en entornos remotos.
role: Developer
feature: Cloud, Security
topic: Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '976'
ht-degree: 0%

---

# Conexiones seguras a entornos remotos

Secure Shell (SSH) es un protocolo común que se utiliza para iniciar sesión de forma segura en servidores y sistemas remotos. Puede utilizar SSH para acceder a sus entornos remotos para administrar la aplicación de Adobe Commerce y acceder a los registros de entornos remotos. El Adobe solo admite conexiones FTP seguras (sFTP) mediante la clave pública SSH. No se admiten conexiones FTP.

## Generar un par de claves SSH

Cree un par de claves SSH en cada equipo y espacio de trabajo que requiera acceso al código fuente y a los entornos del proyecto. La clave SSH le permite conectarse a GitHub para administrar el código fuente y conectarse a servidores en la nube sin tener que proporcionar constantemente su nombre de usuario y contraseña. Consulte [Conexión a GitHub con SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh) para obtener más instrucciones sobre cómo crear un par de claves SSH.

- La _clave pública_ es segura para acceder a un sitio, SSH y sFTP.
- La _clave privada_ sigue siendo privada en la estación de trabajo.

>[!CAUTION]
>
>**Nunca comparta su clave privada.** No lo agregue a un ticket, ni lo copie a un chat, ni lo adjunte a correos electrónicos.

## Añada una clave pública SSH a su cuenta

Después de añadir la clave pública SSH a la cuenta de Adobe Commerce en la nube, vuelva a implementar todos los entornos activos de la cuenta para instalar la clave.

Puede agregar claves SSH a su cuenta mediante uno de los siguientes métodos: CLI de nube o [!DNL Cloud Console].

>[!BEGINTABS]

>[!TAB CLI]

### Añada su clave SSH mediante la CLI de Cloud

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. Inicie sesión en el proyecto:

   ```bash
   magento-cloud login
   ```

1. Añada la clave pública.

   ```bash
   magento-cloud ssh-key:add ~/.ssh/id_rsa.pub
   ```

>[!TIP]
>
>Puede enumerar y eliminar claves SSH mediante los comandos de CLI de nube `ssh-key:list` y `ssh-key:delete`.

>[!TAB Consola]

### Añada su clave SSH usando el [!DNL Cloud Console]

**Para agregar una clave SSH a un nuevo proyecto**:

1. Inicie sesión en [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Haga clic en **[!UICONTROL No SSH key]**. Este icono se encuentra a la derecha del campo de comando y es visible cuando el proyecto no contiene una clave SSH.

1. Copie y pegue el contenido de su clave SSH pública en el campo **Clave pública**.

1. Siga las indicaciones restantes.

**Para agregar una clave SSH a su perfil de Cloud**:

1. Inicie sesión en [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. En el menú superior derecho de la cuenta, haz clic en **Mi perfil**.

1. En la vista _Claves SSH_, haga clic en **Agregar clave pública**.

1. En el formulario _Agregar una clave SSH_, dé a su clave un **Título** y pegue la clave SSH pública en el campo **Clave**.

1. Haga clic en **Guardar**.

>[!ENDTABS]

## Conectarse a un entorno remoto

Puede conectarse a un entorno remoto mediante la CLI `magento-cloud` o un comando SSH. Los comandos CLI `magento-cloud` solo se pueden usar en entornos de integración de Starter y Pro.

### Utilizar la CLI de Cloud

**Para iniciar sesión en un entorno de integración remota**:

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. Enumerar los entornos de ese proyecto.

   ```bash
   magento-cloud environment:list -p <project-ID>
   ```

1. Utilice SSH para iniciar sesión en el entorno remoto.

   ```bash
   magento-cloud ssh -p <project-ID> -e <environment-ID>
   ```

### Uso de un comando SSH

[!DNL Cloud Console] incluye una lista de comandos de acceso web y SSH para cada entorno.

**Para copiar el comando SSH**:

1. Inicie sesión en [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Seleccione un proyecto de la lista _Todos los proyectos_.

1. Seleccione un entorno.

1. Haga clic en **[!UICONTROL SSH]**.

1. En la ficha _SSH_, haga clic en el botón Copiar para copiar el comando SSH completo en el portapapeles.

1. Abra un terminal y pegue el comando SSH para crear una conexión.

   ```bash
   ssh abcdefg123abc-branch-a12b34c--mymagento@ssh.us-2.magento.cloud
   ```

>[!TIP]
>
>Para los entornos de ensayo y producción de Pro, el comando SSH puede tener el siguiente aspecto:
>
>```bash
>ssh <node>.ent-<project-ID>-<environment>-<user-ID>@ssh.<region>.magento.com
>```

## sFTP

Adobe Commerce en la infraestructura de la nube admite el acceso a sus entornos mediante sFTP (FTP seguro) con autenticación SSH. Utilice un cliente que admita la autenticación de claves SSH para sFTP y use su clave SSH pública. La clave SSH pública debe añadirse al entorno de destino. Para entornos de Starter y entornos de integración Pro, puede [agregarlo a través de [!DNL Cloud Console]](#add-your-ssh-key-using-the-project-web-interface).

_no se admiten_ conexiones sFTP de solo lectura; el acceso sFTP se proporciona con el permiso _write_ de forma predeterminada.

Al configurar sFTP, utilice la información del comando del entorno de acceso SSH: `<project-id>-<environment-id>--<app-name>@ssh<cloud-host>`

- **Nombre de usuario**: Todo el contenido anterior a `@` en su destino de acceso SSH.
- **Contraseña**: No necesita una contraseña para sFTP. El acceso sFTP utiliza la autenticación de clave SSH.
- **Host**: Todo el contenido posterior a `@` en su acceso SSH.
- **Puerto**: 22, que es el puerto SSH predeterminado.
- Clave privada **SSH**: si es necesario, proporcione la ubicación de la clave privada al cliente sFTP. De manera predeterminada, las claves privadas se almacenan en el directorio `~/.ssh`.

Según el cliente, es posible que se requieran opciones adicionales para completar la autenticación SSH para sFTP. Revise la documentación del cliente seleccionado.

Para **entornos Starter y entornos de integración Pro**, quizá también quiera considerar [agregar un `mount`](../application/properties.md#mounts) para tener acceso a un directorio específico. Agregaría el montaje a su archivo `.magento.app.yaml`. Para obtener una lista de directorios editables, vea [Estructura del proyecto](../project/file-structure.md). Este punto de montaje solo funciona en esos entornos.

Para **entornos de ensayo y producción Pro**, si no tiene acceso SSH al entorno, debe [enviar un vale de soporte de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para solicitar acceso a sFTP y un punto de montaje para acceder a la carpeta específica, por ejemplo, `pub/media`.

>[!NOTE]
>Para Ensayo y producción profesionales, si la conexión sFTP es para un usuario de _generic_ que no necesita **not** para ser [agregado al proyecto en la nube](../project/user-access.md), debe [enviar un ticket de soporte de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) con su clave **public** adjunta. **Nunca proporcione su clave SSH privada.**

## Túnel SSH

Puede utilizar el túnel SSH para conectarse a un servicio desde su entorno de desarrollo local como si el servicio fuera local. Antes de crear un túnel, configura tu [SSH](#add-an-ssh-public-key-to-your-account).

Utilice una aplicación de terminal para iniciar sesión y emitir comandos.

```bash
magento-cloud login
```

Compruebe si hay algún túnel abierto con.

```bash
magento-cloud tunnel:list
```

Para crear un túnel, debe conocer el [nombre de la aplicación](../application/properties.md#name). Puede comprobar el nombre de la aplicación mediante la CLI:

```bash
magento-cloud apps
```

### Configuración del túnel SSH

```bash
magento-cloud tunnel:open -e <environment-ID> --app <app-name>
```

Por ejemplo, para abrir un túnel a la rama `sprint5` en un proyecto con una aplicación denominada `mymagento`, escriba

```bash
magento-cloud tunnel:open -e sprint5 --app mymagento
```

Respuesta de ejemplo:

```
SSH tunnel opened on port 30004 to relationship: redis
SSH tunnel opened on port 30005 to relationship: database
Logs are written to: /home/magento_user/.magento/tunnels.log

List tunnels with: magento-cloud tunnels
View tunnel details with: magento-cloud tunnel:info
Close tunnels with: magento-cloud tunnel:close
```

**Para mostrar información acerca de su túnel**:

```bash
magento-cloud tunnel:info -e <environment-ID>
```

### Conectar a servicios

Después de establecer un túnel SSH, puede conectarse a los servicios como si se ejecutara localmente. Por ejemplo, para conectarse a la base de datos, utilice el siguiente comando:

```bash
mysql --host=127.0.0.1 --user='<database-username>' --pass='<user-password>' --database='<name>' --port='<port>'
```
