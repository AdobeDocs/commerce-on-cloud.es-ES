---
user-guide-title: Guía de Commerce en la nube
user-guide-description: Aprenda a administrar la aplicación de Adobe Commerce en la infraestructura en la nube.
product: magento
feature: Cloud
source-git-commit: fd7879e8f3c9e1965cf4aa3d99824e577971529d
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 7%

---


# Commerce en infraestructura en la nube {#user-guide}

+ [Commerce](overview.md)
+ Arquitectura {#architecture}
   + [Infraestructura en nube](architecture/cloud-architecture.md)
   + [Seguridad](architecture/security.md)
   + [Pila de tecnología](architecture/tech-stack.md)
   + [Arquitectura de inicio](architecture/starter-architecture.md)
   + [Flujo de trabajo inicial](architecture/starter-develop-deploy-workflow.md)
   + [Arquitectura profesional](architecture/pro-architecture.md)
   + [Flujo de trabajo profesional](architecture/pro-develop-deploy-workflow.md)
   + [Arquitectura a escala](architecture/scaled-architecture.md)
   + [Escalado automático](architecture/autoscaling.md)
+ [Introducción](https://experienceleague.adobe.com/docs/commerce-on-cloud/start/overview.html)
+ Notas de la versión {#release-notes}
   + [Conjunto de herramientas de nube](release-notes/cloud-tools-suite.md)
   + [Paquete ECE-Tools](release-notes/ece-tools-package.md)
   + [Parches de nube](release-notes/cloud-patches.md)
   + [Paquete Cloud Docker](release-notes/cloud-docker.md)
   + [Componentes de nube](release-notes/cloud-components.md)
   + [Paquetes de nube](release-notes/cloud-packages.md)
   + [Cambios incompatibles con versiones anteriores](release-notes/backward-incompatible-changes.md)
   + [Archivo de notas de versión](release-notes/cloud-release-archive.md)
+ Proyecto en la nube {#project}
   + [Resumen del proyecto](project/overview.md)
   + [Estructura del proyecto](project/file-structure.md)
   + [Acceso de usuario](project/user-access.md)
   + [Autenticación de varios factores](project/multi-factor-authentication.md)
   + [Flujo de actividad](project/activity-stream.md)
   + [Correos electrónicos salientes](project/outgoing-emails.md)
   + [Servicio de correo electrónico SendGrid](project/sendgrid.md)
   + [Administración de ramas de consola](project/console-branches.md)
   + [Direcciones IP regionales](project/regional-ip-addresses.md)
+ Herramientas para desarrolladores {#dev-tools}
   + [Información general](dev-tools/overview.md)
   + CLI de nube {#cloud-cli}
      + [Información general sobre CLI](dev-tools/cloud-cli-overview.md)
      + [Referencia de CLI](dev-tools/cloud-cli-reference.md)
   + [Cloud Docker](dev-tools/cloud-docker.md)
   + ECE-Tools {#ece-tools}
      + [Introducción a paquetes](dev-tools/package-overview.md)
      + [Actualización única para utilizar ECE-Tools](dev-tools/install-package.md)
      + [Actualizar el paquete de herramientas ECE](dev-tools/update-package.md)
      + [Referencia de CLI](dev-tools/ece-tools-cli-reference.md)
      + [Referencia de error](dev-tools/error-reference.md)
   + Integraciones {#integrations}
      + [Información general](integrations/overview.md)
      + [Bitbucket](integrations/bitbucket.md)
      + [GitHub](integrations/github.md)
      + [GitLab](integrations/gitlab.md)
      + [Notificaciones de estado](integrations/health-notifications.md)
+ Desarrollo {#develop}
   + [Información general](development/overview.md)
   + [Claves de autenticación](development/authentication-keys.md)
   + [Administración de ramas CLI](development/cli-branches.md)
   + [Conexiones seguras](development/secure-connections.md)
   + Implementar {#deploy}
      + [Proceso de implementación](deploy/process.md)
      + [Optimización](deploy/optimization.md)
      + [Prácticas recomendadas](deploy/best-practices.md)
      + [Implementación basada en escenarios](deploy/scenario-based.md)
      + [Implementación sin tiempo de inactividad](deploy/reduce-downtime.md)
      + [Implementación de contenido estático](deploy/static-content.md)
      + [Asistentes inteligentes](deploy/smart-wizards.md)
      + [Implementar en ensayo y producción](deploy/staging-production.md)
      + [Recuperar de un error de componente](deploy/recover-failed-deployment.md)
   + Prueba {#test}
      + [Guía de pruebas](test/guidance.md)
      + [Registros](test/log-locations.md)
      + [Xdebug](test/debug.md)
      + [Datos de muestra](test/sample-data.md)
      + [Ensayo y producción](test/staging-and-production.md)
      + [Segundo entorno de ensayo](test/second-staging.md)
   + [Servicio PrivateLink](development/privatelink-service.md)
   + [Bloque protector](development/protective-block.md)
   + [Restaurar entorno](development/restore-environment.md)
   + Almacenamiento {#storage}
      + [Administrar espacio en disco](storage/manage-disk-space.md)
      + [Consultas de base de datos de perfil](storage/profile-database-queries.md)
      + [Realizar copia de seguridad de la base de datos](storage/database-dump.md)
      + [Administración de backup](storage/snapshots.md)
   + Actualizaciones y parches {#upgrade}
      + [Prácticas recomendadas](development/best-practices.md)
      + [Actualizar la versión de Commerce](development/commerce-version.md)
      + [Aplicar parches](development/apply-patches.md)
+ Configuración {#configure}
   + [Información general](environment/overview.md)
   + Aplicación {#app}
      + [Configurar la implementación de aplicaciones](application/configure-app-yaml.md)
      + [Configuración de PHP](application/php-settings.md)
      + Propiedades {#properties}
         + [Propiedades de aplicación](application/properties.md)
         + [Crons](application/crons-property.md)
         + [Cortafuegos (solo Starter)](application/firewall-property.md)
         + [Enlaces](application/hooks-property.md)
         + [Variables](application/variables-property.md)
         + [Web](application/web-property.md)
         + [Trabajadores](application/workers-property.md)
      + [Establecer caché para archivos estáticos](application/set-cache.md)
   + Entorno {#env}
      + [Configurar la implementación del entorno](environment/configure-env-yaml.md)
      + [Niveles variables y opciones](environment/variable-levels.md)
      + Anular variables {#stage}
         + [Variables de entorno](environment/variables-intro.md)
         + [ADMINISTRADOR](environment/variables-admin.md)
         + [Variables de nube](environment/variables-cloud.md)
         + [Global](environment/variables-global.md)
         + [Generar](environment/variables-build.md)
         + [Implementar](environment/variables-deploy.md)
         + [Posterior a la implementación](environment/variables-post-deploy.md)
      + Configurar notificaciones {#log}
         + [Notificaciones](environment/set-up-notifications.md)
         + [Controladores de registro](environment/log-handlers.md)
   + Rutas {#routes}
      + [Configuración de rutas](routes/routes-yaml.md)
      + [Almacenamiento en caché](routes/caching.md)
      + [Redirecciones](routes/redirects.md)
      + [Inclusiones del lado del servidor](routes/server-side-includes.md)
   + Servicios {#service}
      + [Configurar servicios](services/services-yaml.md)
      + [Elasticsearch](services/elasticsearch.md)
      + [MySQL](services/mysql.md)
      + [OpenSearch](services/opensearch.md)
      + [RabbitMQ](services/rabbitmq.md)
      + [Redis](services/redis.md)
      + [Valkey](services/valkey.md)
+ Servicios rápidos {#cdn}
   + [Información general](cdn/fastly.md)
   + Configurar rápidamente {#setup-fastly}
      + [Configurar servicios de Fastly](cdn/fastly-configuration.md)
      + [Personalizar configuración de caché](cdn/fastly-custom-cache-configuration.md)
      + [Personalización de páginas de error y mantenimiento](cdn/fastly-custom-response.md)
   + [Cortafuegos de aplicación web](cdn/fastly-waf-service.md)
   + [Optimización de imagen](cdn/fastly-image-optimization.md)
   + Personalizar con VCL {#custom-vcl-snippets}
      + [Introducción](cdn/fastly-vcl-custom-snippets.md)
      + [Redireccionar solicitudes a un servidor de CMS](cdn/fastly-vcl-wordpress.md)
      + [Bloquear spam de referencia](cdn/fastly-vcl-badreferer.md)
      + [LISTA DE PERMITIDOS IP](cdn/fastly-vcl-allowlist.md)
      + [LISTA DE BLOQUEADOS IP](cdn/fastly-vcl-blocking.md)
      + [Omitir caché de Fastly](cdn/fastly-vcl-bypass-to-origin.md)
   + [Solución de problemas rápida](cdn/fastly-troubleshooting.md)
+ Configuración de tienda {#configure-store}
   + [Información general](store/overview.md)
   + [Prácticas recomendadas](store/best-practices.md)
   + [Tema personalizado](store/custom-theme.md)
   + [Extensiones](store/extensions.md)
   + [Módulo B2B](store/b2b-module.md)
   + [Varios sitios](store/multiple-sites.md)
   + [Robots de motores de búsqueda y mapas del sitio](store/robots-sitemap.md)
   + [Formas de pago de PayPal](store/paypal.md)
   + [Administración de configuración](store/store-settings.md)
+ Sitio de lanzamiento {#launch}
   + [Información general](launch/overview.md)
   + [Iniciar lista de comprobación](launch/checklist.md)
   + [Pasos del inicio](launch/steps.md)
+ Supervisar el sitio {#monitor}
   + [Rendimiento](monitor/performance.md)
   + Servicio de New Relic {#new-relic}
      + [Información general de New Relic](monitor/new-relic-service.md)
      + [Administración de cuentas y usuarios](monitor/account-management.md)
      + Investigar rendimiento {#investigate}
         + [Políticas, alertas y flujos de trabajo](monitor/investigate-performance.md)
         + [Ingesta de datos](monitor/ingest-data.md)
         + [Seguimiento de implementaciones](monitor/track-deployments.md)
      + [Administración de registros](monitor/log-management.md)
