---
title: Escalado automático
description: Descubra cómo se puede ampliar Adobe Commerce en la infraestructura en la nube para satisfacer las demandas de recursos.
feature: Cloud, Auto Scaling
topic: Architecture
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '549'
ht-degree: 0%

---

# Escalado automático

El escalado automático añade o elimina automáticamente los recursos a la infraestructura de la nube para mantener un rendimiento óptimo y costes razonables. Actualmente, esta característica solo está disponible para proyectos configurados con una [arquitectura escalada](scaled-architecture.md).

## Nodos del servidor web

El [nivel web](scaled-architecture.md#web-tier) se escala para adaptarse al aumento en las solicitudes de procesos y a los requisitos de tráfico más altos. Actualmente, la función de escalado automático solo se escala horizontalmente añadiendo o eliminando nodos de servidor web.

Se produce un evento de escalado automático cuando el uso y el tráfico de CPU alcanzan un umbral predefinido:

- **Nodos agregados**: las CPU/núcleos en todos los nodos web activos tienen una capacidad del 75% durante 1 minuto y el tráfico aumenta un 20% durante 5 minutos consecutivos.
- **Nodos eliminados**: las CPU y los núcleos de todos los nodos web activos se cargan al 60 % durante 20 minutos. Los nodos se eliminan en el orden en que se agregaron.

Los umbrales mínimo y máximo se determinan y establecen en función de los límites de recursos contratados de cada comerciante; esto reduce el riesgo de escalado infinito.

## Monitorización de umbrales con New Relic

Puede usar el [servicio New Relic](../monitor/new-relic-service.md) para monitorizar ciertos umbrales, como el recuento de hosts y el uso de CPU. Las siguientes consultas de New Relic utilizan una notación de variable para `cluster-id` solo con fines de ejemplo.

>[!TIP]
>
>Para obtener una referencia sobre la generación de consultas, consulte [Sintaxis, cláusulas y funciones de NRQL](https://docs.newrelic.com/docs/query-your-data/nrql-new-relic-query-language/get-started/nrql-syntax-clauses-functions/) en la documentación de _New Relic_.
>Use sus consultas para crear un [panel de New Relic](https://docs.newrelic.com/docs/query-your-data/explore-query-data/dashboards/introduction-dashboards/).

### Recuento de hosts

La siguiente consulta de ejemplo de New Relic muestra el recuento de hosts dentro del entorno:

```sql
SELECT uniqueCount(SystemSample.entityId) AS 'Infrastructure hosts', uniqueCount(Transaction.host) AS 'APM hosts seen' FROM SystemSample, Transaction where (Transaction.appName = 'cluster-id_stg' AND Transaction.transactionType = 'Web') OR SystemSample.apmApplicationNames LIKE '%|cluster-id_stg|%' TIMESERIES SINCE 3 HOURS AGO
```

En la siguiente captura de pantalla, **hosts de APM vistos** hace referencia al número de hosts con transacciones registradas durante el período seleccionado.

![Número de hosts de New Relic](../../assets/new-relic/host-count.png)

### Uso de CPU

La siguiente consulta de ejemplo de New Relic muestra el uso de CPU para nodos web:

```sql
SELECT average(cpuPercent) FROM SystemSample FACET hostname, apmApplicationNames WHERE instanceType LIKE 'c%' TIMESERIES SINCE 3 HOURS AGO
```

![Uso de CPU en los nodos web de New Relic](../../assets/new-relic/web-node-cpu-usage.png)

## Habilitar escalado automático

Para habilitar o deshabilitar el escalado automático para su proyecto de infraestructura de Adobe Commerce en la nube, [Envíe un ticket de soporte de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket). Elija las siguientes razones en el ticket:

- **Razón de contacto**: Solicitud de cambio de infraestructura
- **Motivo de contacto de infraestructura de Adobe Commerce**: otra solicitud de cambio de infraestructura

>[!IMPORTANT]
>
>La función de escalado automático captura eventos no anticipados. Aunque tenga habilitada la escala automática, Adobe recomienda que continúe [enviando un ticket de soporte de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) si espera un evento próximo.

### Prueba de carga

El Adobe habilita el escalado automático en el clúster _staging_ del proyecto en la nube primero. Después de realizar y completar las pruebas de carga en el entorno, Adobe habilita el escalado automático en el clúster de producción. Para obtener instrucciones sobre las pruebas de carga, consulte [Pruebas de rendimiento](../launch/checklist.md#performance-testing).

### LISTA DE PERMITIDOS IP

Después de habilitar el escalado automático, el tráfico del nodo web saliente se origina desde las direcciones IP de los nodos de servicio. Si utiliza una lista de permitidos con un servicio de terceros que no está empaquetado con su proyecto de Adobe Commerce en la nube, compruebe las direcciones IP en la lista de permitidos de servicio de terceros.

Por ejemplo:

- Si la lista de permitidos contiene las direcciones IP de los nodos de servicio (1, 2 y 3), no se requiere ninguna acción.
- Si la lista de permitidos contiene las direcciones IP de los nodos de servicio (1, 2 y 3) y los nodos web (4, 5 y 6), en este caso los seis nodos, no se requiere ninguna acción.
- Si la lista de permitidos contiene las direcciones IP _solamente_ para los nodos web (4, 5 y 6), debe actualizar la lista de permitidos para incluir las direcciones IP para los nodos de servicio.
