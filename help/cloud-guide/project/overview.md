---
title: Proyecto de infraestructura en nube
description: Lea información general sobre Adobe Commerce en la infraestructura en la nube [!DNL Cloud Console] y aprenda a acceder a la configuración de la cuenta.
last-substantial-update: 2024-02-06T00:00:00Z
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '987'
ht-degree: 0%

---

# Proyecto de infraestructura en nube

El proyecto de Adobe Commerce en la nube incluye todo el código de las ramas de Git, los entornos asociados y los scripts para implementar la aplicación [!DNL Commerce]. Los entornos contienen servicios para admitir la aplicación [!DNL Commerce], incluida una base de datos, un servidor web y un servidor de almacenamiento en caché.

El Adobe proporciona [!DNL Cloud Console] y herramientas para desarrolladores para administrar por completo todos los aspectos del proyecto. Usted, como propietario de la cuenta, tiene acceso completo a todos los entornos.

## [!DNL Cloud Console]

[!DNL Cloud Console] proporciona métodos interactivos para generar, administrar e implementar código Commerce en un formato fácil de usar. [Inicia sesión en [!DNL Cloud Console]](https://console.adobecommerce.com) para ver tu lista de proyectos. Solo puede ver los proyectos a los que tiene permiso para acceder como administrador o para tipos de entorno específicos. Si es socio de soluciones de Adobe, es posible que vea varios proyectos para clientes que admite.

>[!TIP]
>
>Si no ve ningún proyecto, debe comunicarse con el [Propietario de cuenta o administrador del proyecto](../project/user-access.md) asociado al proyecto y solicitar acceso. Para los usuarios nuevos, vea el [tema de incorporación](../../get-started/onboarding.md#cloud-console) en la guía de _introducción_.

La vista _Todos los proyectos_ enumera todos los proyectos para los que tiene permiso de acceso. Puede hacer clic en **[!UICONTROL Show filters]** y filtrar la lista de proyectos por tipo, región o plan.

![Lista de proyectos](../../assets/ui-allprojects-list.png)

### Resumen del proyecto

Al seleccionar un proyecto de la lista _Todos los proyectos_, se abre la descripción general del proyecto. La información general del proyecto siempre muestra una barra de navegación del proyecto, que incluye un selector de entorno y un botón de configuración:

![Navegación de proyecto](../../assets/project-nav.png)

La descripción general del proyecto, siempre que no tenga un entorno seleccionado, muestra un resumen de los detalles del proyecto en el área de vista previa:

- Nombre del proyecto
- Región, ID de proyecto
- Planificar, almacenamiento asignado, entornos, usuarios
- URL de tienda con botón **[!UICONTROL Set a custom domain]**

Y en la descripción general del proyecto principal:

- La vista Entornos muestra una lista o vista de árbol de ![entornos de rama activos](../../assets/icon-active.png){width="32"} (active) and ![inactive branch](../../assets/icon-inactive.png){width="32"} (inactivos).
- [Flujo de actividad](activity-stream.md) muestra actividades en ejecución, pendientes y recientes para el proyecto.
<!-- - Apps & Services—Shows a topology of service containers -->

Para **Proyectos iniciales**, hay una jerarquía de ramas a partir de `master` (Producción). Cualquier rama que cree se mostrará como rama secundaria de la rama `master`. El Adobe recomienda crear una rama `staging` y luego crear una rama `integration` para desarrollo. Consulte [Arquitectura inicial](../architecture/starter-architecture.md).

Para **Pro**, hay una jerarquía de ramas que comienza desde `production` hasta `staging` hasta `integration`. El icono ![Icono dedicado](../../assets/icon-dedicated.png){width="32"} indica que la rama se implementa en un entorno dedicado. Todas las ramas que cree se mostrarán como secundarias de la rama `integration`. Consulte [Arquitectura profesional](../architecture/pro-architecture.md).

![Lista de entornos profesionales](../../assets/pro-environments.png)

### Resumen del entorno

Al seleccionar un entorno de la barra de navegación del proyecto, se cambia la descripción general y la barra de navegación para centrarse en el entorno seleccionado. La barra de navegación incluye controles de bifurcación (bifurcación, combinación y sincronización) y un botón de configuración:

![Entorno seleccionado](../../assets/environment-selected.png)

La descripción general del entorno muestra un resumen de los detalles del entorno en el área de vista previa:

- Nombre del entorno, tipo
- Región, ID de proyecto
- Fecha y hora de la última actividad, incluida la copia de seguridad
- Acceso HTTP y estado del motor de búsqueda
- Nombre de equipo asignado al entorno
- Estado del entorno (activo o inactivo)
- URL de tienda con botón **[!UICONTROL Set a custom domain]**

Y en la descripción general del entorno principal:

- [Secuencia de actividades](activity-stream.md) constituye la información general del entorno principal y muestra las actividades en ejecución, pendientes y recientes para el entorno seleccionado.
<!-- - Services tab shows and Apps & Services menu, including overview and configuration tabs for each service. -->
- [Ficha Copias de seguridad](../storage/snapshots.md#create-a-manual-backup) proporciona una lista de las copias de seguridad almacenadas, el historial de acciones de copia de seguridad y el botón Copia de seguridad.

### Acceder a tienda

Cada entorno activo tiene una tienda. Seleccione un entorno en la barra de navegación superior y haga clic en la URL en la descripción general del entorno. Además, hay una lista **[!UICONTROL URLs]** en el lado derecho sobre la lista Actividad.

La dirección URL de acceso web puede incluir lo siguiente:

```
https://<branch>-<unique-ID>-<project-ID>.<region>.magentosite.cloud/
```

- **ID único** = 7 caracteres alfanuméricos aleatorios
- **Id. de proyecto** = Id. de proyecto de 13 caracteres
- **Región** = nombre de región de AWS o Azure, consulte [Direcciones IP regionales](regional-ip-addresses.md)

Los entornos de producción y ensayo de Pro incluyen tres nodos a los que puede acceder mediante los siguientes vínculos:

- URL del equilibrador de carga:

   - `http[s]://<your-domain>.c.<project-ID>.ent.magento.cloud`
   - `http[s]://<your-staging-domain>.c.<project-ID>.ent.magento.cloud`

- Acceso directo a uno de los tres servidores redundantes:

   - `http[s]://<your-domain>.{1|2|3}.<project-ID>.ent.magento.cloud`
   - `http[s]://<your-staging-domain>.{1|2|3}.<project-ID>.ent.magento.cloud`

  La red de entrega de contenido (CDN) utiliza la URL de producción.

## Configuración

Abra el panel _Configuración_ haciendo clic en el icono ![configurar proyecto](../../assets/icon-configure.png){width="36"} (configurar) que se encuentra a la derecha de la navegación del proyecto.

### Configuración del proyecto

**[!UICONTROL Project Settings]** amplía un menú de controles de nivel de proyecto para administrar usuarios, variables y mucho más:

| Opción | Descripción |
|--------------|-------------------------------------------------------------------------------------------------------------------------------|
| General | Administre la zona horaria para utilizarla con la programación de copias de seguridad o el mantenimiento. |
| Acceso | Administrar [acceso de usuario](user-access.md) a los tipos de proyecto y entorno. |
| Certificados | Vea una lista de los certificados SSL asociados al proyecto. |
| Implementar clave | Agregue y vea la clave pública al repositorio de código del proyecto. |
| Domains | Agregue un nombre de dominio al proyecto. Consulte [Administrar dominios](../cdn/fastly-custom-cache-configuration.md#manage-domains). |
| Integraciones | Agregar y administrar [integraciones](../integrations/overview.md), como notificaciones de mantenimiento y enlaces web. |
| Variables | Agregue [variables de nivel de proyecto](../environment/variable-levels.md) que estén disponibles durante la compilación y el tiempo de ejecución en todos los entornos. |

{style="table-layout:auto"}

### Configuración del entorno

Haga clic en **[!UICONTROL Environments]** y seleccione un entorno específico en la lista de controles para administrar la configuración del sitio, las variables de entorno, etc.:

| Opción | Descripción |
| --------- | -------------------------------------------------------------------------------------------------------------------------------- |
| General | Configure el nombre para mostrar, el tipo de entorno y el entorno principal.<br>Alternar diferentes configuraciones de entorno: |
|           | **Habilitar correos electrónicos salientes**: envíe [correos electrónicos salientes](outgoing-emails.md) desde el entorno mediante el protocolo SMTP. |
|           | **Ocultar de los motores de búsqueda**: bloquee los indexadores y rastreadores de motores de búsqueda del sitio. |
|           | **Control de acceso HTTP**: habilite la configuración de seguridad para [!DNL Cloud Console] mediante el inicio de sesión y el control de acceso a la dirección IP. |
|           | El estado es `active` o `inactive`. La mayor parte de su trabajo se realiza en un entorno activo. Puede desactivar o eliminar el entorno. |
| Variables | Ver, crear y administrar [variables de nivel de entorno](../environment/variable-levels.md) disponibles durante la ejecución. |
| Domains | Ver una lista de [rutas configuradas](../routes/routes-yaml.md). |

{style="table-layout:auto"}

>[!WARNING]
>
>**NO** usa el método de control de acceso HTTP para proteger los entornos de ensayo y producción de Pro. Esto interrumpe el almacenamiento en caché de Fastly. En su lugar, use la característica [Bloqueo](../cdn/fastly-vcl-blocking.md) disponible en la CDN de Fastly para Adobe Commerce.

## Credenciales de Fastly y New Relic

Tu proyecto incluye [Fastly](../cdn/fastly.md) y [New Relic](../monitor/new-relic-service.md). Los detalles del proyecto muestran información sobre el plan del proyecto y las licencias y tokens importantes para estas integraciones. Solo el propietario de la licencia tiene acceso inicial a las credenciales y los servicios. Proporcione estas credenciales a los recursos técnicos y de desarrollador que necesite.

- [Fastly](https://www.fastly.com/) proporciona servicios de entrega de contenido (CDN), optimización de imágenes y seguridad (DDoS y WAF) para su Adobe Commerce en proyectos de infraestructura en la nube. Ver [Obtener credenciales de Fastly](../cdn/fastly-configuration.md#get-fastly-credentials).

- [New Relic](../monitor/new-relic-service.md) proporciona métricas de aplicación e información de rendimiento para los entornos de ensayo y producción.

Use la [CLI de nube](../dev-tools/cloud-cli-overview.md) para revisar sus tokens de integración, ID y mucho más:

```bash
magento-cloud subscription:info services
```
