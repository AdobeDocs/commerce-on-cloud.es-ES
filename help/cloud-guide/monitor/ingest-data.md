---
title: Ingesta de datos
description: Obtenga información sobre cómo ver y administrar la ingesta de datos de Commerce en New Relic.
feature: Cloud, Observability
exl-id: b457b4de-deeb-4e92-b95a-c2b89d6f7a05
TQID: https://experienceleague.adobe.com/60hhI0IvazUSrw6cCRoXfbGjA4fkLLPXb8Q4Q08AqeE
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: ebde5b41-29c9-4f5e-9ef6-1197e85409e3id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 209
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
