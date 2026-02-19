---
title: Administración de cuentas de New Relic
description: Obtenga información sobre cómo acceder a su cuenta de New Relic y administrar el acceso, las integraciones y el uso de las herramientas de su proyecto de Adobe Commerce en la nube.
feature: Cloud, Observability
role: Admin
exl-id: 7aeedd12-7a81-47eb-a82f-3079e16ecb06
source-git-commit: 558c645e353e38ce8455ef17e1d0e9fa99b22c6e
workflow-type: tm+mt
source-wordcount: '790'
ht-degree: 0%

---

# Administración de cuentas de New Relic

Cuando Adobe aprovisiona el proyecto de infraestructura en la nube, el propietario de la licencia recibe un correo electrónico de New Relic con credenciales e instrucciones para acceder a la cuenta de New Relic. Si no ha recibido el correo electrónico, utilice la dirección de correo electrónico del propietario de la licencia para restablecer la contraseña de New Relic.

Si el propietario de la licencia ha cambiado y el nuevo propietario de la licencia no tiene acceso a New Relic en este momento, [envíe un ticket de asistencia de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=es#submit-ticket).

## Administrar el acceso de los usuarios (función de administrador)

>[!NOTE]
>
>Conceda únicamente acceso completo a los usuarios que requieran estrictamente acceso al conjunto completo de funciones.

**Para tener acceso a la administración de usuarios en New Relic**:

1. Inicie sesión en su [cuenta de New Relic](https://login.newrelic.com/login).

1. Seleccione su nombre de usuario en la barra de navegación inferior izquierda.

1. Haga clic en **[!UICONTROL Administration]** y seleccione una de las siguientes opciones de la lista:

   - **[!UICONTROL User management]** para agregar un usuario y administrar usuarios activos e invitaciones pendientes.

   - **[!UICONTROL Access management]** para administrar grupos de usuarios, roles y cuentas.

Consulte [Administración de usuarios](https://docs.newrelic.com/docs/accounts/accounts-billing/new-relic-one-user-management/user-management-ui-and-tasks/) en la documentación de _New Relic_.

## Configuración de New Relic para el entorno de inicio

>[!NOTE]
>
>**Los entornos profesionales** están preconfigurados para usar los servicios de New Relic y pueden omitir las instrucciones de activación y conexión. Si New Relic APM no está instalado en los entornos de ensayo y producción o New Relic Infrastructure no está disponible en el entorno de producción, [envíe un ticket de asistencia de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=es#submit-ticket) para solicitar la instalación.

En entornos iniciales, debe comprobar el archivo `.magento.app.yaml` para comprobar que la sección `runtime` incluye la extensión de New Relic. Si la extensión no se ha configurado, agregue lo siguiente:

> `.magento.app.yaml`

```yaml
runtime:
    extensions:
        - newrelic
```

### Aplicar clave de licencia

Para conectar un entorno de nube a New Relic, añada la clave de licencia de New Relic al entorno.

- Para **proyectos Pro**, Adobe agrega la clave de licencia a los entornos de Producción y Ensayo durante el proceso de aprovisionamiento. Puede iniciar sesión en su [cuenta de New Relic](https://login.newrelic.com/login) para comprobar la conectividad entre su sitio de Adobe Commerce en la infraestructura de la nube y New Relic.

- Para **Proyectos iniciales**, tiene una clave de licencia de New Relic que admite hasta _tres_ entornos. Debe agregar la clave a las configuraciones de entorno manualmente. Los entornos iniciales no están preaprovisionados para utilizar el servicio de New Relic.

Para entornos iniciales, habilite la integración de New Relic agregando la clave de licencia de New Relic a la configuración del entorno. Agregue la clave a los entornos de Ensayo y Producción y a otro entorno de su elección. Solo se requiere la clave de licencia de New Relic para la configuración. Puede encontrar información sobre opciones de configuración adicionales en el tema [Informes de New Relic](https://experienceleague.adobe.com/docs/commerce-admin/config/general/new-relic-reporting.html?lang=es) en la _Guía del usuario de Adobe Commerce_.

{{redeploy-warning}}

>[!PREREQUISITES]
>
>- Credenciales de inicio de sesión para la página de cuenta de Adobe Commerce o para la licencia de New Relic asociada a su proyecto
>- [Acceso de administrador](../project/user-access.md) a los entornos de inicio para configurar
>- Credenciales para acceder a [Admin](https://experienceleague.adobe.com/docs/commerce-admin/systems/user-accounts/permissions.html?lang=es) para el entorno

**Para configurar New Relic para entornos iniciales**:

1. Busque su clave de licencia de New Relic en [!DNL Cloud Console] o en la CLI de nube.

   **[!DNL Cloud Console]método**:

   - Abra el proyecto en la nube [página de cuenta](https://accounts.magento.cloud/user).

   - En la ficha _Proyectos_, busque su proyecto.

   - Haga clic en **Ver detalles** para obtener información sobre la infraestructura del proyecto.

   - Expanda la sección **Servicio de New Relic** para ver la clave de licencia.

   - Copie la clave de licencia.

   **Método CLI de nube**:

   ```bash
   magento-cloud subscription:info services.newrelic
   ```

1. Agregue la clave de licencia de New Relic a un entorno utilizando la CLI `magento-cloud`.

   - Cambie al entorno que necesita la clave de licencia.
   - Actualice el valor de la variable mediante el siguiente comando CLI `magento-cloud`:

     ```bash
     magento-cloud variable:update php:newrelic.license --value <newrelic-license-key>
     ```

   Opcionalmente, puede agregarlo desde [Commerce Admin](https://experienceleague.adobe.com/docs/commerce-admin/start/reporting/new-relic-reporting.html?lang=es#step-3%3A-configure-your-store).

1. Inicie sesión en su [cuenta de New Relic](https://login.newrelic.com/login) para comprobar que puede ver datos desde el entorno de Adobe Commerce. Ver [Investigar rendimiento](investigate-performance.md).

### Quitar clave de licencia

Solo puede utilizar su clave de licencia de New Relic en tres entornos activos. Si la clave está en uso en tres entornos, debe eliminarla de uno de ellos para poder agregarla a otro.

**Para quitar una clave de licencia de un entorno**:

1. Enumerar variables de entorno.

   ```bash
   magento-cloud variable:list
   ```

   Respuesta de ejemplo:

   ```
    +----------------------+-------------+----------------------+---------+
    | Name                 | Level       | Value                | Enabled |
    +----------------------+-------------+----------------------+---------+
    | php:newrelic.license | environment | newrelic-license-key | true    |
    +----------------------+-------------+----------------------+---------+
   ```

   >[!WARNING]
   >
   >Si agregó la clave de licencia como una variable de _proyecto_, debe quitar esa variable de nivel de proyecto. Una variable de proyecto agrega la licencia a _cada_ rama de entorno creada, la cual puede consumir o exceder el límite de licencia. Para enumerar variables de proyecto: `magento-cloud variable:list --level project`

1. Elimine la variable de licencia.

   ```bash
   magento-cloud variable:delete php:newrelic.license
   ```

## Cambiar propietario de cuenta para New Relic en la nube

Para cambiar el propietario de la cuenta de New Relic de su proyecto de Adobe Commerce en la nube:

1. **Cambiar el propietario** en la interfaz de usuario de New Relic. Consulte [Cambiar el propietario de la cuenta](https://docs.newrelic.com/docs/accounts/accounts-billing/new-relic-one-user-management/account-user-mgmt-tutorial/) en la documentación de New Relic.

2. **Agregue primero al usuario** si aún no está en su cuenta. Consulte [Agregar y actualizar usuarios](https://docs.newrelic.com/docs/accounts/accounts-billing/new-relic-one-user-management/user-management-ui-and-tasks/#add-users) en la documentación de New Relic.

3. **¿Necesita ayuda?** Si ningún propietario o administrador existente puede ayudar, cualquier usuario de Adobe Commerce con acceso a la [cuenta de propietario de la asociación con Adobe Commerce](https://account.newrelic.com/accounts/1311131/users) podrá agregar usuarios en su nombre.

Para obtener más información, consulte la [descripción general del servicio New Relic](https://experienceleague.adobe.com/es/docs/commerce-cloud-service/user-guide/monitor/new-relic/new-relic-service).
