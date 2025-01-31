---
title: Resumen de integraciones
description: Obtenga información acerca de las opciones de integración de terceros para su proyecto de Adobe Commerce en la nube.
role: Developer
feature: Cloud, Integration
last-substantial-update: 2024-02-06T00:00:00Z
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '564'
ht-degree: 0%

---

# Resumen de integraciones

Las integraciones son útiles para utilizar servicios externos, como alojamiento de Git o bots de Slack, y para mantener los procesos de desarrollo actuales, como el uso de la función de solicitud de extracción de revisión de código en GitHub. Puede añadir las siguientes integraciones a su proyecto de infraestructura de Adobe Commerce en la nube:

![Integraciones](/help/assets/integrations.png)

>[!BEGINTABS]

>[!TAB CLI]

**Para agregar una integración usando la CLI de nube**:

El siguiente comando inicia las indicaciones interactivas para seleccionar el tipo y las opciones de la nueva integración.

```bash
magento-cloud integration:add
```

**Para enumerar las integraciones configuradas para su proyecto**:

```bash
magento-cloud integration:list
```

Respuesta de ejemplo:

```
+----------+--------------+---------------------------------------------------------------------------+
| ID       | Type         | Summary                                                                   |
+----------+--------------+---------------------------------------------------------------------------+
| <int-id> | bitbucket    | Repository: user/magento-int                                              |
|          |              | Hook URL:                                                                 |
|          |              | https://magento-url.cloud/api/projects/projectID/integrations/int-ID/hook |
| <int-id> | health.email | From: you@example.com                                                     |
|          |              | To: them@example.com                                                      |
+----------+--------------+---------------------------------------------------------------------------+
```

>[!TAB Consola]

**Para agregar una integración usando[!DNL Cloud Console]**:

1. En _Configuración del proyecto_, haga clic en **[!UICONTROL Integrations]**.

1. Haga clic en un tipo de integración o en **[!UICONTROL Add integration]**.

1. Siga los pasos de configuración y selección del tipo de integración.

1. Después de agregar la integración, esta aparece en la lista de la vista Integraciones.

>[!ENDTABS]

## Webhooks de Commerce

Puede configurar los webhooks de Commerce en su proyecto de Cloud con la [variable global ENABLE_WEBHOOKS](../environment/variables-global.md#enable_webhooks). Los enlaces web de Commerce envían solicitudes a un servidor externo en respuesta a eventos generados por Commerce. La [_Guía de Webhooks_](https://developer.adobe.com/commerce/extensibility/webhooks) describe esta característica en detalle.

## Webhooks genéricos

Puede capturar y crear informes sobre los eventos del repositorio e infraestructura de la nube mediante una integración de gancho web personalizada en `POST` mensajes JSON a una URL de _gancho web_.

**Para agregar una URL de gancho web, use la siguiente sintaxis**:

```bash
magento-cloud integration:add --type=webhook --url=https://hook-url.example.com
```

- `type`: especifique el tipo de integración `webhook`.
- `url`: proporcione la dirección URL del gancho web que puede recibir mensajes JSON.

La respuesta de ejemplo muestra una serie de mensajes que proporcionan una oportunidad para personalizar la integración. Si se utiliza la respuesta predeterminada (en blanco), se envían mensajes sobre todos los eventos de todos los entornos de un proyecto.

Puede personalizar la integración para que informe de [eventos](#events-to-report) específicos, como la inserción de código en una rama. Por ejemplo, puede especificar el evento `environment.push` para enviar un mensaje cuando un usuario inserte código en una rama:

```
Events to report (--events)
A list of events to report, e.g. environment.push
Default: *
Enter comma-separated values (or leave this blank)
>
```

Puede elegir informar sobre eventos en un estado `pending`, `in_progress` o `complete`:

```
States to report (--states)
A list of states to report, e.g. pending, in_progress, complete
Default: complete
Enter comma-separated values (or leave this blank)
>
```

Y puede _incluir_ o _excluir_ mensajes para entornos específicos:

```
Included environments (--environments)
The environment IDs to include
Default: *
Enter comma-separated values (or leave this blank)
>

Excluded environments (--excluded-environments)
The environment IDs to exclude
Enter comma-separated values (or leave this blank)
>
```

Cuando se complete la integración, recibirá un resumen de los valores:

```
Created integration integration-ID (type: webhook)
+-----------------------+------------------------------+
| Property              | Value                        |
+-----------------------+------------------------------+
| id                    | integration-ID               |
| type                  | webhook                      |
| events                | - '*'                        |
| environments          | - '*'                        |
| excluded_environments | {  }                         |
| states                | - complete                   |
| url                   | https://hook-url.example.com |
+-----------------------+------------------------------+
```

### Actualizar integración existente

Puede actualizar una integración existente. Por ejemplo, cambie los estados de `complete` a `pending` mediante lo siguiente:

```bash
magento-cloud integration:update --states=pending <int-id>
```

Respuesta de ejemplo:

```
Integration integration-ID (webhook) updated
+-----------------------+------------------------------+
| Property              | Value                        |
+-----------------------+------------------------------+
| id                    | integration-ID               |
| type                  | webhook                      |
| events                | - '*'                        |
| environments          | - '*'                        |
| excluded_environments | {  }                         |
| states                | - pending                    |
| url                   | https://hook-url.example.com |
+-----------------------+------------------------------+
```

### Eventos para informar

| Evento | Descripción |
| ----- | :-----------|
| `environment.access.add` | Se ha concedido acceso al entorno a un usuario |
| `environment.access.remove` | Se ha eliminado un usuario del entorno |
| `environment.activate` | Se ha &quot;activado&quot; una rama con un entorno |
| `environment.backup` | Un usuario ha activado una instantánea |
| `environment.branch` | Se ha creado una rama con la consola de administración |
| `environment.deactivate` | Se ha &quot;desactivado&quot; una rama. El código sigue ahí, pero el medio ambiente fue destruido |
| `environment.delete` | Se ha eliminado una rama |
| `environment.initialize` | La rama `master` del proyecto se inicializó con una primera confirmación |
| `environment.merge` | Se ha fusionado una rama activa mediante la consola de administración o la API |
| `environment.push` | Un usuario insertó código en una rama |
| `environment.restore` | Un usuario restauró una instantánea |
| `environment.route.create` | Se ha creado una ruta mediante la consola de administración |
| `environment.route.delete` | Se ha eliminado una ruta mediante la consola de administración |
| `environment.route.update` | Se ha modificado una ruta mediante la consola de administración |
| `environment.subscription.update` | Se ha cambiado el tamaño del entorno `master` porque la suscripción ha cambiado, pero no hay cambios en el contenido |
| `environment.synchronize` | Se han copiado datos o código de un entorno desde su entorno principal |
| `environment.update.http_access` | Se han modificado las reglas de acceso HTTP de un entorno |
| `environment.update.restrict_robots` | Se ha habilitado o deshabilitado la función de bloquear todos los robots |
| `environment.update.smtp` | Se ha habilitado o deshabilitado el envío de correos electrónicos para un entorno |
| `environment.variable.create` | Se ha creado una variable |
| `environment.variable.delete` | Se ha eliminado una variable |
| `environment.variable.update` | Se ha modificado una variable |
| `project.domain.create` | Se ha creado un dominio y se ha agregado al proyecto |
| `project.domain.delete` | Se ha eliminado un dominio asociado con el proyecto |
| `project.domain.update` | Se ha actualizado un dominio asociado al proyecto |
