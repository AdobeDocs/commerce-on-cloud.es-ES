---
source-git-commit: 67ed09e3b7c5f5218407b6648e8ca2c32933bbda
workflow-type: tm+mt
source-wordcount: '1008'
ht-degree: 0%

---
# Fragmentos de nube

## Advertencia de Elasticsearch {#elasticsearch-support}

>[!WARNING]
>
>Elasticsearch 7 y versiones posteriores no son compatibles con Adobe Commerce en infraestructuras en la nube. Adobe Commerce 2.4.4 y versiones posteriores admiten el servicio OpenSearch.

## Integración mejorada {#enhanced-integration-envs}

>[!NOTE]
>
>Los proyectos aprovisionados antes del 5 de junio de 2020 tenían varios entornos de integración más pequeños. Si necesita un entorno de integración más grande para pruebas y desarrollo, solicite una actualización a entornos de integración mejorados. Consulte el artículo [Solicitud de entorno de integración](https://experienceleague.adobe.com/en/docs/experience-cloud-kcs/kbarticles/ka-27242) en el _Centro de ayuda de Adobe Commerce_ para obtener más información.

## Opciones de combinación {#merge-options}

De manera predeterminada, el proceso de implementación sobrescribe todos los valores del archivo `env.php`; sin embargo, puede optar por combinar uno o varios valores de una configuración de servicio sin sobrescribir todos los valores.

Establezca la opción `_merge` en una de las siguientes opciones:

- `true`—**Combinar** los valores de servicio configurados con los valores de variable de entorno.
- `false`—**Sobrescribir** los valores de servicio configurados con los valores de variable de entorno.

## Repositorio privado {#private-repository}

>[!NOTE]
>
>Adobe recomienda utilizar un repositorio privado para su proyecto de infraestructura de Adobe Commerce en la nube a fin de proteger cualquier trabajo de desarrollo o información de propiedad, como extensiones y configuraciones confidenciales.

## Advertencia de autoservicio de Pro {#pro-self-service-warning}

>[!WARNING]
>
>Algunos **proyectos Pro** requieren asistencia del soporte técnico de Adobe para actualizar las configuraciones de ruta en el archivo `routes.yaml` y las configuraciones cron en el archivo `.magento.app.yaml`. Adobe recomienda realizar y validar primero todos los cambios de configuración de YAML en un entorno de integración y luego implementarlos en el entorno de ensayo.
>
>
>Si los cambios no se reflejan en los sitios de ensayo después de la reimplementación y no hay mensajes de error relacionados en el registro, **debe** [enviar un ticket de soporte de Adobe Commerce](https://experienceleague.adobe.com/en/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket). En el ticket, describa claramente los cambios de configuración que ha intentado y adjunte cualquier archivo de configuración YAML actualizado en el ticket.

## Backups Pro {#pro-backups}

>[!TIP]
>
>Para recuperar una copia de seguridad específica en entornos de ensayo y producción de Pro, [envíe un ticket de soporte de Adobe Commerce](https://experienceleague.adobe.com/en/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket) indicando la fecha, la hora y la zona horaria en el ticket.
>
>Adobe **no** restaura ningún entorno desde una copia de seguridad automática. Consulte [Restaurar una instantánea de base de datos desde Ensayo o Producción](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/how-to/restore-a-db-snapshot-from-staging-or-production) para obtener ayuda sobre cómo elegir un método para restaurar una instantánea de ensayo o producción.

## Advertencia de reimplementación {#redeploy-warning}

>[!WARNING]
>
>El proceso de implementación comienza cuando se realiza una combinación, inserción o sincronización del entorno, o cuando se déclencheur una reimplementación manual, durante la cual la aplicación [!DNL Commerce] se encuentra en modo de mantenimiento. En un entorno de producción, Adobe recomienda completar este trabajo durante las horas de menor actividad para evitar interrupciones en el servicio.

## Marcador de posición de ruta {#route-placeholder}

>[!NOTE]
>
>Los siguientes ejemplos de configuración de rutas utilizan plantillas de rutas con marcadores de posición. El marcador de posición `{default}` representa el dominio predeterminado configurado para el sitio. Si el proyecto tiene varios dominios, use el marcador de posición `{all}` para configurar el enrutamiento para el dominio predeterminado y todos los alias. Consulte [Configurar rutas](/help/cloud-guide/routes/routes-yaml.md).

## Sincronización de SCD {#scd-timing-warning}

>[!WARNING]
>
>Si tiene problemas con archivos de contenido estático en la aplicación después de la implementación, como la falta de archivos de temas personalizados, aumente el tiempo de ejecución máximo esperado a 900 segundos o superior.

## Implementación basada en escenarios {#scenarios}

>[!NOTE]
>
>Con [!DNL ECE-Tools] 2002.1.0 y versiones posteriores, puede utilizar la característica de implementación basada en escenarios para personalizar los procesos de compilación, implementación y posteriores a la implementación para su proyecto de infraestructura de Adobe Commerce en la nube. Consulte [Implementación basada en escenarios](/help/cloud-guide/deploy/scenario-based.md).

## Segunda fase {#second-staging}

>[!NOTE]
>
>Algunos proyectos requieren un flujo de trabajo de desarrollo más sofisticado. Para satisfacer esta necesidad, Adobe ofrece [entorno de ensayo adicional](/help/cloud-guide/test/second-staging.md) como opción adicional a su infraestructura en la nube.

## Instrucción de servicio {#service-instruction}

Siga estas instrucciones para la configuración del servicio en entornos de integración profesional y entornos de inicio, incluida la rama `master`.

>[!NOTE]
>
>Para cambiar la configuración del servicio en los entornos de ensayo y producción de Pro, [envíe un vale de soporte de Adobe Commerce](https://experienceleague.adobe.com/en/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket). Para conocer los requisitos de programación y las instrucciones de disponibilidad del cliente, consulte [Soporte de servicios Pro](https://experienceleague.adobe.com/en/docs/cloud-guide/services/services-yaml.md#pro-services-support) en _Configurar servicios_.

## Cambio de servicio {#service-change-tip}

>[!TIP]
>
>Después de la instalación inicial del servicio, puede cambiar la versión del software de un servicio instalado actualizando los archivos de configuración `services.yaml` y `.magento.app.yaml`. Consulte [Cambiar la versión del servicio](/help/cloud-guide/services/services-yaml.md#change-service-version) para obtener instrucciones sobre cómo actualizar o degradar un servicio. Este método de autoservicio no se aplica a los entornos de ensayo o producción Pro; consulte [Compatibilidad con servicios Pro](https://experienceleague.adobe.com/en/docs/cloud-guide/services/services-yaml.md#pro-services-support) en _Configurar servicios_.

## Sugerencia de implementación atascada {#stuck-deployment-tip}

>[!TIP]
>
>Para obtener ayuda con implementaciones bloqueadas, use el [solucionador de problemas de implementación de Adobe Commerce](https://experienceleague.adobe.com/en/docs/experience-cloud-kcs/kbarticles/ka-29640) en el _Centro de ayuda de Commerce_.

## Actualización a ECE-Tools {#ece-tools-package}

>[!NOTE]
>
>Para quitar los paquetes obsoletos de las versiones de Adobe Commerce en la infraestructura en la nube que no contienen el paquete `ece-tools`, debe realizar una [actualización única](/help/cloud-guide/dev-tools/install-package.md) a su proyecto en la nube. Si actualmente usa el paquete `ece-tools` y necesita actualizarlo, consulte [Actualizar el paquete ECE-Tools](/help/cloud-guide/dev-tools/update-package.md).

## Sugerencia de actualización {#upgrade-tip}

>[!TIP]
>
>Antes de comenzar una actualización o un proceso de aplicación de parches, cree una rama activa desde el entorno de integración y extraiga la nueva rama a su estación de trabajo local. La dedicación de una rama al proceso de actualización o de revisión ayuda a evitar interferencias con el trabajo en curso.

## Valkey en New Relic {#valkey-newrelic}

>[!NOTE]
>
>New Relic puede mostrar Redis incluso después de la migración a Valkey.
>
>Se espera que New Relic continúe refiriéndose al servicio de caché como Redis incluso después de que el entorno se haya migrado a Valkey.
>
>Valkey es una ramificación de código abierto de Redis, y algunas herramientas e integraciones siguen identificando el servicio mediante el uso de nombres de Redis en lugar de una etiqueta de Valkey distinta. Este comportamiento no indica necesariamente que Redis siga instalado.

<!-- Fastly-related snippets begin -->

## Inicio de sesión de administrador {#admin-login-step}

1. [Inicie sesión](/help/get-started/onboarding.md#access-your-admin-panel) en el administrador.

## Automatización de la implementación de fragmentos VCL personalizados {#automate-vcl-snippet-deployment}

>[!NOTE]
>
>En lugar de cargar manualmente fragmentos de VCL personalizados, puede agregar fragmentos al directorio `$MAGENTO_CLOUD_APP_DIR/var/vcl_snippets_custom` de su entorno. Los fragmentos de este directorio se cargan automáticamente al hacer clic en _cargar VCL a Fastly_ en el administrador de Commerce. Consulte [Implementación de fragmentos de VCL personalizados automatizados](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/CUSTOM-VCL-SNIPPETS.md#automated-custom-vcl-snippets-deployment) en el módulo Fastly de CDN para la documentación de Magento 2.

<!-- Fastly-related snippets end -->
