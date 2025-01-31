---
title: Iniciar sesión en  [!DNL Cloud Console]
description: Obtenga información acerca de  [!DNL Cloud Console] para Adobe Commerce en la infraestructura en la nube.
recommendations: noDisplay, catalog
last-substantial-update: 2024-02-06T00:00:00Z
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '389'
ht-degree: 0%

---


# Iniciar sesión en [!DNL Cloud Console]

[!DNL Cloud Console] proporciona métodos interactivos para generar, administrar e implementar código Commerce. [!DNL Cloud Console] es una experiencia más moderna y fácil de usar que sentará las bases para futuras mejoras en la interfaz.

[Inicia sesión en [!DNL Cloud Console]](https://console.adobecommerce.com) para ver tu lista de proyectos.

![Lista de proyectos](../assets/ui-allprojects-list.png)

## Funciones

Las funciones nuevas o mejoradas incluyen:

- Información general clara sobre las características del proyecto y del entorno
- Flujo de actividad con historial ordenable
- Administración manual de copias de seguridad e historial para proyectos iniciales
- Vistas de registro mejoradas
- Listas ordenables
- Formularios sencillos y directrices para agregar integraciones
- Directrices de accesibilidad del contenido web (WCAG)

![[!DNL Cloud Console]](../assets/CloudConsole.svg)

Las funciones nuevas o mejoradas son las siguientes:

| Función | Mejoras |
| -------------- | ----------------------------------- |
| [Flujo de actividad](../cloud-guide/project/activity-stream.md) | Interactúe con una lista ordenable de acciones en ejecución, pendientes o históricas. Seleccione una actividad y vea los registros o cancele una compilación en ejecución. |
| [Información general sobre proyectos y entornos](../cloud-guide/project/overview.md#project-overview) | Abra el proyecto y vea la descripción general de los detalles del proyecto y la lista de entornos. La descripción general del entorno proporciona más detalles sobre el estado del entorno, el acceso a la aplicación y las actividades recientes. |
| [Formularios de integración](../cloud-guide/integrations/overview.md) | Utilice formularios y directrices sencillos para agregar integraciones, como Bitbucket o notificaciones a Slack. |
| [Lista de proyectos](../cloud-guide/project/overview.md#cloud-console) | La vista _Todos los proyectos_ enumera todos los proyectos para los que tiene permiso de acceso. Puede hacer clic en **[!UICONTROL Show filters]** y filtrar la lista de proyectos por tipo, región o plan. |
| [Opciones de visibilidad de variables](../cloud-guide/environment/variable-levels.md) | Limite la visibilidad de una variable de nivel de proyecto o de nivel de entorno durante la compilación o el tiempo de ejecución. |

<!-- The following are features yet to be activated:
| **Apps and services topology** | The Apps & Services topology is visible on Project and Environment views. This interactive diagram allows you to select a service and view the relationship details, such as name, type, version, port, and more. Click **[!UICONTROL View details]** to access the overview and configuration panel for each service. | -->

## Preguntas de consola

**_Dónde puedo encontrar la característica Instantáneas_**?

Para [!DNL Starter] proyectos, la característica Instantáneas ahora se llama _Copias de seguridad_. Puede crear una copia de seguridad manual de su entorno [!DNL Starter] desde [!DNL Cloud Console] o crear una instantánea desde la CLI de la nube. Debe tener una función de administrador para el entorno.

Seleccione un entorno de la barra de navegación del proyecto. El entorno debe estar activo. Seleccione la ficha **[!UICONTROL Backups]**. Actualmente, esta opción no está disponible para entornos Pro.

**_¿Dónde está la lista de rutas configuradas para el entorno_**?

Puede encontrar la lista de rutas configuradas en la ficha _Servicios_ para un entorno.

Seleccione un entorno de la barra de navegación del proyecto. Seleccione la ficha **[!UICONTROL Services]**. La descripción general de **Router** muestra las rutas configuradas. Actualmente, no puede agregar una ruta desde el nuevo(a) [!DNL Cloud Console].

## Menú Cuenta

En la esquina superior derecha se encuentra el menú de la cuenta. Haga clic en la flecha hacia abajo del menú y seleccione **[!UICONTROL My Profile]**. En la vista _Mi perfil_, puede controlar los detalles del usuario y la configuración de pantalla, administrar [autenticación de seguridad](../cloud-guide/project/user-access.md#user-authentication-requirements), [tokens de API](../cloud-guide/project/user-access.md#create-an-api-token) y [claves SSH](../cloud-guide/development/secure-connections.md).
