---
title: Ingesta de datos
description: Obtenga información sobre cómo ver y administrar la ingesta de datos de Commerce en New Relic.
feature: Cloud, Observability
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 0%

---

# Ingesta de datos

New Relic depende de los datos enriquecidos para proporcionar una monitorización y un análisis eficaces, pero los conjuntos de datos grandes pueden afectar a los resultados, el rendimiento y el cumplimiento normativo a tiempo. En este tema se proporcionan algunas directrices sobre la administración de la ingesta de datos y las estrategias para refinar los datos de modo que sean más eficaces.

New Relic proporciona una vista de _administración de datos_ que resume el uso del plan por fuente de datos.

**Para ver sus fuentes y datos de ingesta**:

1. En el menú de usuario de New Relic, haga clic en **[!UICONTROL Manage your data]**.
1. Haga clic en **[!UICONTROL Data management]** en la lista _Administración_.

   ![Administración de datos](../../assets/new-relic/data-ingestion.png)

   La pestaña **[!UICONTROL Data ingestion]** muestra los datos ingeridos para el día y el origen de los datos.
La pestaña de retención de datos muestra y controla cuánto tiempo se almacenan los datos.

1. Seleccione la ficha **[!UICONTROL Limits]** y vea los límites de su cuenta.

Las fuentes de datos de Adobe Commerce incluyen:

- **Eventos de APM**: datos de evento utilizados en gráficos y paneles
- **Infraestructura**: métricas de procesos y hosts, como CPU, almacenamiento y redes
- **Registro**: Registros para CDN, APM y servidor de aplicaciones

Los datos de registro contribuyen a una gran parte de la ingesta. Vea cómo [Ver y analizar los datos de registro](log-management.md#view-and-analyze-log-data) y trabajar con su representante de Adobe para formar una estrategia para la ingesta de datos y las necesidades de retención. Obtenga más información acerca de [administrar la ingesta de datos](https://docs.newrelic.com/docs/data-apis/manage-data/manage-data-coming-new-relic/) en la _documentación de New Relic_.
