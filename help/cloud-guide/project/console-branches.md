---
title: Administrar ramas con  [!DNL Cloud Console]
description: Obtenga información sobre cómo administrar las ramas de entorno para Adobe Commerce en la infraestructura en la nube mediante  [!DNL Cloud Console].
role: Developer
feature: Cloud, Install
exl-id: 2c254586-b670-4dd7-8f82-edcc139e9800
TQID: https://experienceleague.adobe.com/-9EfBaTgSBPQa6HspiaqngBtwURAeUGlNP9hREcXrQQ
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: ba9e5be9-7de1-4f71-a5d2-baead0e425eeid: dac87252-6066-4d6e-a9d2-f6d84c323de7id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
subfeature_v2: id: f8ddfd3b-6194-46e8-a176-0e918039be56
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: d863fc70609dcc66d21eb95e709db80e29114714
workflow-type: tm+mt
source-wordcount: 1638
ht-degree: 0%

---

# Administrar ramas con [!DNL Cloud Console]

Puede administrar sus entornos mediante la CLI [!DNL Cloud Console] o `magento-cloud`. Los archivos de proyecto se almacenan en un repositorio Git. Puede usar comandos Git para administrar su código, pero la CLI de `magento-cloud` está diseñada para interactuar con características de plataforma, mientras que los comandos Git no lo hacen. Consulte [Comandos Git](../dev-tools/cloud-cli-overview.md#git-commands) en el tema de la CLI de la nube.

En este tema se explica cómo usar [!DNL Cloud Console] para:

- Agregar o eliminar un entorno
- Sincronizar (`git pull`) desde el entorno principal
- Combinar (`git push`) con el entorno principal

>[!TIP]
>
>No puede crear ramas desde entornos de ensayo y producción profesionales. Puede bifurcar desde la rama `master`.

## Crear un entorno

La estrategia de ramificación utiliza un flujo de trabajo Git común en el que se desarrolla código y se añaden extensiones en una rama de desarrollo. Ver las descripciones generales de la arquitectura de [Starter](../architecture/starter-architecture.md) y [Pro](../architecture/starter-develop-deploy-workflow.md).

- Para empezar, cree una rama `staging` desde la rama `master` y luego una rama desde `staging` para desarrollo.
- Para Pro, cree una rama de desarrollo a partir del entorno `Integration`.

Su cuenta admite un número limitado de ![ramas activas](../../assets/icon-active.png){width="32"} (activas) y un número ilimitado de ![ramas de desarrollo inactivas](../../assets/icon-inactive.png){width="32"} (inactivas). Administre ramas activas e inactivas agregando o eliminando una rama usando solamente la [!DNL Cloud Console] o la CLI de nube. Antes de eliminar una rama, debe desactivarla, que permanece en la lista _Entornos_ como _inactiva_. Puede reactivar la rama más tarde o [eliminar la rama](../dev-tools/cloud-cli-overview.md#) en la configuración del entorno o mediante la CLI de nube.

Si necesita entornos activos adicionales para el desarrollo, envíe un [ticket de asistencia](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket).

**Para agregar una rama**:

1. Inicie sesión en [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Seleccione un proyecto de la lista _Todos los proyectos_.

1. Seleccione un entorno.

   >[!TIP]
   >
   >La nueva rama se clona desde este entorno. Elija un entorno principal similar al entorno que está a punto de crear.

1. Haga clic en **[!UICONTROL Branch]**.

   ![Crear una rama](../../assets/button-branch.png){width="150"}

1. En el formulario _Bifurcación desde..._, escriba un nombre de rama.

   El entorno _name_ es diferente del entorno _ID_ solo si el nombre del entorno contiene espacios o mayúsculas. Un ID de entorno consta de todas las letras minúsculas, números y símbolos permitidos. Las letras mayúsculas en un nombre de entorno se convierten a minúsculas en el ID.; los espacios en un nombre de entorno se convierten en guiones.

   Un nombre de entorno **no puede** incluir caracteres reservados para su shell de Linux o para expresiones regulares. Los caracteres prohibidos incluyen llaves (`{ }`), paréntesis, asteriscos (`*`), corchetes angulares (`>`), signo ampersand (`&`), porcentaje (<code>%</code>) y otros caracteres.

1. Seleccione un(a) **[!UICONTROL Environment type]**.

1. Haga clic en **[!UICONTROL Create Branch]**.

1. Espere mientras se implementa el entorno.

   Durante la implementación, el estado del entorno es **En proceso**. Después de una implementación correcta, el estado cambia a una marca de verificación verde para **success**.

## Crear rama inactiva

No puede crear una rama inactiva desde la consola o CLI de Adobe Commerce Cloud. Si desea crear una rama inactiva, créela en el repositorio Git y haga una inserción mediante la opción `environment.Parent` del comando.

```bash
git push -o "environment.Parent=<parent branch>" <origin> <branch>
```

## Eliminar un entorno

Antes de poder eliminar un entorno, debe desactivarlo. Una vez que un entorno esté inactivo, puede eliminarlo.

**Para desactivar un entorno**:

1. Inicie sesión en [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Seleccione un proyecto de la lista _Todos los proyectos_.

1. Seleccione el entorno de la lista de la barra de navegación _Entorno_.

1. Haga clic en el icono de configuración en la parte derecha de la barra de navegación superior, que abre la configuración del entorno.

1. En la ficha _[!UICONTROL General]_, desplácese hacia abajo hasta la sección_[!UICONTROL Deactivate environment]_, haga clic en **[!UICONTROL Deactivate environment and delete data]** y siga las instrucciones.

## Sincronizar un entorno

La sincronización de un entorno (o rama) es la misma que `git pull origin <parent>`. Puede sincronizar el código actualizado desde un entorno principal. Puede utilizar esta característica a través de [!DNL Cloud Console] para todos los entornos Starter y Pro.

En el plan Pro, puede sincronizar desde Ensayo y producción a su rama `master`. Esta sincronización solo extrae y inserta código, no datos. Para sincronizar datos, volque los datos de la base de datos y empújelos a la base de datos de otro entorno. Ver [Migrar e implementar datos y archivos estáticos](/help/cloud-guide/deploy/staging-production.md#migrate-static-files).

**Para sincronizar un entorno**:

1. Inicie sesión en [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Seleccione un proyecto de la lista _Todos los proyectos_.

1. En la lista de entornos, haga clic en el nombre de la rama que desea sincronizar.

1. Haga clic en (sincronizar).

   ![Sincronizar un entorno](../../assets/button-sync.png){width="150"}

1. Seleccione los elementos que desea sincronizar.

   - Reemplazar los datos (datos y archivos): sincroniza los cambios de la base de datos y los archivos de contenido de la rama principal.
   - Combinar: (código) sincroniza el código actualizado de la rama principal.

   Esto también genera un comando CLI para que usted copie y utilice.

1. Haga clic en **Sincronizar**.

## Combinar con el entorno principal

Combinar un entorno (o rama) es lo mismo que `git push origin`. Puede combinar para insertar el código actualizado de un entorno a su entorno principal. Puede combinar este código con `master`. Puede implementar en Ensayo y producción utilizando el comando `merge`.

**Para combinar con el entorno principal**:

1. Inicie sesión en [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Seleccione un proyecto de la lista _Todos los proyectos_.

1. En la lista entorno, haga clic en el nombre de la rama que desea combinar.

1. Haga clic en (combinar).

   ![Combinar un entorno](../../assets/button-merge.png){width="150"}

1. Haga clic en **Combinar** y confirme la acción.

## Ver registros

A través de [!DNL Cloud Console], puede revisar varios registros para entornos, incluidos los de compilación, implementación e historial de implementación.

Para **Starter**, puede revisar los registros de compilación e implementación y el historial de implementación. Estos entornos incluyen la rama `master` (Producción) y todas las ramas creadas a partir de ella.

Para **Pro**, puede revisar los siguientes registros en cada entorno:

- Integración: generación e implementación e historial de implementación
- Ensayo: registros de compilación e historial de implementación. Utilice SSH para iniciar sesión en el servidor y ver los registros de implementación.
- Producción: registros de compilación e historial de implementación. Utilice SSH para iniciar sesión en el servidor y ver los registros de implementación.

**Para ver los registros de[!DNL Cloud Console]**:

1. Inicie sesión en [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Seleccione un proyecto de la lista _Todos los proyectos_.

1. Seleccione un entorno.

   La vista de entorno proporciona una [lista de actividades](activity-stream.md) que muestra _eventos recientes_, una entrada por cada acción intentada, incluidas sincronizaciones, combinaciones, ramas, copias de seguridad y más. Haga clic en **Todos** para ver el historial de implementación completo.

1. Para ver el registro de generación, seleccione el vínculo Éxito o Error por registro de implementación en la cuenta.

>[!TIP]
>
>Haga clic en el icono **Filtrar por** en una lista desplegable y seleccione el tipo de mensajes que desea ver.

## Extraer código de un repositorio Git privado

Su proyecto de infraestructura de Adobe Commerce en la nube puede incluir código de un repositorio Git privado. Por ejemplo, puede tener código para un módulo o tema personalizado en un repositorio privado. Para ello, debe agregar la clave SSH pública del proyecto a su repositorio Git privado y actualizar el archivo del proyecto `composer.json`.

Para agregar una clave de implementación al repositorio privado de GitHub, debe ser el administrador de dicho repositorio. GitHub le permite utilizar una clave de implementación solo para un repositorio.

Si prefiere que su proyecto acceda a varios repositorios, puede adjuntar una clave SSH a una cuenta de usuario automatizada. Dado que esta cuenta no la usa un humano, se denomina [usuario de equipo](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/managing-deploy-keys). Agregue la cuenta del equipo como colaborador o agregue el usuario del equipo a un equipo con acceso a los repositorios.

>[!INFO]
>
>Adobe recomienda añadir y combinar este código en los repositorios Git del proyecto. Si no configura la conexión, es posible que experimente problemas de compilación.

**Para encontrar su clave pública SSH**:

1. Inicie sesión en [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Seleccione un proyecto de la lista _Todos los proyectos_.

1. Haga clic en el icono de configuración en la parte derecha de la barra de navegación superior.

1. En _Configuración del proyecto_, haga clic en **[!UICONTROL Deploy Key]**.

1. Copie la clave de implementación en el portapapeles para usarla en uno de los siguientes métodos basados en Git:

>[!BEGINTABS]

>[!TAB GitHub]

### Introduzca la clave de implementación de GitHub

En GitHub, las claves de implementación son de solo lectura de forma predeterminada.

**Para escribir la clave pública del proyecto como clave de implementación de GitHub**:

1. Inicie sesión en el repositorio de GitHub como administrador.
1. Haga clic en la ficha **[!UICONTROL Settings]** del repositorio.

   >[!NOTE]
   >
   >Si no ve esta opción, no ha iniciado sesión como administrador del repositorio y no puede completar esta tarea. Pida al administrador del repositorio de GitHub que lo haga.

1. En la ficha _Configuración_ de la navegación izquierda, haga clic en **[!UICONTROL Deploy Keys]**.
1. Haga clic en **[!UICONTROL Add deploy key]**.
1. Siga las indicaciones.

En `composer.json`, use el formato `<user>@<host>:<.git</code>` o `ssh://<user>@<host>:<port>/<path>.git` si usa un puerto no estándar.

>[!TAB Bits]

### Introduzca la clave de implementación de Bitbucket

**Para escribir la clave pública del proyecto como clave de implementación de bloque de bits**:

1. Inicie sesión en el repositorio de Bitbucket como administrador.
1. En el panel de navegación izquierdo, haga clic en **[!UICONTROL Settings]**.
1. Haga clic en General > **[!UICONTROL Deployment Keys]**.
1. Haga clic en **[!UICONTROL Add Key]**.
1. Siga las indicaciones.

>[!TAB GitLab]

### Introduzca la clave de implementación de GitLab

**Para agregar la clave SSH pública para su proyecto como clave de implementación de GitLab**:

1. Inicie sesión en el repositorio de GitLab como propietario.
1. Compruebe que la opción _Canalizaciones_ esté habilitada para su proyecto:

   1. En la configuración del proyecto, expanda la sección **[!UICONTROL Visibility, project, features, permissions]**.
   1. Si es necesario, haga clic en **[!UICONTROL Pipelines]** para habilitar la opción.

1. Añada la clave SSH pública a la configuración de CI/CD.

   1. En el panel de navegación izquierdo, haga clic en Configuración > **[!UICONTROL CI / CD]**.
   1. Haga clic en Implementar claves **Expand** para configurar la clave.
   1. En el formulario _Implementar clave_, agregue un nombre de clave de implementación al campo **[!UICONTROL Title]** y pegue la clave SSH pública en el campo **[!UICONTROL Key]**.
   1. Haga clic en **[!UICONTROL Add Key]** para guardar la configuración.

>[!ENDTABS]

## Entornos y ramas seguros

Puede acceder a su proyecto y entornos desde cualquier ubicación a través de un explorador web utilizando [!DNL Cloud Console]. Es posible que tenga la seguridad configurada para su entorno de producción, tiendas y sitios. Esta sección le ayuda a proteger los entornos de integración y ensayo estrictamente para desarrolladores, DBA y mucho más.

>[!WARNING]
>
>**NO** usa los siguientes métodos para proteger los entornos de ensayo y producción de Pro. Esto interrumpe el almacenamiento en caché de Fastly. Utilice la función [Bloqueo](../cdn/fastly-vcl-blocking.md) disponible en la CDN de Fastly para Adobe Commerce.

**Para proteger entornos**:

1. Inicie sesión en [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Seleccione un proyecto de la lista _Todos los proyectos_.

1. Seleccione un entorno y haga clic en el icono de configuración en la barra de navegación.

1. En la ficha Configuración del entorno _General_, haga clic en **ACTIVADO** para **[!UICONTROL HTTP access control enabled]** para habilitar el acceso seguro. Puede elegir entre credenciales o direcciones IP para filtrar el acceso.

1. Para filtrar por credenciales, haga clic en **[!UICONTROL Add Login]**, escriba un nombre de usuario y una contraseña y haga clic en **[!UICONTROL Add Login]** para agregar.

1. Para filtrar por dirección IP, escriba las direcciones IP en una lista con `deny` o `allow`. Por ejemplo:

   ```text
   123.456.789.111/29 allow
   123.456.789.112/29 allow
   234.123.567.111/29 allow
   0.0.0.0/0 deny
   ```

1. Haga clic en **[!UICONTROL Save]**. Esto vuelve a implementar el entorno para actualizar la seguridad y la configuración. Adobe recomienda probar el entorno después de completar la configuración de seguridad.

