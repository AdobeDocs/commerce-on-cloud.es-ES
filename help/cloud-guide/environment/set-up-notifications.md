---
title: Configuración de notificaciones
description: Obtenga información sobre cómo configurar las notificaciones para Adobe Commerce en entornos de infraestructura en la nube.
feature: Cloud, Configuration, Logs
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 0%

---

# Configuración de notificaciones

De forma predeterminada, Adobe Commerce en la infraestructura de la nube escribe acciones de compilación e implementación en el archivo `app/var/log/cloud.log` dentro del directorio raíz de la aplicación de Adobe Commerce. De forma opcional, puede enviar registros a un sistema de mensajería, como Slack y correo electrónico, para recibir notificaciones en tiempo real.

Por ejemplo, podría enviar un mensaje al Slack para avisar a un grupo de personas cuando una implementación falle y solicitar que se investigue qué ha fallado.

## Notificaciones del plan

Antes de configurar las notificaciones, tenga en cuenta lo siguiente:

- ¿Qué tipo de notificaciones desea recibir (mensajes del Slack, correo electrónico, ambos)?
- ¿Cuántos detalles desea ver en los registros?
- ¿Dónde desea configurar las notificaciones (integración, ensayo, producción)?

Por ejemplo, durante el desarrollo inicial puede preferir las notificaciones por correo electrónico que muestran registros detallados sobre el entorno de integración para ayudarle a depurar los problemas antes de implementarlos en el entorno de ensayo. Cuando esté listo para implementar en el entorno de ensayo o producción, puede preferir un mensaje del Slack que contenga información menos detallada.

>[!NOTE]
>
>El archivo de configuración utilizado para configurar las notificaciones se encuentra en la raíz del directorio del proyecto, por lo que se aplica cuando se insertan cambios en cualquier entorno. Si desea personalizar las notificaciones por entorno, debe modificar el archivo de configuración antes de insertarlo en ese entorno.

## Configuración de notificaciones

Para configurar las notificaciones:

1. En la estación de trabajo local, cambie al directorio del proyecto.
1. En el archivo `.magento.env.yaml` de la raíz del proyecto, agregue la configuración del sistema de mensajería, incluyendo las notificaciones preferidas [Niveles de registro](log-handlers.md#log-levels).

   Por ejemplo, para configurar las configuraciones de correo electrónico del Slack _y_, use lo siguiente:

   ```yaml
   log:
     slack:
       token: "<your-slack-token>"
       channel: "<your-slack-channel>"
       username: "SlackHandler"
       min_level: "info"
     email:
       to: <your-email>
       from: <your-email>
       subject: "Log notification from Adobe Commerce"
       min_level: "notice"
   ```

   >[!NOTE]
   >
   >Adobe Commerce en la infraestructura en la nube solo envía correos electrónicos durante la fase de implementación.

1. Confirme y envíe los cambios al servidor remoto.

   ```bash
   git -A && git commit -m "Configure build/deploy notifications"
   ```

   ```bash
   git push origin <branch-name>
   ```

### Ejemplo de configuración del Slack

El siguiente ejemplo muestra una configuración solo de Slack:

```yaml
log:
  slack:
    token: "<your-slack-token>"
    channel: "<your-slack-channel>"
    username: "SlackHandler"
    min_level: "info"
```

- `token`: su Slack [token de usuario](https://api.slack.com/docs/token-types#user). El token de usuario autoriza a Adobe Commerce en la infraestructura de la nube a enviar mensajes.
- `channel`: nombre del canal de Slack que Adobe Commerce envía notificaciones en la infraestructura de la nube.
- `username`: nombre de usuario que utiliza Adobe Commerce en la infraestructura de la nube para enviar mensajes de notificación en Slack.
- `min_level`: nivel mínimo de registro para los mensajes de notificación. Se recomienda usar `info`.

### Ejemplo de configuración de correo electrónico

El siguiente ejemplo muestra una configuración de solo correo electrónico:

>[!NOTE]
>
>Adobe Commerce en la infraestructura en la nube solo envía correos electrónicos durante la fase de implementación.

```yaml
log:
  email:
    to: <your-email>
    from: <your-email>
    subject: "Log notification from Adobe Commerce"
    min_level: "notice"
```

- `to`: la dirección de correo electrónico Adobe Commerce en la infraestructura en la nube envía mensajes de notificación.
- `from`: dirección de correo electrónico para enviar mensajes de notificación a los destinatarios.
- `subject`: descripción del correo electrónico.
- `min_level`: nivel mínimo de registro para los mensajes de notificación. Se recomienda usar `notice` o `warning`.
