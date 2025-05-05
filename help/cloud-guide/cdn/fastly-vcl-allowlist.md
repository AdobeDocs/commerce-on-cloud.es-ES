---
title: VCL personalizado para permitir solicitudes
description: Filtre las solicitudes entrantes y permita el acceso por dirección IP a los sitios de Adobe Commerce mediante una lista ACL de Fastly Edge y un fragmento de VCL personalizado.
feature: Cloud, Configuration, Security
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '848'
ht-degree: 0%

---

# VCL personalizado para permitir solicitudes

Puede utilizar una lista ACL de Fastly Edge con un fragmento de código VCL personalizado para filtrar solicitudes entrantes y permitir el acceso por dirección IP. La lista ACL especifica las direcciones IP que se permiten.

Cree una lista de permitidos para limitar el acceso al entorno de ensayo, de modo que solo se permitan las solicitudes de direcciones IP especificadas para desarrolladores internos y servicios externos aprobados. También puede crear una lista de permitidos para proteger el acceso al administrador en los entornos de ensayo y producción.

El siguiente ejemplo muestra cómo utilizar un fragmento de VCL personalizado con una [Lista de control de acceso rápido (ACL)](https://docs.fastly.com/guides/access-control-lists/about-acls) para asegurar el acceso al administrador para un entorno de proyecto de Adobe Commerce en la nube. Al agregar el fragmento de VCL personalizado al entorno de Cloud, Fastly solo permite solicitudes de direcciones IP incluidas en la ACL.

>[!TIP]
>
>Para entornos de ensayo e integración que no deben ser de acceso público, utilice la opción de control de acceso HTTP disponible en [[!DNL Cloud Console]](../project/overview.md#access-the-project-web-interface) para administrar el acceso a todo el sitio por dirección IP.

**Requisitos previos:**


{{$include /help/_includes/vcl-snippet-prerequisites.md}}

- Lista de direcciones IP de cliente para incluir en la lista de permitidos

## Crear una ACL de Edge para permitir direcciones IP de cliente

Las ACL de Edge crean listas de direcciones IP para administrar el acceso al sitio. En este ejemplo, se crea una ACL de Edge y se añade la lista de direcciones IP de cliente permitidas para acceder al administrador del entorno del proyecto.

{{admin-login-step}}

1. Haga clic en **Tiendas** > Configuración > **Configuración** > **Avanzado** > **Sistema**.

1. Expandir **Caché de página completa** > **Configuración rápida** > **ACL de Edge**.

1. Cree el contenedor ACL:

   - Haga clic en **Agregar ACL**.

   - En la página *Contenedor ACL*, escriba un **nombre ACL**—`allowlist`.

   - Seleccione **Activar después del cambio** para implementar los cambios en la versión de la configuración del servicio de Fastly que está editando.

   - Haga clic en **Cargar** para adjuntar la ACL a la configuración del servicio de Fastly.

1. Añada la lista de direcciones IP permitidas para acceder al administrador:

   - Haga clic en el icono Configuración para la ACL `allowlist`.

   - Agregue y guarde el *valor de IP* para cada dirección IP de cliente.

   - Haga clic en **Cancelar** para volver a la página de configuración del sistema.

1. Haga clic en **Guardar configuración**.

1. Actualice la caché según la notificación que aparece en la parte superior de la página.

## Cree el fragmento de VCL personalizado para proteger el acceso de administrador

El siguiente código de fragmento de VCL personalizado (formato JSON) muestra la lógica para filtrar solicitudes al administrador y permitir el acceso si la dirección IP del cliente coincide con una dirección de la ACL `allowlist`.

```json
{
  "name": "allowlist",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ((req.url ~ \"^/admin\") && !(client.ip ~ allowlist) && !req.http.Fastly-FF) { error 403 \"Forbidden\"; }"
}
```

Antes de [crear un fragmento personalizado](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-allowlist.html?lang=es#add-the-custom-vcl-snippet) a partir de este ejemplo, revise los valores para determinar si necesita realizar algún cambio. A continuación, introduzca cada valor en los campos respectivos, como `type` en el campo Tipo, `content` en el campo Contenido.

- `name`: nombre del fragmento de VCL. Para este ejemplo, `allowlist`.

- `priority` — Determina cuándo se ejecuta el fragmento de VCL. La prioridad es `5` para ejecutar inmediatamente y comprobar si las solicitudes de un administrador provienen de una dirección IP permitida. El fragmento se ejecuta antes de que cualquiera de los fragmentos de VCL de Magento predeterminados (`magentomodule_*`) tenga asignada una prioridad de 50. Establezca la prioridad de cada fragmento personalizado por encima o por debajo de 50, según el momento en el que desee que se ejecute el fragmento. Los fragmentos con números de prioridad más bajos se ejecutan primero.

- `type` — especifica una ubicación para insertar el fragmento en el código de VCL con versiones. Este VCL es un tipo de fragmento `recv` que agrega el código de fragmento a la subrutina `vcl_recv` debajo del código VCL predeterminado de Fastly y encima de cualquier objeto.

- `content`: fragmento de código VCL que se va a ejecutar. En este ejemplo, el código filtra las solicitudes al administrador y permite el acceso si la dirección IP del cliente coincide con una dirección de la ACL `allowlist`. Si la dirección no coincide, la solicitud se bloquea con un error `403 Forbidden`.

  Si se cambió la dirección URL del administrador, reemplace el valor de ejemplo `/admin` por la dirección URL de su entorno. Por ejemplo, `/company-admin`.

En el ejemplo de código, la condición `!req.http.Fastly-FF` es importante cuando se usa [Blindaje de origen](fastly-custom-cache-configuration.md#configure-back-ends-and-origin-shielding). No elimine ni edite este código.

Después de revisar y actualizar el código para su entorno, utilice cualquiera de los siguientes métodos para agregar el fragmento de VCL personalizado a la configuración del servicio de Fastly:

- [Agregar el fragmento de VCL personalizado del administrador](#add-the-custom-vcl-snippet). Se recomienda utilizar este método si puede acceder al administrador. (Requiere el módulo CDN de [Fastly para la versión 1.2.58](fastly-configuration.md#upgrade) de Magento 2 o posterior).

- Guarde el ejemplo de código JSON en un archivo (por ejemplo, `allowlist.json`) y [cárguelo mediante la API de Fastly](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api). Utilice este método si no puede acceder al administrador.

## Añadir el fragmento de VCL personalizado

{{admin-login-step}}

1. Haga clic en **Tiendas** > Configuración > **Configuración** > **Avanzado** > **Sistema**.

1. Expandir **Caché De Página Completa** > **Configuración Rápida** > **Fragmentos De VCL Personalizados**.

1. Haga clic en **Crear fragmento personalizado**.

1. Añada los valores de fragmento de VCL:

   - **Nombre** — `allowlist`

   - **Tipo** — `recv`

   - **Prioridad** — `5`

   - Agregar el contenido del fragmento **VCL**:

     ```conf
     if ((req.url ~ "^/admin") && !(client.ip ~ allowlist) && !req.http.Fastly-FF) { error 403 "Forbidden";}
     ```

1. Haga clic en **Crear** para generar el archivo de fragmento de VCL con el patrón de nombre `type_priority_name.vcl`, por ejemplo `recv_5_allowlist.vcl`

1. Después de que la página se vuelva a cargar, haga clic en **Cargar VCL a Fastly** en la sección *Configuración de Fastly* para agregar el archivo a la configuración del servicio de Fastly.

1. Una vez finalizada la carga, actualice la caché según la notificación que aparece en la parte superior de la página.

Valida rápidamente la versión actualizada del código VCL durante el proceso de carga. Si la validación falla, edite el fragmento de VCL personalizado para solucionar el problema. A continuación, vuelva a cargar la VCL.

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}
