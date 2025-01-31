---
title: Personalización de páginas de error y mantenimiento
description: Aprenda a personalizar la página de error predeterminada que se muestra cuando fallan las solicitudes al servidor de origen de Fastly.
feature: Cloud, Configuration, Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '797'
ht-degree: 0%

---

# Personalización de páginas de error y mantenimiento

Cuando falla una solicitud al origen de Fastly, devuelve páginas de respuesta predeterminadas con formato básico y mensajería genérica que pueden ser confusas para los usuarios. Por ejemplo, Fastly devuelve la siguiente página de error predeterminada cuando una solicitud al origen de Fastly falla debido a un error 503.

![Página de error predeterminada rápida](../../assets/cdn/fastly-503-example.png)

Puede actualizar la configuración de la tienda Adobe Commerce para reemplazar algunas páginas de respuesta predeterminadas por páginas que tengan mensajes más descriptivos y un estilo de HTML mejorado, como se muestra en el siguiente ejemplo.

![Página de error personalizada rápida](../../assets/cdn/fastly-new-error-page.png)

Actualmente, puede personalizar las siguientes páginas de respuesta rápida para su proyecto de Adobe Commerce en la nube.

- [Errores del servidor: errores internos del servidor, tiempo de espera o interrupciones de mantenimiento del sitio (código de error 500 o superior)](#customize-the-503-error-page)
- [Eventos de bloqueo de WAF que se producen cuando WAF detecta tráfico de solicitud sospechoso (403 Prohibido)](#customize-the-waf-error-page)

**Requisitos de codificación de HTML:**

El código de HTML de la página personalizada debe cumplir los siguientes requisitos:

- El contenido puede contener hasta 65.535 caracteres.
- Especifique todos los CSS en línea en el origen del HTML.
- Agrupe imágenes en la página del HTML utilizando base64 para que se muestren aunque Fastly esté sin conexión. Ver [URI de datos en el sitio de css-tricks](https://css-tricks.com/data-uris/).

## Personalización de la página de error 503

Los clientes ven la página de error 503 predeterminada en los siguientes casos:

- Cuando una solicitud al origen Fastly devuelve un estado de respuesta mayor que 500
- Cuando el origen de Fastly está inactivo, como un tiempo de espera, una actividad de mantenimiento o problemas de estado

Puede personalizar la página predeterminada adaptando el siguiente código de HTML para incluir el estilo que coincida con la temática de la tienda Adobe Commerce y modificando el título y los mensajes según sea necesario.

```html
<!DOCTYPE html>
<html>
   <head>
      <meta charset="UTF-8">
         <title>503</title>
   </head>
   <body>
      <p>Service unavailable</p>
   </body></html>
```

Compruebe que el origen modificado se muestra correctamente en el explorador. A continuación, añada el código de HTML personalizado a la configuración de Fastly.

Para agregar la página de respuesta personalizada a la configuración de Fastly:

{{admin-login-step}}

1. Seleccione **Tiendas** > **Configuración** > **Configuración** > **Avanzada** > **Sistema**.

1. En el panel derecho, expanda **Caché de página completa** > **Configuración rápida** > **Páginas sintéticas personalizadas**.

   ![Editar página de error 503](../../assets/cdn/fastly-custom-synthetic-pages-edit-html.png)

1. Seleccione **Establecer HTML**.

1. Copie y pegue el código fuente de la página de respuesta personalizada en el campo HTML.

   ![Página de error de actualización 503](../../assets/cdn/fastly-customize-503-response.png)

1. Seleccione **Cargar** en la parte superior de la página para cargar el origen del HTML personalizado en el servidor de Fastly.

1. Seleccione **Guardar configuración** en la parte superior de la página para guardar el archivo de configuración actualizado.

1. Actualice la caché.

   - En la notificación que aparece en la parte superior de la página, seleccione el vínculo *Administración de caché*.

   - En la página Administración de caché, seleccione **Vaciar caché del Magento**.

## Personalizar la página de errores de WAF

Los clientes ven la siguiente página de error predeterminada de WAF cuando una solicitud al origen de Fastly falla con un error `403 Forbidden` causado por un evento de bloqueo de [WAF](fastly-waf-service.md).

![Página de error de WAF](../../assets/cdn/fastly-waf-403-error.png)

El siguiente ejemplo de código muestra el origen del HTML para la página predeterminada:

```html
<html>
  <head>
    <title>Magento 403 Forbidden</title>
  </head>
  <body>
    <p>The requested URL was rejected.</p>
    <p>For additional information, please contact support and provide this reference ID:</p>
    <p>"} req.http.x-request-id {"</p>
    <p><button onclick='history.back();'>Go Back</button></p>
  </body>
</html>
```

Puede usar la opción **Páginas sintéticas personalizadas** > **Editar página de WAF** en el menú de configuración de Fastly para personalizar el código predeterminado para su proyecto de infraestructura de Adobe Commerce en la nube. Cuando edite el código, conserve la siguiente línea que proporciona el ID de referencia para el evento de bloqueo de WAF:

```html
<p>"} req.http.x-request-id {"</p>
```

>[!NOTE]
>
>La opción Editar WAF solo está disponible si el servicio Cloud WAF administrado está habilitado para su proyecto de infraestructura en la nube de Adobe Commerce.

**Para editar la página de error de WAF**:

1. [Inicie sesión en Admin](../../get-started/onboarding.md#access-your-admin-panel).

1. Seleccione **Tiendas** > **Configuración** > **Configuración** > **Avanzada** > **Sistema**.

1. En el panel derecho, expanda **Caché de página completa** > **Configuración rápida** > **Páginas sintéticas personalizadas**.

   ![Editar opción de página de error de WAF](../../assets/cdn/fastly-custom-synthetic-pages-edit-waf.png)

1. Seleccione **Editar página de WAF**.

1. Rellene los campos para actualizar el HTML.

   ![Actualizar página de error de WAF](../../assets/cdn/fastly-edit-waf-html.png)

   - **Estado**: seleccione el estado `403 Forbidden`.
   - **Tipo MIME** — Tipo `text/html`.
   - **Contenido**: edite la respuesta predeterminada del HTML para agregar CSS personalizado y actualizar el título y los mensajes según sea necesario.

1. Seleccione **Cargar** en la parte superior de la página para cargar el origen del HTML personalizado en el servidor de Fastly.

1. Seleccione **Guardar configuración** en la parte superior de la página para guardar el archivo de configuración actualizado.

1. Actualice la caché.

   - En la notificación que aparece en la parte superior de la página, seleccione el vínculo **Administración de caché**.

   - En la página Administración de caché, seleccione **Vaciar caché del Magento**.

## Mostrar número de informe de error

De manera predeterminada, oculta rápidamente todos los errores de Adobe Commerce detrás del error *503 Servicio no disponible*. Para mostrar el número del informe de registro de errores y poder encontrar y revisar los detalles del error en los registros, abra el sitio web omitiendo Rápidamente siguiendo estos pasos:

1. Recupere la dirección IP de su tienda:

   - Para entornos de ensayo y producción Pro:

     ```bash
     nslookup {your_project_id}.ent.magento.cloud
     ```

   - Para entornos de integración Pro y entornos Starter:

     ```bash
     nslookup gw.{your_region}.magentosite.cloud
     ```

1. Añada el dominio de aplicación y la dirección IP al archivo de hosts de la estación de trabajo local:

   ```text
   {server_IP} {store_domain}
   ```

1. Borre la caché y las cookies del explorador (o cambie al modo incógnito).

1. Vuelva a abrir el sitio web de la tienda para ver el código de error.

1. Utilice el código de error para encontrar los detalles en el archivo de informe de errores:

   - [Conexión al entorno afectado mediante SSH](../development/secure-connections.md#connect-to-a-remote-environment)

   - Busque el archivo `./var/report/{error_number}`.
