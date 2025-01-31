---
title: Bloquear spam de referencia
description: Bloquee el correo no deseado de referencia de su sitio mediante el diccionario Fastly de Edge y un fragmento de VCL personalizado.
feature: Cloud, Configuration, Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '684'
ht-degree: 0%

---

# Bloquear spam de referencia

El siguiente ejemplo muestra cómo configurar [Fastly Edge Dictionary](https://docs.fastly.com/guides/edge-dictionaries/working-with-dictionaries-using-the-api) con un fragmento de VCL personalizado para bloquear el correo no deseado de referencia de su Adobe Commerce en el sitio de infraestructura de la nube.

>[!NOTE]
>
>Se recomienda añadir configuraciones de VCL personalizadas a un entorno de ensayo en el que pueda probarlas antes de ejecutarlas en el entorno de producción.

**Requisitos previos:**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

- Revise los registros del sitio para ver si hay direcciones URL de referencia falsas y haga una lista de dominios para bloquear.

## Creación de una lista de bloqueados de referente

Los diccionarios de Edge crean pares de clave-valor accesibles para las funciones VCL durante el procesamiento de fragmentos VCL. En este ejemplo, se crea un diccionario de Edge que proporciona la lista de sitios web de referencia que se van a bloquear.

{{admin-login-step}}

1. Haga clic en **Tiendas** > **Configuración** > **Configuración** > **Avanzado** > **Sistema**.

1. Expandir **Caché de página completa** > **Configuración rápida** > **diccionarios de Edge**.

1. Cree el contenedor Diccionario:

   - Haga clic en **Agregar contenedor**.

   - En la página *Contenedor*, escriba un **nombre de diccionario**—`referrer_blocklist`.

   - Seleccione **Activar después del cambio** para implementar los cambios en la versión de la configuración del servicio de Fastly que está editando.

   - Haga clic en **Cargar** para adjuntar el diccionario a la configuración del servicio de Fastly.

1. Agregue la lista de nombres de dominio para bloquear al diccionario `referrer_blocklist`:

   - Haga clic en el icono Configuración para el diccionario `referrer_blocklist`.

   - Agregue y guarde pares de clave-valor en el nuevo diccionario. Para este ejemplo, cada **clave** es el nombre de dominio de una URL de referente que se va a bloquear y **valor** es `true`.

     ![Agregar elementos incorrectos del diccionario de referente](../../assets/cdn/fastly-referrer-blocklist-dictionary.png)

   - Haga clic en **Cancelar** para volver a la página de configuración del sistema.

1. Haga clic en **Guardar configuración**.

1. Actualice la caché según la notificación que aparece en la parte superior de la página.

Para obtener más información acerca de los diccionarios de Edge, consulte [Creación y uso de diccionarios de Edge](https://docs.fastly.com/guides/edge-dictionaries/working-with-dictionaries-using-the-api) y [fragmentos personalizados de VCL](https://docs.fastly.com/guides/edge-dictionaries/working-with-dictionaries-using-the-api#custom-vcl-examples) en la documentación de Fastly.

## Cree un fragmento de VCL personalizado para bloquear el correo no deseado del referente

El siguiente código de fragmento de VCL personalizado (formato JSON) muestra la lógica para comprobar y bloquear solicitudes. El fragmento de VCL captura el host de un sitio web de referente en un encabezado y, a continuación, compara el nombre de host con la lista de direcciones URL del diccionario `referrer_blocklist`. Si el nombre de host coincide, la solicitud se bloquea con un error `403 Forbidden`.

```json
{
  "name": "block_bad_referrer",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if (req.http.Referer ~ \"^(.*:)//([A-Za-z0-9\-\.]+)(:[0-9]+)?(.*)$\") {set req.http.Referer-Host = re.group.2;}if (table.lookup(referrer_blocklist, req.http.Referer-Host)) {error 403 \"Forbidden\";}"
}
```

Antes de crear un fragmento basado en este ejemplo, revise los valores para determinar si necesita realizar algún cambio:

- `name`: nombre del fragmento de VCL. Para este ejemplo, usamos `block_bad_referrer`.

- `dynamic` — El valor 0 indica que hay [un fragmento normal](https://docs.fastly.com/en/guides/using-regular-vcl-snippets) que cargar en el VCL con versiones para la configuración de Fastly.

- `priority` — Determina cuándo se ejecuta el fragmento de VCL. La prioridad es `5` para ejecutar este código de fragmento antes de que cualquiera de los fragmentos de VCL de Magento predeterminados (`magentomodule_*`) tenga asignada una prioridad de 50. Establezca la prioridad de cada fragmento personalizado por encima o por debajo de 50, según el momento en el que desee que se ejecute el fragmento. Los fragmentos con números de prioridad más bajos se ejecutan primero.

- `type` — especifica una ubicación para insertar el fragmento de código en la versión de VCL. En este ejemplo, el fragmento VCL es un fragmento `recv`. Cuando el fragmento se inserta en la versión de VCL, se agrega a la subrutina `vcl_recv`, debajo del código VCL predeterminado de Fastly y encima de cualquier objeto.

- `content`: fragmento de código VCL que se ejecutará en una línea, sin saltos de línea.

Después de revisar y actualizar el código para su entorno, utilice cualquiera de los siguientes métodos para agregar el fragmento de VCL personalizado a la configuración del servicio de Fastly:

- [Agregar el fragmento de VCL personalizado del administrador](#add-the-custom-vcl-snippet). Se recomienda utilizar este método si puede acceder al administrador. (Requiere [Fastly versión 1.2.58](fastly-configuration.md#upgrade) o posterior).

- Guarde el ejemplo de código JSON en un archivo (por ejemplo, `allowlist.json`) y [cárguelo mediante la API de Fastly](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api). Utilice este método si no puede acceder al administrador.

## Añadir el fragmento de VCL personalizado

{{admin-login-step}}

1. Haga clic en **Tiendas** > Configuración > **Configuración** > **Avanzado** > **Sistema**.

1. Expandir **Caché De Página Completa** > **Configuración Rápida** > **Fragmentos De VCL Personalizados**.

1. Haga clic en **Crear fragmento personalizado**.

1. Añada los valores de fragmento de VCL:

   - **Nombre** — `block_bad_referrer`

   - **Tipo** — `recv`

   - **Prioridad** — `5`

   - **VCL** contenido de fragmento —

     ```conf
     if (req.http.Referer ~ "^(.*:)//([A-Za-z0-9\-\.]+)(:[0-9]+)?(.*)$") {
       set req.http.Referer-Host = re.group.2;  
     }
     if (table.lookup(referrer_blocklist, req.http.Referer-Host)) {
       error 403 "Forbidden";
     }
     ```

1. Haga clic en **Crear**.

   ![Crear fragmento VCL de bloque de referente personalizado](/help/assets/cdn/fastly-create-referrer-block-snippet.png)

1. Después de que la página se vuelva a cargar, haz clic en **Cargar VCL a Fastly** en la sección *Configuración de Fastly*.

1. Una vez finalizada la carga, actualice la caché según la notificación que aparece en la parte superior de la página.

Valida rápidamente la versión de VCL actualizada durante el proceso de carga. Si la validación falla, edite el fragmento de VCL personalizado para solucionar cualquier problema. A continuación, vuelva a cargar la VCL.

{{automate-vcl-snippet-deployment}}

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}
