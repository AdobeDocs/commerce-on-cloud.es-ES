---
title: Flujo de actividad
description: Aprenda a leer el flujo de actividad en  [!DNL Cloud Console]  o la CLI de la nube para Adobe Commerce en la infraestructura en la nube.
last-substantial-update: 2024-02-06T00:00:00Z
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '449'
ht-degree: 0%

---

# Flujo de actividad

La vista principal de cada entorno muestra una lista de eventos históricos de **Actividad** similar a un registro Git. La lista Actividad es un flujo de los eventos recientes para entornos activos. A continuación se muestra una lista de los tipos de actividades y sus iconos que se muestran en el flujo de actividades:

![Tipos de actividades](../../assets/activity-types.svg){width="500" align="center"}

## Ver registros

En la lista Actividad, haga clic en el icono de estado de una actividad para ver el &quot;log&quot;. También puede hacer clic en el menú ![Más](../../assets/icon-more.png){width="32"} (_más_) para obtener acceso a más opciones y administrar la actividad. A continuación se muestra un breve registro que crea una copia de seguridad. Puede [usar la CLI de nube](#activity-stream-with-cloud-cli) para ver el mismo registro.

![Vista de registro](../../assets/log-view.png)

## Administración de una actividad

Algunas actividades están en estado _ejecutándose_ o _pendiente_. Puede actuar en una actividad en ejecución, como cancelar una implementación en ejecución. Las fichas siguientes muestran dos métodos para cancelar una actividad: la [!DNL Cloud Console] o la CLI de nube.

>[!BEGINTABS]

>[!TAB Consola]

**Para cancelar una actividad en[!DNL Cloud Console]**:

Puede actuar en una actividad en ejecución si accede al menú ![Más](../../assets/icon-more.png){width="32"} (_más_) y selecciona una acción, como `Cancel` o `View log`. Para este ejemplo, seleccione la opción **Cancel** para detener la actividad en ejecución.

No todas las actividades tienen la opción de cancelación. Por ejemplo, la opción para cancelar la implementación de la aplicación solo aparece durante la fase _build_. Una vez que la aplicación se haya movido a la fase _deploy_, ya no podrá cancelar la actividad. Ver [Proceso de implementación](../deploy/process.md) acerca de las diferentes fases.

![Cancelar actividad](../../assets/activity-icons/cancel-activity.png){width="450" align="center"}

Si tiene un terminal ejecutando la actividad de implementación, cancelar en el [!DNL Cloud Console] resulta en la cancelación en el terminal:

![Actividad cancelada en terminal](../../assets/activity-icons/activity-cancelled.png){width="300"}

>[!TAB CLI]

**Para cancelar una actividad en la CLI de la nube**:

1. Identifique las actividades en ejecución y seleccione un ID de actividad.

   ```bash
   magento-cloud activity:list --state=in_progress
   ```

1. Cancele la actividad con el ID de actividad:

   ```bash
   magento-cloud activity:cancel wvl5wm7s5vkhy
   ```

>[!ENDTABS]

## Filtrar flujo de actividad

La capacidad de filtrar la lista de actividades es útil cuando se busca algo específico, como una copia de seguridad o un evento de combinación.

**Para filtrar la lista de actividades en[!DNL Cloud Console]**:

1. Seleccione un entorno y elija la vista de la actividad **[!UICONTROL All]** para incluir el historial de eventos completo.

1. Haga clic en ![Filtrar por](../../assets/icon-filterby.png){width="32"} y seleccione las **[!UICONTROL Filter by]** opciones:

   ![Filtrar actividades](../../assets/activity-filter.png)

1. Elija la vista de la actividad **[!UICONTROL Recent]** y restablezca la lista.

## Ver flujo con CLI de nube

La CLI `magento-cloud` proporciona la mayoría de las mismas capacidades que [!DNL Cloud Console]. El comando `activity` puede:

- `list` el flujo de actividades para un entorno
- `get` detalles sobre una actividad específica
- mostrar `log` para una actividad específica
- `cancel` una actividad

**Para ver el flujo de actividad con la CLI de nube**:

1. Enumerar las actividades para el entorno actual.

   ```bash
   magento-cloud activity:list
   ```

1. Cada actividad tiene un ID único. Seleccione un ID de la lista anterior y vea los detalles de esa actividad.

   ```bash
   magento-cloud activity:get wvl5wm7s5vkhy
   ```

1. Vea el registro completo de esa actividad.

   ```bash
   magento-cloud activity:log wvl5wm7s5vkhy
   ```

   Respuesta de ejemplo:

   ```bash
   Activity ID: wvl5wm7s5vkhy
   Type: environment.backup
   Description: User created a backup of Master
   Created: 2023-09-08T14:03:33+00:00
   State: complete
   Log:
   Creating backup of master
   Created backup eg5pu63egt2dcojkljalzjdopa
   ```
