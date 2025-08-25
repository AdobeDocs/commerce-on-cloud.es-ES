---
title: Redireccionar solicitudes a un servidor de CMS
description: Aprenda a redireccionar las solicitudes entrantes de una tienda de Adobe Commerce a un sitio de WordPress independiente mediante el módulo Fastly edge.
feature: Cloud, Configuration, Routes
exl-id: ef024c68-395b-4d47-9362-a8404a93dbbe
source-git-commit: d08ef7d46e3b94ae54ee99aa63de1b267f4e94a0
workflow-type: tm+mt
source-wordcount: '307'
ht-degree: 0%

---

# Redireccionar solicitudes a un servidor de CMS

Redireccione las solicitudes entrantes de una tienda Adobe Commerce a un sitio de WordPress independiente mediante el módulo Fastly Edge _Otra integración de CMS/back-end_ con un diccionario de Edge. Puede seguir un proceso similar para redireccionar las solicitudes a otros backends de CMS.

Utilice los módulos de Fastly Edge para crear y cargar código VCL personalizado desde el administrador en lugar de escribir manualmente el código VCL y cargarlo mediante la API de Fastly.

>[!NOTE]
>
>Se recomienda añadir configuraciones de VCL personalizadas a un entorno de ensayo en el que pueda probarlas antes de actualizar la configuración del servicio Fastly en el entorno de producción.

**Requisitos previos**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

**Para redireccionar solicitudes de Adobe Commerce a WordPress**:

1. Habilite rápidamente los módulos de Edge en el entorno de ensayo o producción.

   - Inicie sesión en Admin.

   - Vaya a **Tiendas** > Configuración > **Configuración** > **Avanzado** > **Sistema** > **Caché de página completa** > **Configuración rápida** > **Configuración avanzada**.

   - Establezca el valor de **Módulos Edge de Fastly** en **Sí**.

   - Guarde la configuración.

1. Identifique las rutas URL para redireccionar al backend de WordPress.

1. Complete las siguientes tareas para configurar el servicio Fastly y crear el código VCL personalizado para redireccionar solicitudes al back-end de WordPress.

   - Cree un diccionario de Edge que especifique las rutas para volver a enrutar desde el almacén de Adobe Commerce al servidor.

   - Agregue el servidor de WordPress a la configuración del servicio Fastly y adjunte la condición de solicitud para las reescrituras de URL.

   - Configure el módulo _Other CMS/backend integration_ de Edge para administrar las reescrituras de URL de Adobe Commerce al backend de WordPress.

     Para obtener instrucciones detalladas, consulte [Módulos Edge de Fastly: otra integración de CMS/back-end](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/Edge-Modules/EDGE-MODULE-OTHER-CMS-INTEGRATION.md) en el módulo CDN de _Fastly para la documentación de Magento 2_.

1. Después de actualizar la configuración del servicio Fastly, pruebe la tienda de Adobe Commerce para asegurarse de que las solicitudes de URL especificadas para WordPress se redireccionan correctamente.

<!-- Last updated from includes: 2025-01-27 17:16:28 -->
