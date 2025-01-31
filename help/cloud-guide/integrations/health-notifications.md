---
title: Notificaciones de estado
description: Obtenga información sobre cómo configurar las notificaciones de Slack, correo electrónico y PagerDuty para el uso del espacio en disco en su proyecto de Adobe Commerce en la nube.
feature: Cloud, Observability, Integration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '312'
ht-degree: 0%

---

# Notificaciones de estado

Adobe Commerce en la infraestructura en la nube supervisa el uso del espacio en disco en todas las aplicaciones y servicios del entorno de inicio o del entorno de integración Pro. Un disco de base de datos que se quede sin espacio podría dañar los datos. La comprobación del estado se realiza cada 5 minutos y puede enviarle notificaciones por correo electrónico u otro servicio externo. Hay tres advertencias de disco bajo para las notificaciones de mantenimiento:

- **Advertencia**: el espacio disponible en disco cae por debajo del 20%
- **Crítico**: el espacio disponible en disco cae por debajo del 10%
- **Borrar todo**: el espacio disponible en disco devuelve más del 20% tras un evento de disco bajo

>[!NOTE]
>
>En entornos de Pro Production, puede utilizar la directiva Alertas administradas para alertas de Adobe Commerce para que New Relic supervise el espacio en disco. Ver [Supervisar el rendimiento con alertas administradas](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts).

## Notificaciones por correo electrónico

La integración del correo electrónico de mantenimiento requiere una dirección de origen y al menos una dirección de destinatario. Puede usar la misma dirección de correo electrónico para `from-address` y para `recipients`. El siguiente ejemplo registra una integración de correo electrónico de mantenimiento con dos destinatarios:

```bash
magento-cloud integration:add --type health.email --from-address you@example.com --recipients them@example.com --recipients others@example.com
```

## Notificaciones del canal del Slack

Slack es un servicio externo que utiliza aplicaciones interactivas denominadas bots para publicar mensajes en una sala de chat. Para poder recibir notificaciones de mantenimiento en Slack, debe crear un [usuario de bots](https://api.slack.com/bot-users) personalizado para el grupo de Slack. Después de configurar el usuario del bot para el canal o los canales, guarde el [token de bot](https://api.slack.com/docs/token-types#bot) proporcionado por el Slack para registrar la integración. El siguiente ejemplo registra las notificaciones de mantenimiento en un canal de Slack:

```bash
magento-cloud integration:add --type health.slack --token SLACK_BOT_TOKEN --channel '#slack-channel-name'
```

## Notificaciones de PagerDuty

PagerDuty es un servicio externo que puede notificar a los miembros del equipo de atención continuada problemas importantes. Para poder recibir notificaciones de mantenimiento en PagerDuty, debe crear una [integración de PagerDuty](https://developer.pagerduty.com/v2/docs/integrating) que use la versión 2 de la API de eventos. Use la clave de integración o _clave de enrutamiento_ para registrar la integración. El ejemplo siguiente registra las notificaciones para PagerDuty mediante una clave de enrutamiento:

```bash
magento-cloud integration:add --type health.pagerduty --routing-key PAGERDUTY_ROUTING_KEY
```
