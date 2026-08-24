---
title: Escalado automático
description: Descubra cómo se puede ampliar Adobe Commerce en la infraestructura en la nube para satisfacer las demandas de recursos.
feature: Cloud, Auto Scaling
topic: Architecture
exl-id: 11bfde40-79d1-4d51-9233-150c4cfb80fd
TQID: https://experienceleague.adobe.com/uL--0lHHJ-4SN3BkFU8reAefWhpMQOLBRVG7fX3jTM8
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
subfeature_v2: id: db6b6496-d1b5-4ad4-9e18-dea78dae3aa8
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: a542dac902dc0de7c0836c1e5e4aece40fc6cbee
workflow-type: tm+mt
source-wordcount: 979
ht-degree: 0%

---

# Escalado automático

El escalado automático añade o elimina automáticamente los recursos a la infraestructura de la nube para mantener un rendimiento óptimo y costes razonables. Adobe ofrece dos tipos de escalado automático para [!DNL Adobe Commerce on cloud infrastructure] proyectos:

- [Escalado automático horizontal](#horizontal-auto-scaling) (disponible solo para arquitectura escalada): agrega o quita nodos de servidor web para proyectos de arquitectura escalada.
- [Escalado automático vertical](#vertical-auto-scaling) (disponible para arquitectura Pro estándar o escalada): cambia el tamaño de la capacidad de CPU de los nodos existentes para adaptarse a los cambios en la demanda.


## Habilitar escalado automático

Para habilitar o deshabilitar el escalado automático horizontal o vertical para su proyecto [!DNL Adobe Commerce on cloud infrastructure], [Envíe un ticket de soporte de Adobe Commerce](https://experienceleague.adobe.com/en/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket). Elija las siguientes razones en el ticket:

- **Razón de contacto**: Solicitud de cambio de infraestructura
- **Motivo de contacto de infraestructura de Adobe Commerce**: otra solicitud de cambio de infraestructura

>[!IMPORTANT]
>
>La función de escalado automático captura eventos no anticipados. Aunque tenga habilitada la escala automática, Adobe recomienda que continúe [enviando un ticket de soporte de Adobe Commerce](https://experienceleague.adobe.com/en/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket) si espera un evento próximo.

### Prueba de carga

Adobe habilita primero el escalado automático en el clúster _staging_ del proyecto en la nube. Después de realizar y completar las pruebas de carga en el entorno, Adobe permite el escalado automático en el clúster de producción. Para obtener instrucciones sobre las pruebas de carga, consulte [Pruebas de rendimiento](../launch/checklist.md#performance-testing).

## Escalado automático horizontal

Actualmente, esta característica solo está disponible para proyectos configurados con una [arquitectura escalada](scaled-architecture.md).

El escalado automático horizontal agrega o elimina nodos de servidor web para proyectos de arquitectura escalada. Como alternativa, [escala automática vertical](#vertical-auto-scaling) cambia el tamaño de la capacidad de CPU de los nodos existentes para adaptarse a los cambios en la demanda.

### Nodos del servidor web

El [nivel web](scaled-architecture.md#web-tier) se escala para adaptarse al aumento en las solicitudes de procesos y a los requisitos de tráfico más altos. Actualmente, la función de escalado automático solo se escala horizontalmente añadiendo o eliminando nodos de servidor web.

Se produce un evento de escalado automático cuando el uso y el tráfico de CPU alcanzan un umbral predefinido:

- **Nodos agregados**: las CPU/núcleos en todos los nodos web activos tienen una capacidad del 75% durante 1 minuto y el tráfico aumenta un 20% durante 5 minutos consecutivos.
- **Nodos eliminados**: las CPU y los núcleos de todos los nodos web activos se cargan al 60 % durante 20 minutos. Los nodos se eliminan en el orden en que se agregaron.

Los umbrales mínimo y máximo se determinan y establecen en función de los límites de recursos contratados de cada comerciante; esto reduce el riesgo de escalado infinito.

### Monitorización de umbrales con New Relic

Puede usar el [servicio New Relic](../monitor/new-relic-service.md) para monitorizar ciertos umbrales, como el recuento de hosts y el uso de CPU. Las siguientes consultas de New Relic utilizan una notación de variable para `cluster-id` solo con fines de ejemplo.

>[!TIP]
>
>Para obtener una referencia sobre la generación de consultas, consulte [Sintaxis, cláusulas y funciones de NRQL](https://docs.newrelic.com/docs/query-your-data/nrql-new-relic-query-language/get-started/nrql-syntax-clauses-functions/) en la documentación de _New Relic_.
>Use sus consultas para crear un [panel de New Relic](https://docs.newrelic.com/docs/query-your-data/explore-query-data/dashboards/introduction-dashboards/).

#### Recuento de hosts

La siguiente consulta de ejemplo de New Relic muestra el recuento de hosts dentro del entorno:

```sql
SELECT uniqueCount(SystemSample.entityId) AS 'Infrastructure hosts', uniqueCount(Transaction.host) AS 'APM hosts seen' FROM SystemSample, Transaction where (Transaction.appName = 'cluster-id_stg' AND Transaction.transactionType = 'Web') OR SystemSample.apmApplicationNames LIKE '%|cluster-id_stg|%' TIMESERIES SINCE 3 HOURS AGO
```

En la siguiente captura de pantalla, **hosts de APM vistos** hace referencia al número de hosts con transacciones registradas durante el período seleccionado.

![Número de hosts de New Relic](../../assets/new-relic/host-count.png)

#### Uso de CPU

La siguiente consulta de ejemplo de New Relic muestra el uso de CPU para nodos web:

```sql
SELECT average(cpuPercent) FROM SystemSample FACET hostname, apmApplicationNames WHERE instanceType LIKE 'c%' TIMESERIES SINCE 3 HOURS AGO
```

![Uso de CPU en los nodos web de New Relic](../../assets/new-relic/web-node-cpu-usage.png)

### LISTA DE PERMITIDOS IP

Después de habilitar el escalado automático, el tráfico del nodo web saliente se origina desde las direcciones IP de los nodos de servicio. Si utiliza una lista de permitidos con un servicio de terceros que no está empaquetado con su proyecto de Adobe Commerce en la nube, compruebe las direcciones IP en la lista de permitidos de servicio de terceros.

Por ejemplo:

- Si la lista de permitidos contiene las direcciones IP de los nodos de servicio (1, 2 y 3), no se requiere ninguna acción.
- Si la lista de permitidos contiene las direcciones IP de los nodos de servicio (1, 2 y 3) y los nodos web (4, 5 y 6), en este caso los seis nodos, no se requiere ninguna acción.
- Si la lista de permitidos contiene las direcciones IP _solamente_ para los nodos web (4, 5 y 6), debe actualizar la lista de permitidos para incluir las direcciones IP para los nodos de servicio.

## Escalado automático vertical

Además del tradicional [escalado automático horizontal](#auto-scaling), [!DNL Adobe Commerce on cloud infrastructure] también ofrece escalado automático vertical tanto para proyectos de arquitectura profesional estándar como para proyectos de arquitectura escalada.

En lugar de agregar o quitar nodos, el escalado automático vertical cambia el tamaño de la capacidad de CPU de los nodos existentes para adaptarse a los cambios en la demanda. Esto complementa el escalado automático horizontal, que agrega o elimina nodos de servidor web para proyectos de arquitectura escalada.

- **Nodos agregados**: no aplicable. El escalado automático vertical cambia el tamaño de los nodos existentes en lugar de agregar nuevos.
- **Aumento de tamaño del nodo**: Se cambia el tamaño de un nodo al siguiente tamaño de instancia más grande cuando la presión de memoria sobrepasa el umbral definido. Solo se aplica un aumento de tamaño por evento de escalado.
- **Reducción de tamaño del nodo**: Los nodos se reducen automáticamente después de que cede la demanda. Los tamaños mínimo y máximo se establecen en función del patrón de uso de cada proyecto y de los límites de recursos contratados, lo que reduce el riesgo de un escalado innecesario.

### Umbrales de escalado automático

Los eventos verticales de escalado automático se activan mediante la Información de parada de presión (PSI) para la memoria en Linux, que mide cuánto tiempo pasa un sistema paralizado debido a la presión de la memoria. Adobe establece umbrales basados en los límites de recursos contratados y los patrones de uso del proyecto; los comerciantes no pueden configurarlos actualmente.

### Monitorización de umbrales con New Relic

Puede usar el servicio [!DNL New Relic] para supervisar los detalles de la instancia de infraestructura, incluidos el tamaño y el tipo de la instancia. Configure alertas en New Relic para recibir notificaciones cada vez que un evento de escalado automático vertical cambie el tamaño o el tipo de una instancia.

### Impacto en su entorno

El escalado automático vertical tiene el siguiente impacto en su entorno:

- **Tiempo de inactividad**: no se prevé tiempo de inactividad al cambiar el tamaño de un nodo.
- **Intervalo**: Cambiar el tamaño de un nodo suele llevar entre 20 y 30 minutos. El nodo se elimina temporalmente del equilibrador de carga mientras se realiza el cambio de tamaño.
