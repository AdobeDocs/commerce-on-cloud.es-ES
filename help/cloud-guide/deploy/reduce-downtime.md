---
title: Implementación sin tiempo de inactividad
description: Aprenda a reducir el tiempo de inactividad general al implementar Adobe Commerce en proyectos de infraestructura en la nube.
feature: Cloud, Deploy, SCD, Themes
exl-id: c216c5e9-d787-4428-b67a-b6aee814ded5
source-git-commit: b831bc5bce0f76ec8972b3578c500508dd4d7d41
workflow-type: tm+mt
source-wordcount: '486'
ht-degree: 0%

---

# Implementación sin tiempo de inactividad

Adobe Commerce en la infraestructura en la nube ejecuta la aplicación en [_mantenimiento_ modo](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/setup/application-modes.html?lang=es#production-mode) durante la fase de implementación, lo que desconecta el sitio hasta que se complete la implementación. El tiempo que el sitio de producción esté en modo de mantenimiento depende del tamaño del sitio, el número de cambios aplicados durante la implementación y la configuración para la implementación de contenido estático. Es posible configurar el proyecto de modo que se implemente con un efecto de tiempo de inactividad de **zero**.

Durante el proceso de implementación, todas las conexiones se ponen en cola durante un máximo de 5 minutos y conservan las sesiones activas y las acciones pendientes, como agregar al carro de compras o cerrar la compra. Después de la implementación, la cola se libera y las conexiones continúan sin interrupción. Para aprovechar esta _conexión_ y reducir el tiempo de inactividad de la implementación a _cero_, debe configurar el proyecto para que utilice la estrategia de implementación más eficiente.

>[!NOTE]
>
>Para comprobar si el proyecto de Cloud está configurado de manera óptima para minimizar el tiempo de inactividad de la implementación, use el [Asistente inteligente](smart-wizards.md). El Asistente inteligente comprueba la configuración actual y le guía a través de los ajustes de configuración recomendados para habilitar las prácticas recomendadas para implementaciones sin tiempo de inactividad.

Siga estos pasos para reducir el tiempo que tarda su tienda en implementar una actualización en Producción:

1. [Actualice al paquete `ece-tools`](../dev-tools/install-package.md) o [actualice la versión `ece-tools`](../dev-tools/update-package.md)
El proyecto de infraestructura en la nube de Adobe Commerce debe tener el paquete `ece-tools` más reciente para disponer de las herramientas necesarias para una implementación óptima. Si tiene el(la) más reciente `ece-tools`, continúe con el paso siguiente.

   >[!NOTE]
   >
   >Aunque es recomendable usar el paquete `ece-tools` más reciente, el método de implementación de tiempo de inactividad cero funciona con `ece-tools` [versión 2002.0.13](../release-notes/cloud-release-archive.md#v2002013) y posteriores.

1. [Configurar la implementación de contenido estático](static-content.md)
Si la implementación de contenido estático falla en la fase de implementación, el sitio se queda atascado en el modo de mantenimiento. Cuando se produce un error durante la fase de compilación, el proceso evita el tiempo de inactividad porque nunca comienza la fase de implementación. [La generación de contenido estático durante la fase de compilación con HTML minimizado](static-content.md#setting-the-scd-on-build), también conocido como el estado ideal, es la configuración óptima para implementaciones sin tiempo de inactividad y _evita_ el tiempo de inactividad si se produce un error.

1. [Configurar el vínculo posterior a la implementación](../application/hooks-property.md)
Debe configurar el vínculo posterior a la implementación para limpiar y calentar la caché. De forma predeterminada, la limpieza de caché se produce durante la fase de implementación cuando el sitio está inactivo. Si mueve la caché limpia a la fase posterior a la implementación, significa que la caché permanece activa hasta que se complete la fase de implementación y, a continuación, puede limpiar con seguridad la caché.

   Personalice la lista de páginas utilizadas para precargar la caché con la [variable de entorno WARM_UP_PAGES](../environment/variables-post-deploy.md#warmuppages).

1. [Reducir archivos de temas](../environment/variables-deploy.md#scdmatrix)
Puede reducir el número de archivos de temas innecesarios configurando la variable de entorno SCD\_MATRIX.

1. [Acelere la implementación de contenido estático](../environment/variables-deploy.md#scdthreads)
Puede acelerar el proceso de implementación actualizando la variable de entorno SCD\_THREADS para aumentar el número de subprocesos para la implementación de contenido estático.

>[!NOTE]
>
>Puede validar la configuración de su proyecto para una implementación óptima si [ejecuta el asistente de estado ideal](smart-wizards.md#verifying-an-ideal-configuration).
