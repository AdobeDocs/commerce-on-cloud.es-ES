---
title: Prácticas recomendadas para actualizar el proyecto
description: Consulte una lista de prácticas recomendadas para actualizar los archivos de proyecto.
feature: Cloud, Best Practices, Upgrade
exl-id: 64f92739-9170-4cbf-90ef-aab6593a37ca
source-git-commit: 31494a956babaf15320d0ffa86fcba9e845d53a1
workflow-type: tm+mt
source-wordcount: '702'
ht-degree: 0%

---

# Prácticas recomendadas para actualizar el proyecto

Siga las prácticas recomendadas para compilaciones e implementación, y use el flujo de trabajo [Actualizaciones y parches](../development/commerce-version.md) para actualizar su aplicación. Siga estas directrices para planificar el trabajo de actualización y posterior a la actualización:

- **Haga una copia de seguridad del proyecto**-Antes de actualizar Adobe Commerce y cualquier extensión de terceros o personalizada, haga una copia de seguridad de la base de datos en los entornos de integración, ensayo y producción. Consulte [Hacer una copia de seguridad de la base de datos](../development/commerce-version.md#project-backup).

- **Comprobar problemas de compatibilidad**-

   - Asegúrese de que todas las temáticas personalizadas sean compatibles con la nueva versión de Adobe Commerce

   - Después de actualizar las extensiones personalizadas y de terceros, use el comando `magento-cloud local:build` para validar las dependencias del Compositor antes de implementar y ejecute [Actualizar herramienta de compatibilidad](#use-the-upgrade-compatibility-tool) para identificar incompatibilidades de nivel de código entre las versiones actual y de destino. A continuación, utilice la [herramienta de compatibilidad de actualización](https://fluffyjaws.adobe.com/#use-the-upgrade-compatibility-tool) para identificar y priorizar las incompatibilidades de nivel de código antes de implementarla en los entornos de integración, ensayo o producción.

   - Revise las notas de la versión y la documentación de la extensión de Adobe Commerce para asegurarse de que ha implementado las soluciones alternativas o los cambios de configuración necesarios para solucionar problemas funcionales conocidos y errores relacionados con la versión y las extensiones de Adobe Commerce actualizadas.

   - Asegúrese de que las versiones del servicio instalado sean compatibles con la nueva versión de Adobe Commerce y actualice los servicios según sea necesario. Ver [Servicios](../services/services-yaml.md).

   - Pruebe la base de datos para abordar cualquier problema que puedan introducir las actualizaciones de la versión y las extensiones de Adobe Commerce.

   - Realice las actualizaciones necesarias en la configuración específica del entorno antes de implementarla en el entorno remoto.

   - Asegúrese de que la versión del servicio de búsqueda sea compatible con la versión del cliente PHP. Ver [Configurar Elasticsearch](../services/elasticsearch.md) o [Configurar OpenSearch](../services/opensearch.md).

- **Compruebe la conectividad de la base de datos y el almacenamiento disponible en entornos remotos**-

   - Utilice SSH para iniciar sesión en el servidor remoto y verificar la conexión con la base de datos MySQL. Ver [Conectarse a la base de datos](../services/mysql.md#connect-to-the-database).

   - Comprobar el almacenamiento disponible en el entorno remoto: utilice el comando `disk free` para ver y administrar el espacio en disco disponible en los entornos de la nube. Ver [Administrar espacio en disco](../storage/manage-disk-space.md).

      - Compruebe el tamaño de la base de datos actualizada y compruebe que el archivo `services.yaml` tiene suficiente espacio en disco asignado.

      - Libere espacio en disco: borre la caché y limpie los directorios `/log` y `/tmp` antes de implementar.

- **Planifique y realice una actualización correcta en los entornos local e de integración antes de implementarla en Ensayo**: después de la actualización, pruebe su implementación y resuelva cualquier problema.

- **Combine código en Ensayo y, a continuación, en Producción**: realice pruebas y resuelva cualquier problema en el entorno de ensayo antes de insertar cambios en el entorno de producción.

- **Completar tareas posteriores a la actualización**-

   - Utilice SSH para iniciar sesión en el servidor remoto y compruebe lo siguiente:

      - Compruebe el estado del indexador y vuelva a indexar según sea necesario. Consulte [Administrar los indexadores](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/manage-indexers.html?lang=es) en la _Guía de configuración_.

      - Compruebe los registros de `cron` y la tabla `cron_schedule` en la base de datos de Adobe Commerce para comprobar el estado de cron y volver a ejecutar los trabajos de cron según sea necesario.
Consulte [Registro](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/configure-cron-jobs.html?lang=es#logging) en la _Guía de configuración_.

   - Complete el UAT de prueba de aceptación de usuarios posterior a la actualización en entornos de ensayo y producción y corrija cualquier problema relacionado con las actualizaciones de extensiones personalizadas y de terceros.

## Uso de la herramienta de compatibilidad de actualización

Ejecute la herramienta de compatibilidad de actualización (UCT) como parte del análisis previo a la actualización para comprender el ámbito y el impacto de una actualización.

- UCT compara la instancia actual con una versión de Adobe Commerce de destino y devuelve una lista de problemas, errores y advertencias críticos que deben solucionarse antes de la actualización.
- Use `--coming-version (-c)` para comparar con la versión de destino planeada y `--ignore-current-version-compatibility-issues` para centrarse únicamente en los nuevos problemas introducidos por la actualización.
- Considere el informe HTML de UCT como una entrada a la lista de comprobación de actualización, junto con la compatibilidad de extensiones, versiones de servicio y comprobaciones de bases de datos.

Para obtener más información sobre la configuración y el uso, consulte:

- [Descripción general de la herramienta de compatibilidad de actualización](https://experienceleague.adobe.com/es/docs/commerce-operations/upgrade-guide/upgrade-compatibility-tool/overview)
- [Ejecutar la herramienta de compatibilidad de actualización](https://experienceleague.adobe.com/es/docs/commerce-operations/upgrade-guide/upgrade-compatibility-tool/use-upgrade-compatibility-tool/run)

Para los comerciantes de Cloud que utilizan la herramienta de análisis de todo el sitio, también puede almacenar en déclencheur UCT desde el panel y descargar el informe de HTML directamente desde el widget. Consulte Integrar la [herramienta de análisis de todo el sitio](https://experienceleague.adobe.com/es/docs/commerce-operations/upgrade-guide/upgrade-compatibility-tool/use-upgrade-compatibility-tool/integrate-analysis-tool).
