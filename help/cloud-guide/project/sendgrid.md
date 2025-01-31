---
title: Servicio de correo electrónico SendGrid
description: Obtenga información acerca del servicio de correo electrónico SendGrid para Adobe Commerce en la infraestructura en la nube y cómo puede probar la configuración de DNS.
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1317'
ht-degree: 0%

---

# Servicio de correo electrónico SendGrid

El servicio proxy del Protocolo simple de transferencia de correo (SMTP) de SendGrid proporciona servicios de autenticación de correo electrónico saliente y de supervisión de reputación, incluida la compatibilidad con:

* Todos los correos electrónicos transaccionales salientes
* Direcciones IP dedicadas
* Registro de dominios, firmas de DomainKeys Identified Mail (DKIM) para la validación de dominios de correo electrónico (solo para entornos de ensayo y producción Pro)
* Registro de dominio personalizado (solo para Pro)
* Integración automatizada para entornos de integración de Starter y Pro. Los entornos de producción y ensayo de Pro requieren un aprovisionamiento y una configuración manuales durante el proceso de aprovisionamiento de hardware de Infrastructure as a Service (IaaS)

El proxy SMTP SendGrid no está diseñado para utilizarse como servidor de correo electrónico de uso general para recibir correo electrónico entrante o para utilizarse con campañas de marketing por correo electrónico.

>[!TIP]
>
>Asegúrese de haber configurado las direcciones de correo electrónico de tienda adecuadas en la Administración de; para ello, vaya a Tiendas > Configuración > General para evitar problemas con la capacidad de entrega y la verificación del dominio. Debe desactivar **[!UICONTROL Use Default]** y reemplazar los valores predeterminados por un dominio de su propiedad. Los servicios de correo electrónico públicos/de dominio compartido, como gmail.com y outlook.com, no deben configurarse como la dirección de correo electrónico del remitente al enviar correos electrónicos a través de Sendgrid.

## Habilitar o deshabilitar correo electrónico

Puede habilitar o deshabilitar los correos electrónicos salientes para cada entorno desde la consola de Cloud o desde la línea de comandos.

De forma predeterminada, los correos electrónicos salientes están habilitados en los entornos de Pro Production y Staging. Sin embargo, [!UICONTROL Outgoing emails] puede aparecer deshabilitado en la configuración del entorno hasta que establezca la propiedad `enable_smtp` a través de la [línea de comandos](outgoing-emails.md#enable-emails-in-the-cli) o la [consola de nube](outgoing-emails.md#enable-emails-in-the-cloud-console). Puede habilitar correos electrónicos salientes para los entornos de integración y ensayo para enviar correos electrónicos de autenticación de doble factor o restablecer la contraseña para los usuarios del proyecto en la nube. Ver [Configurar correos electrónicos para pruebas](outgoing-emails.md).

Si los correos electrónicos salientes deben deshabilitarse o volver a habilitarse en entornos de ensayo o producción profesional, puede enviar un [ticket de asistencia de Adobe Commerce](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide).

>[!TIP]
>
>Al actualizar el valor de la propiedad [!UICONTROL enable_smtp] por [línea de comandos](outgoing-emails.md#enable-emails-in-the-cli) también se cambia el valor de configuración [!UICONTROL Enable outgoing emails] para este entorno en [Cloud Console](outgoing-emails.md#enable-emails-in-the-cloud-console).

## Panel SendGrid

Todos los proyectos de Cloud se administran en una cuenta central, por lo que solo el equipo de asistencia tiene acceso al panel SendGrid. SendGrid no proporciona características de restricción de subcuenta.

Para revisar los registros de actividad para el estado de entrega o una lista de direcciones de correo electrónico rechazadas, bloqueadas o rechazadas, [envíe un ticket de asistencia de Adobe Commerce](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket). El equipo de soporte técnico **no puede** recuperar los registros de actividad con más de 30 días.

Si es posible, incluya la siguiente información en su solicitud:

* las direcciones de correo electrónico afectadas
* el periodo en cuestión (solo en los últimos 30 días)
* asunto del correo electrónico

## Correo identificado por DomainKeys (DKIM)

DKIM es una tecnología de autenticación de correo electrónico que permite a los proveedores de servicios de Internet (ISP) identificar direcciones de remitente legítimas y falsas, una técnica que se utiliza comúnmente en las estafas por correo electrónico y phishing. DKIM depende de un propietario del dominio que administra los registros DNS. Al utilizar DKIM, el servidor remitente utiliza una clave privada para firmar los mensajes. Además, el propietario del dominio agrega un registro DKIM, que es un registro `TXT` modificado, a los registros DNS del dominio del remitente. Este registro `TXT` contiene una clave pública que los servidores de correo de los destinatarios utilizan para comprobar la firma de un mensaje. El procedimiento de criptografía de clave pública de DKIM permite a los destinatarios verificar la autenticidad de un remitente. Ver [Explicación de los registros de DKIM](https://docs.sendgrid.com/ui/account-and-settings/dkim-records).

>[!WARNING]
>
>Las firmas de DKIM de SendGrid y la compatibilidad con la autenticación de dominios solo están disponibles en los entornos Producción y Ensayo para proyectos Pro, pero no para todos los entornos Starter. Como resultado, es probable que los correos electrónicos transaccionales salientes estén marcados por filtros de correo no deseado. El uso de DKIM mejora la tasa de entrega como remitente de correo electrónico autenticado. Para mejorar la tasa de entrega de mensajes, puede actualizar de Starter a Pro o utilizar su propio servidor SMTP o proveedor de servicios de entrega de correo electrónico. Consulte [Configurar conexiones de correo electrónico](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/communications/email-communications) en la _guía de sistemas de administración_.

### Autenticación de remitente y dominio

Para que SendGrid envíe correos electrónicos transaccionales en su nombre desde entornos de Pro Production o Staging, debe configurar la configuración de DNS para incluir las tres entradas DNS del subdominio SendGrid. A cada cuenta de SendGrid se le asigna un registro `TXT` único que se usa para autenticar correos electrónicos salientes.

>[!TIP]
>
>Asegúrese de configurar **[!UICONTROLSalmacenar direcciones de correo electrónico]** con el dominio correcto en **[!UICONTROL Stores > Configuration > General > Store Email Addresses]**. La autenticación de dominio se realiza en la dirección de correo electrónico del remitente. Si se configura la configuración predeterminada (`example.com`), Sendgrid bloquearía los mensajes de correo electrónico de `example.com`.

**Para habilitar la autenticación de dominio**:

1. Envíe un [ticket de asistencia](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket) para solicitar la habilitación de DKIM para un dominio específico (**solo entornos de ensayo y producción Pro**).
1. Actualice la configuración de DNS con los registros `TXT` y `CNAME` que se le proporcionaron en el vale de soporte.

**Ejemplo `TXT` registro con ID de cuenta**:

```text
v=spf1 include:u17504801.wl.sendgrid.net -all
```

**Ejemplo `CNAME` registros**:

| Dominio | Apunta A | Tipo de registro |
| ---------- | ---------- | ------------- |
| em.emaildomain.com | uxxxxxx.wl.sendgrid.net | CNAME |
| s1._domainkey.emaildomain.com | s1.domainkey.uxxxxxx.wl.sendgrid.net | CNAME |
| s2._domainkey.emaildomain.com | s2.domainkey.uxxxxxx.wl.sendgrid.net | CNAME |

### Firmas de DKIM y seguridad automatizada

Puede seleccionar entre la seguridad automática y la manual al configurar un dominio autenticado. Si elige la seguridad automatizada, SendGrid administra los registros de DKIM y SPF automáticamente. Cuando agrega una nueva dirección IP de envío dedicada a su cuenta, SendGrid actualiza inmediatamente la configuración de DNS y la firma de DKIM. Si desactiva la seguridad automatizada, usted es el responsable de actualizar su firma de DKIM cada vez que cambie su dominio de envío.

**Ejemplo de seguridad automatizada habilitada**:

```text
subdomain.mydomain.com. | CNAME | uxxxxxx.wl.sendgrid.net
s1._domainkey.mydomain.com. | CNAME | s1.domainkey.uxxxxxx.wl.sendgrid.net
s2._domainkey.mydomain.com. | CNAME | s2.domainkey.uxxxxxx.wl.sendgrid.net
```

**Ejemplo de seguridad automatizada deshabilitada**:

```text
me12345.mydomain.com | MX | mx.sendgrid.net
me12345.mydomain.com | TXT | v=spf1 include:sendgrid.net ~all
m1._mydomain.com | TXT | k=rsa; t=s; p=<public-key>
```

Una vez configurada la autenticación de dominios, SendGrid administra automáticamente los registros del Marco de directivas de seguridad (SPF) y de DKIM. Una vez que SendGrid proporciona los registros `CNAME` para agregarlos a los registros DNS, puede agregar direcciones IP dedicadas y realizar otras actualizaciones de la cuenta sin tener que administrar manualmente los registros SPF. Consulte [Seguridad automatizada y su firma de DKIM](https://docs.sendgrid.com/ui/account-and-settings/dkim-records#automated-security-and-your-dkim-signature).

Para probar la configuración de DNS:

```
dig CNAME em.domain_name
dig CNAME s1._domainkey.domain_name
dig CNAME s2._domainkey.domain_name
```

## Umbral de correo electrónico transaccional

El umbral de correo electrónico transaccional se refiere al número de mensajes de correo electrónico transaccionales que puede enviar desde entornos Pro en un período de tiempo específico, como 12 000 correos electrónicos al mes desde entornos que no son de producción. El umbral está diseñado para proteger contra el envío de correo no deseado y potencialmente dañar su reputación de correo electrónico.

No existen límites estrictos en la cantidad de correos electrónicos que se pueden enviar en el entorno de producción, siempre y cuando la puntuación de reputación del remitente sea superior al 95 %. La reputación se ve afectada por el número de correos electrónicos rechazados o rechazados y por si los registros de correo no deseado basados en DNS han marcado su dominio como una posible fuente de correo no deseado. Ver [Correos electrónicos no enviados cuando se excedieron los créditos de SendGrid en Adobe Commerce](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/emails-not-being-sent-sendgrid-credits-exceeded) en la _Base de conocimiento de soporte de Commerce_.

**Para comprobar si se han superado los créditos máximos**:

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. Utilice SSH para iniciar sesión en el entorno remoto.

   ```bash
   magento-cloud ssh
   ```

1. Compruebe `/var/log/mail.log` para ver `authentication failed : Maxium credits exceeded` entradas.

   Si ve `authentication failed` entradas de registro y la reputación de **envío de correo electrónico** es de un mínimo de 95, puede [enviar un ticket de soporte de Adobe Commerce](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket) para solicitar un aumento de la asignación de crédito.

### Reputación de envío de correo electrónico

Una reputación de envío de correo electrónico es una puntuación asignada por un proveedor de servicios de Internet (ISP) a una compañía que envía mensajes de correo electrónico. Cuanto más alta sea la puntuación, más probable es que un ISP envíe mensajes a la bandeja de entrada de un destinatario. Si la puntuación cae por debajo de un determinado nivel, el ISP puede enrutar los mensajes a la carpeta de correo no deseado de los destinatarios o incluso rechazarlos por completo. La puntuación de reputación está determinada por varios factores, como un promedio de 30 días de clasificación de sus direcciones IP frente a otras direcciones IP y la tasa de quejas de spam. Ver [8 formas de comprobar tu reputación de envío de correo electrónico](https://sendgrid.com/en-us/blog/5-ways-check-sending-reputation).

### Listas de supresión de correo electrónico

Una lista de supresión de correo electrónico es una lista de destinatarios a los que no se deben enviar correos electrónicos si ello pudiera dañar su reputación de envío y las tasas de envío. La ley CAN-SPAM lo requiere para garantizar que los remitentes de correo electrónico tengan un método de exclusión de los destinatarios que cancelaron la suscripción o marcaron el correo electrónico como correo no deseado. La lista de supresión también recopila correos electrónicos que rebotan, están bloqueados o no son válidos.

Para evitar que los mensajes de correo electrónico se envíen a la carpeta de correo no deseado, en primer lugar, siga el artículo de prácticas recomendadas de Sendgrid, [¿Por qué mis correos electrónicos van a ser correo no deseado?](https://sendgrid.com/en-us/blog/10-tips-to-keep-email-out-of-the-spam-folder).

Si algunos destinatarios no reciben sus correos electrónicos, puede [Enviar un vale de soporte técnico de Adobe Commerce](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket) para solicitar una revisión de las listas de supresión y eliminar los destinatarios si es necesario.

Para obtener más información, consulte [¿Qué es una lista de supresión?](https://sendgrid.com/en-us/blog/what-is-a-suppression-list)
