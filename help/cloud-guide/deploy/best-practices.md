---
title: Prácticas recomendadas de implementación
description: Descubra las prácticas recomendadas para implementar Adobe Commerce en la infraestructura en la nube.
feature: Cloud, Deploy, Best Practices
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '1904'
ht-degree: 0%

---

# Prácticas recomendadas de implementación

Las secuencias de comandos de generación e implementación se activan al combinar código en un entorno remoto. Estos scripts usan el entorno [archivos de configuración](../environment/overview.md) y el código de la aplicación para aprovisionar la infraestructura de la nube con los datos y servicios apropiados. Además, estos scripts se utilizan para instalar o actualizar la aplicación de Adobe Commerce, los servicios de terceros y las extensiones personalizadas en el entorno de la nube.

El proceso de generación e implementación es ligeramente diferente para cada plan:

- **Planes iniciales**: para el entorno de integración, cada rama activa se genera e implementa en un entorno completo para el acceso y las pruebas. Pruebe completamente su código después de combinarlo con la rama `staging`. Para iniciar el sitio, inserte `staging` en `master` para implementarlo en el entorno de producción. Tiene acceso completo a todas las ramas a través de [!DNL Cloud Console] y los comandos CLI.

- **Planes Pro**: para el entorno de integración, cada rama activa se crea e implementa en un entorno completo para el acceso y las pruebas. Combine su código en la rama `integration` antes de combinarlo con los entornos de ensayo y producción. Puede combinar con los entornos de ensayo y producción utilizando [!DNL Cloud Console] o los comandos SSH y `magento-cloud` CLI.

## Seguimiento del proceso

Puede realizar un seguimiento de las acciones de compilación e implementación en tiempo real mediante el terminal o los mensajes de estado [!DNL Cloud Console]: `in-progress`, `pending`, `success` o `failed` que se muestran durante el proceso de implementación. Puede ver los detalles en los archivos de registro. Ver [Ver registros](../test/log-locations.md).

Si utiliza repositorios de GitHub externos, el registro de operaciones no se muestra en la sesión de GitHub. Sin embargo, puede seguir la actividad en la interfaz para el repositorio externo y [!DNL Cloud Console]. Consulte [Integraciones](../integrations/overview.md).

>[!NOTE]
>
>En entornos de integración, no puede ver los registros de implementación desde [!DNL Cloud Console]. Esta función solo está disponible para entornos de producción y ensayo. Sin embargo, puede ver los registros de cada fase de la implementación en cualquier entorno usando los registros [build e deploy](../test/log-locations.md#build-and-deploy-logs). Para obtener información de solución de problemas, vea la [referencia de error de implementación](../dev-tools/error-reference.md).

Puede habilitar [Rastrear implementaciones con New Relic](../monitor/track-deployments.md) para supervisar eventos de implementación y analizar el rendimiento entre implementaciones.

## Prácticas recomendadas para compilaciones e implementación

Revise estas prácticas recomendadas y consideraciones para el proceso de implementación:

- **Asegúrese de que está ejecutando la versión más actual del paquete `ece-tools`**

  Vea [Notas de la versión de ECE-Tools](../release-notes/ece-tools-package.md).

- **Siga el proceso de compilación e implementación**

  Asegúrese de que tiene el código correcto en cada entorno para evitar sobrescribir configuraciones al combinar código entre entornos. Por ejemplo, para aplicar cambios de configuración a todos los entornos, modifique y pruebe los cambios en el entorno local antes de implementarlos en el entorno de integración remota. A continuación, implemente y pruebe los cambios en el entorno de ensayo antes de implementarlos en producción. Cuando se combina un entorno con otro, la implementación sobrescribe todo el código del entorno, excepto la configuración y las opciones específicas del entorno.

- **Use las mismas variables en todos los entornos**

  Los valores de estas variables pueden diferir entre entornos; sin embargo, normalmente necesita las mismas variables en cada entorno. Consulte [Administración de configuración para la configuración de la tienda](../store/store-settings.md).

- **Mantenga datos y valores de configuración confidenciales en variables específicas del entorno**

  Estos valores incluyen variables especificadas usando la CLI de nube, [!DNL Cloud Console], o agregadas al archivo `env.php`. Consulte [Niveles variables](../environment/variable-levels.md).

- **Asegúrese de que todo el código esté disponible en la rama del entorno**

  Hacer referencia al código de otras ramas, como una privada, puede causar problemas durante el proceso de generación e implementación. Por ejemplo, si hace referencia a una temática desde una rama privada, la temática no es accesible y no se puede generar con el código de la aplicación.

- **Agregue extensiones, integraciones y código en ramas iteradas**

  Realice y pruebe los cambios localmente, inserte en `integration`, luego en `staging` y `production`. Pruebe y resuelva los problemas de cada entorno antes de combinar las actualizaciones en el siguiente. Algunas extensiones e integraciones deben habilitarse y configurarse en un orden específico debido a las dependencias. Agregar y probar en grupos puede facilitar mucho el proceso de generación e implementación y ayudar a determinar dónde se producen los problemas.

- **Verificar las versiones y relaciones del servicio y la capacidad de conexión**

  Compruebe los servicios disponibles para su aplicación y asegúrese de que está utilizando la versión más actual y compatible. Consulte [Relaciones de servicio](../services/services-yaml.md#service-relationships) y [Requisitos del sistema](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html?lang=es) en la _Guía de instalación_ para ver las versiones recomendadas.

- **Realice pruebas localmente y en el entorno de integración antes de implementar en Ensayo y producción**

  Identifique y corrija los problemas en los entornos local e de integración para evitar un tiempo de inactividad prolongado al implementar en los entornos de ensayo y producción.

  >[!TIP]
  >
  >Hay [comandos del asistente inteligente](../deploy/smart-wizards.md) que puede usar para comprobar que la configuración del proyecto en la nube sigue las prácticas recomendadas para la configuración de generación e implementación, incluida la estrategia de implementación de contenido estático (SCD).

- **Después de completar la prueba en entornos locales e de integración, implemente y realice pruebas en el entorno de ensayo**

  Consulte [Pruebas de ensayo y producción](../test/staging-and-production.md).

- **Comprobar configuración del entorno de producción**

  Antes de implementar en Producción, complete las siguientes tareas:

   - Asegúrese de que puede conectarse a los tres nodos del entorno de producción usando [SSH](../development/secure-connections.md).

   - Compruebe que los indizadores están configurados en _Actualizar según lo programado_. Consulte [Modos de indexación](https://developer.adobe.com/commerce/php/development/components/indexing/) en la _Guía para desarrolladores de extensiones_.

   - Prepare el entorno actualizando cualquier variable específica del entorno en el código de producción, comprobando la disponibilidad y compatibilidad del servicio y realizando cualquier otro cambio de configuración necesario.

- **Supervisar el proceso de implementación**

  Revise los mensajes de estado de implementación y mitigue los problemas según sea necesario. Revise los [registros](../test/log-locations.md#) de Cloud para ver mensajes de registro detallados.

## Cinco fases de generación e implementación de la integración

Las siguientes fases se producen en el entorno de desarrollo local y en el entorno de integración. Para los planes Pro, el código no se implementa en los entornos de ensayo o producción en estas fases iniciales.

### Fase 1: Validación de código y configuración

Al configurar un proyecto por primera vez, [la plantilla de infraestructura en la nube](https://github.com/magento/magento-cloud) proporciona una base para los archivos de código. Este repositorio de código se clona en su proyecto como la rama `master`.

- **Para el inicio**—`master`, la rama es su entorno de producción.
- **Para Pro**—`master` comienza como rama de origen para el entorno de integración.

Cree una rama a partir de `master` para su código personalizado, extensiones y módulos, así como integraciones de terceros. Hay un entorno de integración remota para probar el código en la nube.

Al insertar el código desde el espacio de trabajo local en el repositorio remoto, se completa una serie de comprobaciones y validaciones de código antes de iniciar la compilación y la implementación de scripts. El servidor Git integrado valida lo que está insertando y realiza cambios. Por ejemplo, si agrega un servicio OpenSearch, el servidor Git integrado comprueba que la topología del clúster se modifica en consecuencia.

Si tiene un error de sintaxis en un archivo de configuración, el servidor Git rechaza la notificación push. Consulte [Bloque protector](../development/protective-block.md).

Esta fase también ejecuta `composer install` para recuperar dependencias.

### Fase 2: Generación

>[!NOTE]
>
>Durante la fase de compilación, el sitio no está en modo de mantenimiento y si se producen errores o problemas, no se desactiva. Solo genera el código que ha cambiado desde la versión anterior.

Esta fase crea la base de código y ejecuta los vínculos en la sección `build` de `.magento.app.yaml`. El vínculo de generación predeterminado es el comando `php ./vendor/bin/ece-tools` y realiza las siguientes acciones:

- Aplica revisiones en `vendor/magento/ece-patches` y revisiones opcionales específicas del proyecto en `m2-hotfixes`
- Vuelve a generar el código y la configuración [inyección de dependencia](https://experienceleague.adobe.com/es/docs/commerce-operations/implementation-playbook/glossary) (es decir, el directorio `generated/`, que incluye `generated/code` y `generated/metapackage`) utilizando `bin/magento setup:di:compile`.
- Comprueba si el archivo [`app/etc/config.php`](../store/store-settings.md) existe en la base de código. Adobe Commerce genera automáticamente este archivo si no lo detecta durante la fase de compilación e incluye una lista de módulos y extensiones. Si existe, la fase de compilación continúa con normalidad, comprime los archivos estáticos mediante GZIP e implementa, lo que reduce el tiempo de inactividad en la fase de implementación. Consulte [opciones de compilación](../environment/variables-build.md) para obtener más información sobre cómo personalizar o deshabilitar la compresión de archivos.

>[!WARNING]
>
>En este punto, no se ha creado el clúster, por lo que no intente conectarse a una base de datos ni suponga que hay un proceso daemon activo.

Una vez compilada la aplicación, se monta en un **sistema de archivos de solo lectura**. Puede configurar puntos de montaje específicos de lectura y escritura. No puede enviar por FTP al servidor y añadir módulos. En su lugar, debe agregar código al repositorio local y ejecutar `git push`, que genera e implementa el entorno. Para ver la estructura del proyecto, vea [Estructura de directorio del proyecto local](../project/file-structure.md).

### Fase 3: Preparar el slug

El resultado de la fase de compilación es un sistema de archivos de solo lectura denominado _slug_. Esta fase crea un archivo y coloca el slug en almacenamiento permanente. La próxima vez que inserte código, si un servicio no ha cambiado, utilizará el slug del archivo.

- Hace que la integración continua se genere más rápido reutilizando código sin cambios
- Si el código cambia, actualiza el slug para la siguiente compilación que se reutilizará
- Permite la reversión instantánea de una implementación, si es necesario
- Incluye archivos estáticos si el archivo `app/etc/config.php` existe en la base de código

El slug incluye todos los archivos y carpetas **excluyendo los siguientes** montajes configurados en `magento.app.yaml`:

- `"var": "shared:files/var"`
- `"app/etc": "shared:files/etc"`
- `"pub/media": "shared:files/media"`
- `"pub/static": "shared:files/static"`

### Fase 4: Implementar slugs y clústeres

Sus aplicaciones y todos los servicios de [backend](https://experienceleague.adobe.com/es/docs/commerce-operations/implementation-playbook/glossary) se proporcionan de la siguiente manera:

- Monta cada servicio en un contenedor, como un servidor web, OpenSearch, [!DNL RabbitMQ]
- Monta el sistema de archivos de lectura-escritura (montado en una cuadrícula de almacenamiento distribuido de alta disponibilidad)
- Configura la red para que los servicios puedan &quot;verse&quot; unos a otros (y solo entre sí)

>[!NOTE]
>
>Realice los cambios en una rama Git después de que se completen todas las compilaciones e implementaciones y vuelva a insertar. Todos los sistemas de archivos de entorno son _de solo lectura_. Un sistema de solo lectura garantiza implementaciones determinísticas y mejora considerablemente la seguridad del sitio porque ningún proceso puede escribir en el sistema de archivos. También sirve para garantizar que el código sea idéntico en los entornos de integración, ensayo y producción.

### Fase 5: Vínculos de implementación

>[!NOTE]
>
>Esta fase pone la aplicación [!DNL Commerce] en modo de mantenimiento hasta que se complete la implementación.

El último paso ejecuta un script de implementación, que puede utilizar para anonimizar datos en entornos de desarrollo, borrar cachés y consultar herramientas de integración continua externas. Cuando se ejecuta este script, tiene acceso a todos los servicios de su entorno, como Redis.

Si el archivo `app/etc/config.php` no existe en la base de código, los archivos estáticos se comprimen mediante `gzip` y se implementan durante esta fase, lo que aumenta la duración de la fase de implementación y el mantenimiento del sitio.

>[!NOTE]
>
>Consulte [implementar variables](../environment/variables-deploy.md) para obtener más información sobre cómo personalizar o deshabilitar la compresión de archivos.

Hay dos vínculos de implementación. El vínculo `pre-deploy.php` completa la limpieza y recuperación necesarias de los recursos y el código generados en el vínculo de generación. El vínculo `php ./vendor/bin/ece-tools deploy` ejecuta una serie de comandos y scripts:

- Si Adobe Commerce **no está instalado**, se instala con `bin/magento setup:install`, actualiza la configuración de implementación, `app/etc/env.php` y la base de datos para el entorno especificado, como Redis y las direcciones URL del sitio web. **Importante:** Cuando completó la [implementación por primera vez](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/launch/overview.html?lang=es) durante la instalación, Adobe Commerce se instaló e implementó en todos los entornos.

- Si Adobe Commerce **está instalado**, realice las actualizaciones necesarias. El script de implementación ejecuta `bin/magento setup:upgrade` para actualizar el esquema y los datos de la base de datos (lo que es necesario después de actualizar la extensión o el código principal) y también actualiza la configuración de implementación, `app/etc/env.php`, y la base de datos de su entorno. Finalmente, la secuencia de comandos de implementación borra la caché de Adobe Commerce.

- El script genera opcionalmente contenido web estático mediante el comando `magento setup:static-content:deploy`.

- Utiliza ámbitos (`-s` indicador en scripts de compilación) con una configuración predeterminada de `quick` para la estrategia de implementación de contenido estático. Puede personalizar la estrategia mediante la variable de entorno [`SCD_STRATEGY`](../environment/variables-deploy.md#scd_strategy). Para obtener detalles sobre estas opciones y características, consulte [Estrategias de implementación de archivos estáticos](../deploy/static-content.md) y el indicador `-s` para [Implementar archivos de vista estática](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html?lang=es).

>[!NOTE]
>
>El script de implementación utiliza los valores definidos por los archivos de configuración en el directorio `.magento` y, a continuación, el script elimina el directorio y su contenido. Su entorno de desarrollo local no se ve afectado.

### Posterior a la implementación: configurar el enrutamiento

Mientras se ejecuta la implementación, el proceso detiene el tráfico entrante en el punto de entrada durante 60 segundos y reconfigura el enrutamiento para que el tráfico web llegue al clúster recién creado.

Una implementación correcta quita el modo de mantenimiento para permitir el acceso normal y crea archivos de copia de seguridad (BACK) para los archivos de configuración `app/etc/env.php` y `app/etc/config.php`.

Habilite la generación de contenido estático mediante la variable `SCD_ON_DEMAND` y configure el vínculo [`post_deploy`](../application/hooks-property.md) para que borre la caché y precargue (caliente) la caché _después de_ de que el contenedor empiece a aceptar conexiones y _durante_ tráfico entrante normal.

Para revisar los registros de compilación e implementación, consulte [Ver registros](../test/log-locations.md#view-and-manage-logs).
