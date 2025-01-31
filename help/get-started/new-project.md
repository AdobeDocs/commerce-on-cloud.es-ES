---
title: Aprovisionar Commerce en la nube
description: Aprenda a preparar un asesor técnico del cliente de Adobe para aprovisionar su Adobe Commerce en proyectos de infraestructura en la nube.
recommendations: noDisplay, catalog
role: Admin
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '751'
ht-degree: 0%

---

# Requisitos previos de Commerce en Cloud Provisioning

Empecemos e iniciemos su proyecto de Commerce en la infraestructura en la nube.

Antes de que Adobe aprovisione su Commerce en entornos de proyectos en la nube, se recomienda que tenga en cuenta las siguientes estrategias y prepare respuestas para la primera consulta con el equipo de cuenta de Adobe. Utilice las siguientes secciones como lista de comprobación para prepararse para la conversación con un asesor técnico del cliente a fin de aprovisionar un proyecto en la nube:

## Definición de dominio

**Pregunta 1**: _¿Qué dominio o dominios piensa usar para el lanzamiento del sitio?_

Prepárese para integrar los servicios de Fastly y nginx definiendo sus dominios y subdominios de nivel superior para los entornos de ensayo y producción de Pro. Después de la configuración inicial, solo puede agregar dominios enviando un ticket de asistencia de Adobe Commerce, por lo que se recomienda tener preparada la información de dominio.

Ejemplos de los dominios de producción y ensayo:

- `www.your-store.com`
- `your-store.com`
- `mcprod.your-store.com`
- `mcstaging.your-store.com`

Consulte [Configurar varios sitios web o tiendas](../cloud-guide/store/multiple-sites.md) en la guía de _Commerce en infraestructura en la nube_ para obtener más información sobre dominios únicos o múltiples.

Consulte [Varias cuentas de Fastly y dominios asignados](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/cdn/fastly#multiple-fastly-accounts-and-assigned-domains){target="_blank"} si tiene una cuenta existente de Fastly que vincula los mismos Apex y subdominios utilizados en el sitio de Adobe Commerce.

## Dominio de correo electrónico transaccional

**Pregunta 2**: _Qué dominio o dominios desea usar para los correos electrónicos transaccionales?_

Adobe Commerce en la nube utiliza el servicio proxy del Protocolo simple de transferencia de correo (SMTP) de SendGrid, que proporciona servicios de autenticación de correo electrónico saliente y de monitorización de la reputación. SendGrid envía correos electrónicos transaccionales en su nombre, por lo que requiere información de dominio.

Ejemplo de dominio de SendGrid: `example@your-store.com`

Consulte [Servicio de correo de SendGrid](../cloud-guide/project/sendgrid.md) en la guía de _Commerce en infraestructura en la nube_ para obtener más información acerca de los correos electrónicos transaccionales y la configuración de dominio.

## Asignación de almacenamiento

**Pregunta 3**: _Cuánto almacenamiento contratado planea asignar para la carga de archivos y para la base de datos?_

Adobe Commerce en la infraestructura de la nube utiliza el almacenamiento para la estructura de archivos, la indexación de búsqueda y la base de datos. Puede subdividir el almacenamiento según sea necesario empezando por 10 GB para cada partición.

La cantidad de almacenamiento que contrató para su proyecto de nube representa el almacenamiento total para cada entorno. Por ejemplo, si compró 50 GB de espacio de almacenamiento, tendrá 50 GB de almacenamiento para cada entorno. Hay 50 GB de almacenamiento independiente para producción, ensayo y cada entorno de integración respectivamente.

Puede aumentar su almacenamiento contratado en cualquier momento. Para los entornos de ensayo y producción profesional, debe enviar un ticket de asistencia de Adobe Commerce para cambiar la asignación del espacio en disco. Un aumento de tamaño de los entornos de ensayo y producción profesional solo puede producirse a intervalos específicos. Según el uso actual del espacio en disco, el equipo de soporte técnico podría recomendar aumentar la asignación del espacio en disco en un mínimo de 10 GB. Una vez asignado, el aumento de almacenamiento para Ensayo y producción profesionales puede **no** revertirse.

Consulte [Administrar espacio en disco](../cloud-guide/storage/manage-disk-space.md) en la guía de _Commerce en infraestructura en la nube_.

## Región de Cloud Service

**Pregunta 4**: _Qué región de Cloud Service es más conveniente para tu proximidad?_

Elija Amazon Web Service (AWS) o Microsoft Azure como base de Infrastructure as a Service (IaaS) para sus proyectos de Adobe Commerce en la nube Pro. Cada proveedor de servicios funciona en varias regiones y proporciona varias zonas de disponibilidad. Seleccione una región adecuada para su ubicación y reduzca el potencial de latencia y los costes.

Ver [un mapa de las regiones de la nube de Adobe Commerce](../cloud-guide/overview.md).

## Servicio de conexión

**Pregunta 5**: _¿Necesita un servicio PrivateLink? En caso afirmativo, ¿en qué región se encuentra la conexión PrivateLink?_

Adobe Commerce en la infraestructura en la nube admite la integración con el servicio AWS PrivateLink o Azure Private Link. Aunque este servicio es opcional, PrivateLink se utiliza para establecer una comunicación segura y privada entre entornos de infraestructura en la nube con servicios y aplicaciones alojados en sistemas externos.

Es importante tener en cuenta la estrategia del servicio en la nube (AWS o Azure) para que la instancia de Adobe Commerce sea accesible dentro de la misma región. Consulte [Servicio PrivateLink](../cloud-guide/development/privatelink-service.md) en la guía de _Commerce en infraestructura en la nube_ para obtener más información sobre la accesibilidad regional.

## Lanzamiento del sitio de Target

**Pregunta 6**: _¿Cuál es la fecha prevista para el lanzamiento?_

El lanzamiento de un sitio requiere una configuración y pruebas iterativas para garantizar que el lanzamiento del sitio se realice correctamente. La configuración de una fecha objetivo le ayuda a usted y al equipo de la cuenta de Adobe a prepararse para las actividades finales previas al lanzamiento, que incluyen una llamada para coordinar los pasos finales.

Consulte la [descripción general del sitio de Launch](../cloud-guide/launch/overview.md) en la guía de _Commerce en infraestructura en la nube_ para revisar el proceso completo y descargar una copia de la lista de comprobación de Launch.

>[!TIP]
>
> Eche un vistazo al portal de Cloud y acceda a su nuevo proyecto de Cloud.
>
>**Siguiente paso**: [Incorporación a Commerce](onboarding.md)
