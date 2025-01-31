---
title: Preparación para el desarrollo
description: Reúna las credenciales y conozca las herramientas disponibles para configurar un espacio de trabajo de desarrollo para utilizarlo con su proyecto de infraestructura en la nube de Commerce.
recommendations: noDisplay, catalog
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '471'
ht-degree: 0%

---

# Preparación para el desarrollo

Tanto si es su primera vez en Commerce como si es el propietario de un Commerce existente que se traslada a la infraestructura en la nube, siga estos pasos para preparar un espacio de trabajo de desarrollo para su proyecto en la nube. Si ya ha completado algunos de estos pasos o ya tiene un entorno de desarrollo de Adobe Commerce, revise lo siguiente para ver los resultados esperados y continúe con el siguiente paso. Algunas configuraciones y flujos de trabajo difieren de una instalación local típica.

## Credenciales

Antes de configurar un espacio de trabajo, recopile las siguientes claves y acceso a la cuenta:

- **Claves de autenticación (claves de composición)**

  Las claves de autenticación son tokens de autenticación de 32 caracteres que proporcionan acceso seguro al repositorio de Adobe Commerce Composer (`repo.magento.com`) y a cualquier otro servicio Git necesario para el desarrollo de aplicaciones, como GitHub. Su cuenta puede tener varias claves de autenticación. Para la configuración del espacio de trabajo, comience con una clave específica para el repositorio de código. Si no tiene ninguna clave, póngase en contacto con el propietario del proyecto o cree las [claves de autenticación](../cloud-guide/development/authentication-keys.md).

- **Cuenta de proyecto en la nube**

  El propietario del proyecto debe invitarle al proyecto de infraestructura de Adobe Commerce en la nube. Cuando reciba la invitación por correo electrónico, haga clic en el vínculo y siga las indicaciones para crear su cuenta. Consulte [Incorporación](onboarding.md).

- **Clave de cifrado de Adobe Commerce**

  Al importar un sistema existente solamente, capture la clave de cifrado utilizada para proteger el acceso y los datos de la base de datos. Para obtener detalles sobre esta clave, consulte [Resolver problemas con la clave de cifrado](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/resolve-issues-with-encryption-key.html)

## Herramientas para desarrolladores

- **Instalar la CLI de Cloud**

  Instale la CLI `magento-cloud` para poder administrar los entornos de nube y ejecutar las tareas de automatización. Consulte [CLI de nube](../cloud-guide/dev-tools/cloud-cli-overview.md) para obtener instrucciones de instalación.

- **Instalar Docker para desarrollo y pruebas locales**

  De forma opcional, utilice el entorno Docker para emular Commerce en el entorno de la infraestructura en la nube `integration` para el desarrollo local. Hay tres componentes esenciales: una plantilla de Adobe Commerce v2, Docker Compose y `ece-tools` paquete.

   - [Arquitectura de Docker y comandos comunes](../cloud-guide/dev-tools/cloud-docker.md)
   - [Entorno de desarrollo de Launch Docker](https://developer.adobe.com/commerce/cloud-tools/docker/setup/)
   - [Paquete ECE-Tools](../cloud-guide/dev-tools/package-overview.md)

- **Integrar servicios basados en Git**

  Opcionalmente, integre un servicio de alojamiento basado en Git, como GitHub o GitLab, con Adobe Commerce en la infraestructura en la nube. Consulte [Integraciones](../cloud-guide/integrations/overview.md).

## Código de proyecto

Una conexión segura es esencial para interactuar con los entornos remotos. Para un proyecto nuevo, [inicia sesión en [!DNL Cloud Console]](https://console.adobecommerce.com) y haz clic en **[!UICONTROL No SSH key]**. Este icono se encuentra a la derecha del campo de comando y es visible cuando el proyecto no contiene una clave SSH. Consulte [Conexiones seguras](../cloud-guide/development/secure-connections.md#add-an-ssh-public-key-to-your-account).

**Para clonar el código base en la estación de trabajo local**:

1. En [[!DNL Cloud Console]](https://console.adobecommerce.com), haga clic en **[!UICONTROL code]** y seleccione la ficha **[!UICONTROL Git]**.

   ![Clonar el código](../assets/ui-git-code.png){width="450"}

1. Copie el comando `git clone ...` proporcionado.

1. En un terminal, cree y cambie al directorio de trabajo.

1. Pegue y ejecute el comando `git clone ...`.

>[!TIP]
>
>El Adobe aprovisiona el entorno inicial del proyecto mediante un repositorio de plantillas que incluye instrucciones de paquete para una versión específica de Adobe Commerce. Revise el tema [estructura de archivos de proyecto](../cloud-guide/project/file-structure.md) y obtenga más información acerca de los archivos de proyecto y las plantillas de nube importantes.
