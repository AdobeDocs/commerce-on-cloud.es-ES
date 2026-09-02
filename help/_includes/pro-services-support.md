---
source-git-commit: 79ac13115bd3f275651a5477f2939c8f00a5a985
workflow-type: tm+mt
source-wordcount: '704'
ht-degree: 0%

---
# Asistencia de servicios Pro y disponibilidad del cliente

## Asistencia de servicios Pro

Para solicitar y completar una actualización del servicio Pro en Ensayo o Producción, siga estos pasos:

1. **Para instalar o actualizar [servicios](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/configure/service/services-yaml) solo en `Staging` y `Production` entornos**, envía un [ticket de soporte de Adobe Commerce](https://experienceleague.adobe.com/es/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket).

   En el ticket, especifique los cambios de servicio requeridos, incluya los archivos `.magento.app.yaml` y `.magento/services.yaml` actualizados y anote la versión de PHP de destino.

   La versión de PHP, las actualizaciones de Compositor, las extensiones y la configuración del entorno son cambios de autoservicio. Es posible que Adobe tenga que actualizar el agente de New Relic para comprobar la compatibilidad con la versión de PHP. Ver [configuración de PHP](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/configure/app/php-settings) en _Configuración de la aplicación_.

   >[!IMPORTANT]
   >
   >Al seleccionar el campo **[!UICONTROL Environment]** en el formulario de solicitud, utilice el nombre de entorno de Adobe. Por ejemplo, seleccione Ensayo aunque llame internamente a ese entorno **Dev**. Puede mencionar su nombre interno en la descripción, pero el campo [!UICONTROL Environment] debe utilizar la nomenclatura de Adobe.

1. **Confirme la programación de actualización** a través del proceso en dos partes de Adobe: primero confirme la fecha y la hora solicitadas y después el equipo de asistencia la envía al equipo de infraestructura para que la confirme definitivamente.

   Los cambios de producción (solo Pro) requieren al menos dos días hábiles de antelación, excepto los fines de semana. Por ejemplo, el equipo de Infraestructura en la nube debe confirmar una actualización el lunes antes del miércoles anterior. Se espera un tiempo de espera adicional durante la demanda máxima. Para evitar demoras, responda a la solicitud inicial al menos 48 horas antes de la ventana. La actualización no se considera programada hasta que reciba la confirmación final.

   >[!NOTE]
   >
   >Proporcionar ventanas de mantenimiento en UTC. Las actualizaciones de ensayo no se programan con antelación y suelen completarse el mismo día que la solicitud.
   >
   >Después de una actualización de RabbitMQ, vuelva a implementar el entorno para reiniciar las colas de mensajes.

1. **Valide la actualización** en un entorno de ensayo o integración antes de programarla en producción.

   Los problemas causados por módulos de terceros, código personalizado o compatibilidad de dependencias a menudo aparecen durante la reimplementación que sigue a una actualización de servicio. Para validar varias actualizaciones de servicio de una en una, un pedido razonable es Valkey o Redis, luego RabbitMQ, luego OpenSearch y luego MariaDB. Esta secuencia no es obligatoria. Las actualizaciones de bases de datos tienen el mayor impacto operativo y merecen la mayor precaución.

   Adobe no garantiza la duración exacta de una ventana de mantenimiento de producción con antelación, ya que el tiempo depende del entorno y los servicios implicados. Utilice el tiempo que tarda la actualización de ensayo como estimación práctica al planificar la ventana Producción.

1. **Vuelva a implementar el entorno** después de que Adobe complete la actualización del servicio para que el cambio surta efecto, incluso si la versión de la aplicación de Adobe Commerce no cambia.

   Si la actualización incluye OpenSearch, planifique también una reindexación completa. Adobe no puede garantizar que no haya tiempo de inactividad durante una actualización de servicio, por lo que debe planificar una ventana de mantenimiento que permita volver a implementar, reindexar si es necesario y validar la tienda y el administrador antes de volver a abrir el sitio.

## Disponibilidad del cliente durante las actualizaciones

**Un representante de su equipo o asociado de implementación debe estar disponible en línea durante la ventana de actualización de producción programada.** La programación durante un período de poco tráfico no impide la actualización. Adobe administra la actualización de la infraestructura en la nube, pero no puede validar el comportamiento de la aplicación, las integraciones, el código personalizado o los flujos de trabajo empresariales.

El representante disponible deberá poder:

- **Supervisar** la tienda y las transacciones comerciales críticas durante y después de la actualización.
- **Responda** a preguntas del equipo de soporte técnico de Adobe o del equipo de infraestructura de nube.
- **Confirme** que las integraciones, extensiones, personalizaciones, trabajos cron, colas y otras funciones específicas del cliente funcionan según lo esperado.
- **Validar** flujos de trabajo críticos para la empresa, como cierre de compra, vistas de catálogo, búsqueda, inicio de sesión y procesamiento de pedidos.
- **Informar** de un comportamiento inesperado rápidamente, mientras el contexto de actualización y los registros siguen disponibles.

>[!TIP]
>
>Para los proyectos Pro, las actualizaciones de servicios en Producción también requieren una programación avanzada y un proceso de confirmación en dos partes con el Soporte de Adobe. Ver [Soporte de servicios Pro](#pro-services-support).

### Modo de mantenimiento

**El modo de mantenimiento no reemplaza la disponibilidad del cliente.** El modo de mantenimiento bloquea el acceso a la tienda, pero no valida los servicios de aplicaciones, las integraciones, las colas, los trabajos cron, el cierre de compra u otras funciones específicas del cliente.

Si el trabajo planificado requiere un modo de mantenimiento, coordine su uso con el Soporte de Adobe y siga las instrucciones para esa actualización. Después, confirme que la tienda y los flujos de trabajo críticos funcionan normalmente antes de considerar que el trabajo se ha completado.
