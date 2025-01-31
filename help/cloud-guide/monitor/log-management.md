---
title: Administración de registros de New Relic
description: Aprenda a utilizar el registro de New Relic
feature: Cloud, Logs, Observability
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '343'
ht-degree: 0%

---

# Administración de registros de New Relic

Todos los proyectos de infraestructura en la nube incluyen [administración de registros de New Relic](https://docs.newrelic.com/docs/logs/get-started/get-started-log-management/). El servicio está preconfigurado para agregar todos los datos de registro de los entornos de ensayo y producción y mostrarlos en un panel de administración de registros centralizado.

Los datos agregados incluyen información de los siguientes registros:

- Todos `ece-tools` y los registros de aplicación del directorio `~/var/log`
- Registra los servicios en la nube del directorio `var/log/platform/<project-ID>`
- Fastly CDN y WAF

Cuando el proyecto esté conectado a New Relic, puede utilizar el servicio Registros de New Relic para completar tareas como las siguientes:

- Usar consultas de New Relic para buscar datos de registro agregados
- Visualizar datos de registro mediante la aplicación New Relic Logs
- Creación de gráficos, tableros y alertas personalizados
- Solucionar problemas de rendimiento desde un solo panel

## Visualización y análisis de datos de registro

Utilice la aplicación New Relic Logs para buscar en los datos de registro agregados y solucionar errores de aplicación, infraestructura, CDN y WAF. Puede crear gráficos, tableros y alertas mediante los datos de registro recopilados de los servicios de infraestructura y APM de New Relic.

**Para usar la aplicación New Relic Logs**:

1. Inicie sesión en su [cuenta de New Relic](https://login.newrelic.com/login).

1. Seleccione **Registros** en el menú de navegación del Explorador.

1. Compruebe que su cuenta está seleccionada en la parte superior de la vista _Todos los registros_.

1. Seleccione un intervalo de tiempo para la consulta Registros.

1. Para revisar los datos de registro de infraestructura de los servicios en la nube (registros de `~/var/log/`), escriba la cadena de consulta `has: "filePath"` en el campo _Buscar registros_. A continuación, haga clic en **[!UICONTROL Query logs]**.

   Los nombres de los archivos de registro se almacenan en la columna `filePath`, con rutas de acceso completas al archivo de registro.

   ![Datos de registro del servicio New Relic del proyecto en la nube](../../assets/new-relic/var-log-query.png)

1. Para revisar los datos de registro rápido, escriba la cadena de consulta `has: "client_ip"` en el campo _Buscar registros_. A continuación, haga clic en **[!UICONTROL Query logs]**.

1. Para filtrar los resultados de registro rápido por código de país, haga clic en **[!UICONTROL Add column]** y luego seleccione **[!UICONTROL geo_country_code]**.

   ![Filtro de atributo de registro de CDN de New Relic del proyecto en la nube](../../assets/new-relic/fastly-countrycode-filter.png)

>[!TIP]
>
>Puede guardar la vista de consulta desde el menú desplegable _Vistas guardadas_. Haga clic en **[!UICONTROL Create new]**, proporcione un nombre, seleccione opciones y haga clic en **[!UICONTROL Save view]**.
>
>Consulte [Introducción a la administración de registros](https://docs.newrelic.com/docs/logs/get-started/get-started-log-management/) y [Introducción al lenguaje de consulta New Relic](https://docs.newrelic.com/docs/query-your-data/nrql-new-relic-query-language/get-started/introduction-nrql-new-relics-query-language/) en el sitio _Documentos de New Relic_.
