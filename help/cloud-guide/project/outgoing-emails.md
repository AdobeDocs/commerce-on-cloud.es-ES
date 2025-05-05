---
title: Configuración de correos electrónicos salientes
description: Obtenga información sobre cómo habilitar los correos electrónicos salientes para Adobe Commerce en la infraestructura en la nube.
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '384'
ht-degree: 0%

---

# Configuración de correos electrónicos salientes

Puede habilitar y deshabilitar los correos electrónicos salientes para los entornos de integración (y ensayo solo para Starter) desde [!DNL Cloud Console] o desde la línea de comandos. Habilite los correos electrónicos salientes para enviar correos electrónicos de autenticación de doble factor o restablecer la contraseña para los usuarios del proyecto en la nube.

De forma predeterminada, los correos electrónicos salientes están habilitados en los entornos Producción y Ensayo (solo Pro). Sin embargo, la configuración de **[!UICONTROL Enable outgoing emails]** puede aparecer deshabilitada en la configuración del entorno independientemente del estado hasta que establezca la propiedad de `enable_smtp` a través de [la línea de comandos](#enable-emails-in-the-cli) o [la consola de Cloud](outgoing-emails.md#enable-emails-in-the-cloud-console).

Al actualizar el valor de la propiedad `enable_smtp` por [línea de comandos](#enable-emails-in-the-cli) también se cambia el valor de configuración [!UICONTROL Enable outgoing emails] para este entorno en la consola de Cloud.

>[!NOTE]
>
>Al habilitar o deshabilitar la configuración **[!UICONTROL Enable outgoing emails]**, no se habilitarán o deshabilitarán los mensajes de correo electrónico en los entornos de ensayo o producción profesional.

{{redeploy-warning}}

## Habilitar correos electrónicos en la consola de Cloud

Use la opción **[!UICONTROL Outgoing emails]** en la vista _Configurar entorno_ para habilitar o deshabilitar la compatibilidad con el correo electrónico.

Si los correos electrónicos salientes deben deshabilitarse o volver a habilitarse en entornos de ensayo o producción profesional, puede enviar un [ticket de asistencia de Adobe Commerce](https://experienceleague.adobe.com/es/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide).

>[!TIP]
>
>Es posible que el estado del correo electrónico saliente no se refleje en los entornos de ensayo o producción de Pro en Cloud Console.

**Para administrar la compatibilidad con el correo electrónico de[!DNL Cloud Console]**:

1. Inicie sesión en [[!DNL Cloud Console]](https://console.adobecommerce.com).
1. Seleccione un proyecto de la lista _Todos los proyectos_.
1. En el panel Proyecto, haga clic en el icono de configuración en la esquina superior derecha.
1. Haga clic en **[!UICONTROL Environments]** y seleccione un entorno específico en la lista (excepto Ensayo y Producción para Pro).
1. Para habilitar o deshabilitar los mensajes de correo electrónico salientes, alterne _Habilitar los mensajes de correo electrónico salientes_ **Activado** o **Desactivado**.

   ![Habilitar configuración de correo electrónico saliente](../../assets/outgoing-emails.png)

Después de cambiar la configuración, el entorno se genera e implementa con la nueva configuración.

## Habilitar correos electrónicos en la CLI

Puede cambiar la configuración de correo electrónico de un entorno activo mediante el comando `magento-cloud` CLI `environment:info` para establecer la propiedad `enable_smtp`. Al habilitar SMTP, se actualiza la variable de entorno `MAGENTO_CLOUD_SMTP_HOST` con la dirección IP del host SMTP para enviar correo.

**Para administrar la compatibilidad con el correo electrónico desde la línea de comandos**:

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. Compruebe la configuración del correo electrónico saliente para el entorno.

   ```bash
   magento-cloud environment:info -e <environment-id> | grep enable_smtp
   ```

1. Cambie la configuración de soporte de correo electrónico estableciendo la variable de entorno `enable_smtp` en `true` o `false`.

   ```bash
   magento-cloud environment:info --refresh -e <environment-id> enable_smtp true
   ```

   Espere a que el entorno se cree e implemente.

1. Utilice un SSH para iniciar sesión en el entorno remoto.

1. Compruebe que el correo electrónico funciona; envíe un correo electrónico de prueba a una dirección que pueda comprobar.

   ```bash
   php -r 'mail("mail@example.com", "test message", "just testing", "From: tester@example.com");'
   ```

1. Compruebe que SendGrid recoge el correo electrónico.

   ```bash
   grep mail@example.com /var/log/mail.log
   ```
