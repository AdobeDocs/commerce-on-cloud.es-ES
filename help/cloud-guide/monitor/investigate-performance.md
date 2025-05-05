---
title: Monitorización de New Relic
description: Obtenga información sobre cómo acceder a su tablero de New Relic y analizar los datos de su proyecto de Adobe Commerce en la nube.
feature: Cloud, Observability
topic: Performance
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '833'
ht-degree: 0%

---

# Monitorización de New Relic

New Relic conecta y supervisa su infraestructura y la aplicación [!DNL Commerce] mediante agentes PHP. Una vez que un entorno de Cloud se conecte a New Relic, puede iniciar sesión en su cuenta de New Relic para revisar los datos recopilados por el agente.

En la página _APM y servicios_, seleccione **Resumen** para ver información transaccional sobre su aplicación. Esta vista le ayuda a identificar posibles errores y a comprobar el estado general de su aplicación y servicios.

![Página de información general de New Relic del proyecto en la nube](../../assets/new-relic/dashboard.png)

Desde esta vista, puede rastrear transacciones que encuentran respuestas lentas o cuellos de botella, rendimiento de la aplicación, errores web y más.

Revisar datos rastreados:

- **Más tiempo**: determine el consumo de tiempo realizando un seguimiento de las solicitudes en paralelo. Por ejemplo, es posible que tenga el tiempo de transacción empleado más alto en las vistas de productos y categorías. Si una página de cuenta de cliente clasifica repentinamente el consumo de tiempo en un nivel superior, la aplicación podría verse afectada por el rendimiento de llamada o de arrastre de consultas.

- **Rendimiento más alto**: identifique las páginas que tengan el mayor impacto en función del tamaño y la frecuencia de los bytes transmitidos.

Todos los datos recopilados detallan el tiempo empleado en las acciones que transmiten datos, consultas o datos de _Redis_. Si las consultas causan problemas, New Relic proporciona información para realizar un seguimiento de estos problemas y responder a ellos.

>[!TIP]
>
>Para obtener más información sobre cómo usar estos datos para solucionar problemas de rendimiento de aplicaciones, consulte [Solucionar problemas de rendimiento con New Relic](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/troubleshoot-performance-using-new-relic-on-magento-commerce.html?lang=es) en el _Centro de ayuda de Adobe Commerce_.

## Monitorización del rendimiento con alertas administradas

El Adobe proporciona la directiva de alertas _Alertas administradas para Adobe Commerce_ para realizar un seguimiento de las métricas de rendimiento. La directiva incluye una colección de alertas que establecen umbrales y advertencias de déclencheur, así como notificaciones críticas cuando los problemas de infraestructura o de aplicaciones afectan al rendimiento del sitio. La directiva realiza el seguimiento de las siguientes métricas en los entornos de producción:

| Métrica | Recopilación de datos | Disponibilidad |
|:-------------------|:----------------|:----------------|
| Puntuación [!DNL Apdex] | APM | Pro y Starter |
| Uso de CPU | NRI | Pro |
| Espacio en disco | NRI | Pro |
| Tasa de error | APM | Pro y Starter |
| Uso de memoria | NRI | Pro |
| Carga de consulta de MariaDB | NRI | Pro |
| Memoria Redis | NRI | Pro |

Cuando la infraestructura del sitio o las condiciones de la aplicación alcanzan un umbral de déclencheur, New Relic envía notificaciones de alerta para que pueda solucionar el problema de forma proactiva. Consulte [Alertas administradas para Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-for-magento-commerce.html?lang=es) en el _Centro de ayuda de Adobe Commerce_ para obtener más información acerca de los umbrales de alerta y los pasos para solucionar problemas que desencadenaron la alerta.

>[!TIP]
>
>Para los entornos de ensayo e integración de Pro y los entornos de inicio, use [Notificaciones de estado](../integrations/health-notifications.md) para supervisar el espacio en disco.

>[!PREREQUISITES]
>
>- **Credenciales de New Relic**: credenciales para iniciar sesión en la cuenta de New Relic para su proyecto de Cloud
>- **Integración activa con New Relic**: compruebe que su entorno de nube esté conectado a New Relic
>- **Notificación de flujo de trabajo**: configure al menos un [flujo de trabajo](#set-up-a-workflow-for-notifications) para recibir las notificaciones de alerta

**Para revisar la directiva Alertas administradas para Adobe Commerce**:

1. Inicie sesión en su [cuenta de New Relic](https://login.newrelic.com/login).

1. Busque la directiva _Alertas administradas para Adobe Commerce_:

   - En el menú de navegación del Explorador, haga clic en **[!UICONTROL Alerts & AI]**.

   - En _Detectar_, haga clic en **[!UICONTROL Alert Conditions & Policies]**.

   - Compruebe que su cuenta está seleccionada en la parte superior de la vista _Condiciones y directivas de alerta_.

   - En la lista _Directiva_, seleccione **Alertas administradas para la directiva Adobe Commerce**.

     ![Directivas de alerta generadas](../../assets/new-relic/managed-alerts-policy.png)

     >[!NOTE]
     >
     >Si la directiva _Alertas administradas para Adobe Commerce_ no está disponible, consulte [Alertas administradas para Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-for-magento-commerce.html?lang=es) en el _Centro de ayuda de Adobe Commerce_.

1. Haga clic en la ficha **[!UICONTROL Alert conditions]** para revisar las condiciones de alerta definidas en la directiva.

## Crear directivas de alerta

No modifique ninguna alerta incluida en la directiva Alertas administradas para Adobe Commerce. La Adobe actualiza y mejora las condiciones de alerta de esta directiva con el tiempo, lo que sobrescribe las personalizaciones que agregue a la directiva.

En lugar de modificar una alerta existente, puede crear una directiva de alertas. A continuación, copie las condiciones de alerta en la nueva directiva.

>[!TIP]
>
>Consulte [Introducción a las alertas](https://docs.newrelic.com/docs/alerts/overview/) en la documentación de _New Relic_ para obtener información más detallada sobre las alertas, las directivas de alerta y los flujos de trabajo.

## Configuración de un flujo de trabajo para notificaciones

Ahora puede configurar un _flujo de trabajo_, anteriormente denominado canal de notificación, para recibir notificaciones sobre el rendimiento del sitio basadas en datos filtrados, como una directiva de alertas. Las notificaciones sobre problemas de rendimiento se dirigen a todos los flujos de trabajo asociados a una directiva de alerta cuando las condiciones de la aplicación o infraestructura generan una déclencheur. También recibe notificaciones cuando se reconoce y se cierra un problema.

New Relic proporciona plantillas para configurar diferentes tipos de notificaciones de flujo de trabajo, como correo electrónico, Slack, PagerDuty, webhooks y mucho más.

**Para configurar un flujo de trabajo**:

1. Inicie sesión en su [cuenta de New Relic](https://login.newrelic.com/login).

1. Cree un flujo de trabajo.

   - En el menú de navegación del Explorador, haga clic en **[!UICONTROL Alerts & AI]**.

   - En el panel de navegación izquierdo bajo _Enriquecer y notificar_, haga clic en **[!UICONTROL Workflows]**.

   - Haga clic en **[!UICONTROL Add a workflow]**, en el lado derecho.

     ![New Relic agregó un flujo de trabajo](../../assets/new-relic/add-a-workflow.png)

   - En la página _Configurar el flujo de trabajo_, escriba un nombre para el flujo de trabajo.

   - En la sección _Filtrar datos_, seleccione **[!UICONTROL Managed Alerts for Adobe Commerce]** de la lista desplegable **[!UICONTROL Policy]**.

   - En la sección _Notificar_, seleccione un canal y siga las instrucciones.

   - Haga clic en **[!UICONTROL Test workflow]** para comprobar la configuración.

1. Haga clic en **[!UICONTROL Activate workflow]**.

Consulte la documentación de New Relic sobre [Flujos de trabajo](https://docs.newrelic.com/docs/alerts-applied-intelligence/applied-intelligence/incident-workflows/incident-workflows/).

>[!WARNING]
>
>Las alertas de la directiva Alertas administradas para Adobe Commerce tienen flujos de trabajo predeterminados configurados para notificar a los equipos de Adobe que admiten Adobe Commerce en clientes de infraestructura en la nube. No modifique la configuración de estos canales predeterminados y no elimine ninguna directiva de alerta asignada a ellos.
