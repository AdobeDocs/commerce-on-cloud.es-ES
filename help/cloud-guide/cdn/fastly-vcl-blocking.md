---
title: VCL personalizado para bloquear solicitudes
description: Bloquear solicitudes entrantes por dirección IP mediante una lista de control de acceso (ACL) de Edge con un fragmento de VCL personalizado.
feature: Cloud, Configuration, Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '996'
ht-degree: 0%

---

# VCL personalizado para bloquear solicitudes

Puede utilizar el módulo Fastly CDN para el Magento 2 para crear una ACL de Edge con una lista de direcciones IP que desee bloquear. A continuación, puede utilizar esa lista con un fragmento de VCL para bloquear solicitudes entrantes. El código comprueba la dirección IP de la solicitud entrante. Si coincide con una dirección IP incluida en la lista ACL, Fastly bloquea la solicitud para que no acceda al sitio y devuelve un `403 Forbidden error`. Todas las demás direcciones IP de cliente tienen acceso permitido.

**Requisitos previos:**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

- Lista de direcciones IP de cliente para bloquear

## Cree una ACL de Edge para bloquear las direcciones IP del cliente

Puede crear una ACL de Edge para definir la lista de direcciones IP que desea bloquear. Después de crear la ACL, puede utilizarla en un fragmento de VCL personalizado para administrar el acceso al sitio de ensayo o producción.

Administre el acceso para los sitios de ensayo y producción creando la ACL de Edge con el mismo nombre en ambos entornos. El código del fragmento de VCL se aplica a ambos entornos.

1. Inicie sesión en Admin.
1. Vaya a **Tiendas** > Configuración > **Configuración** > **Avanzado** > **Sistema** > **Caché de página completa** > **Configuración rápida**.
1. Expanda la sección **Edge ACL**.
1. Haga clic en **Agregar ACL** para crear una lista. Para este ejemplo, asigne a la lista el nombre &quot;lista de bloqueados&quot;.
1. Introduzca valores de direcciones IP en la lista. Las direcciones IP de cliente agregadas a esta lista están bloqueadas y no pueden obtener acceso al sitio.
1. De manera opcional, seleccione la casilla de verificación **Anulado** si es necesario.

Se hace referencia a la ACL de Edge por su nombre en el código de fragmento de VCL.

## Cree la VCL personalizada para la lista de bloqueados

>[!NOTE]
>
>Este ejemplo muestra a usuarios avanzados cómo crear un fragmento de código VCL para configurar reglas de bloqueo personalizadas y cargarlo en el servicio Fastly. Puede configurar una lista de bloqueados o lista de permitidos según el país desde el administrador de Adobe Commerce mediante la función [Bloqueo](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/BLOCKING.md) disponible en el módulo Fastly CDN para el Magento 2.

Después de definir la ACL de Edge, puede utilizarla para crear el fragmento de VCL y bloquear el acceso a las direcciones IP especificadas en la ACL. Puede utilizar el mismo fragmento de VCL en los entornos de Ensayo y Producción, pero debe cargar el fragmento en cada entorno por separado.

El siguiente código de fragmento de VCL personalizado (formato JSON) muestra la lógica para bloquear solicitudes entrantes con una dirección IP de cliente que coincida con una dirección de la ACL de lista de bloqueados.

```json
{
  "name": "blocklist",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ( client.ip ~ blocklist) { error 403 \"Forbidden\"; }"
}
```

Antes de crear un fragmento basado en este ejemplo, revise los valores para determinar si necesita realizar algún cambio:

- `name`: nombre del fragmento de VCL. Para este ejemplo, se usó el nombre `blocklist`.

- `priority`: Determina cuándo se ejecuta el fragmento de VCL. La prioridad es `5` para que se ejecute inmediatamente y compruebe si una solicitud de administrador proviene de una dirección IP permitida. El fragmento se ejecuta antes de que cualquiera de los fragmentos de VCL de Magento predeterminados (`magentomodule_*`) tenga asignada una prioridad de 50. Establezca la prioridad de cada fragmento personalizado por encima o por debajo de 50, según el momento en el que desee que se ejecute el fragmento. Los fragmentos con números de prioridad más bajos se ejecutan primero.

- `type`: especifica el tipo de fragmento de VCL que determina la ubicación del fragmento en el código de VCL generado. En este ejemplo, utilizamos `recv`, que inserta el código VCL en la subrutina `vcl_recv`, debajo de la VCL de plantillas y encima de cualquier objeto. Consulte [Fastly VCL snippet reference](https://docs.fastly.com/api/config#api-section-snippet) para obtener la lista de tipos de fragmentos.

- `content`: fragmento de código VCL que se va a ejecutar, que comprueba la dirección IP del cliente. Si la IP está en la ACL de Edge, se bloquea el acceso con un error `403 Forbidden` para todo el sitio web. Todas las demás direcciones IP de cliente tienen acceso permitido.

Después de revisar y actualizar el código para su entorno, utilice cualquiera de los siguientes métodos para agregar el fragmento de VCL personalizado a la configuración del servicio de Fastly:

- [Agregar el fragmento de VCL personalizado del administrador](#add-the-custom-vcl-snippet). Se recomienda utilizar este método si puede acceder al administrador. (Requiere [Fastly versión 1.2.58](fastly-configuration.md#upgrade-fastly-module) o posterior).

- Guarde el ejemplo de código JSON en un archivo (por ejemplo, `blocklist.json`) y [cárguelo mediante la API de Fastly](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api). Utilice este método si no puede acceder al administrador.

## Añadir el fragmento de VCL personalizado

{{admin-login-step}}

1. Haga clic en **Tiendas** > Configuración > **Configuración** > **Avanzado** > **Sistema**.

1. Expandir **Caché De Página Completa** > **Configuración Rápida** > **Fragmentos De VCL Personalizados**.

1. Haga clic en **Crear fragmento personalizado**.

1. Añada los valores de fragmento de VCL:

   - **Nombre** — `blocklist`

   - **Tipo** — `recv`

   - **Prioridad** — `5`

   - Agregar el contenido del fragmento **VCL**:

     ```conf
     if ( client.ip ~ blocklist) { error 403 "Forbidden"; }
     ```

1. Haga clic en **Crear** para generar el archivo de fragmento de VCL con el patrón de nombre `type_priority_name.vcl`, por ejemplo `recv_5_blocklist.vcl`

1. Después de que la página se vuelva a cargar, haga clic en **Cargar VCL a Fastly** en la sección *Configuración de Fastly* para agregar el archivo a la configuración del servicio de Fastly.

1. Después de las cargas, actualice la caché según la notificación que aparece en la parte superior de la página.

Valida rápidamente la versión actualizada del código VCL durante el proceso de carga. Si la validación falla, edite el fragmento de VCL personalizado para solucionar el problema. A continuación, vuelva a cargar la VCL.

## Ejemplos adicionales de VCL para bloquear solicitudes

Los siguientes ejemplos muestran cómo bloquear solicitudes utilizando instrucciones de condición dentro de la línea en lugar de una lista ACL.

>[!WARNING]
>
>En estos ejemplos, el código VCL tiene el formato de carga útil JSON que se puede guardar en un archivo y enviar en una solicitud de API de Fastly. Puede enviar el fragmento [VCL del administrador](#add-the-custom-vcl-snippet), o como una cadena JSON usando la API de Fastly. Para evitar errores de validación al utilizar la API de Fastly con una cadena JSON, debe utilizar una barra invertida para escapar caracteres especiales.

>[!NOTE]
>Si va a enviar el fragmento de VCL desde el administrador, extraiga los valores individuales del código de VCL de muestra e introdúzcalos en los campos correspondientes. Por ejemplo:
>- Nombre: `<name of the VCL>`
>- Dinámico: `<0/1>`
>- Tipo: `<type>`
>- Prioridad: `<priority>`
>- Contenido: `<content>`

Consulte [Uso de fragmentos de VCL dinámicos](https://docs.fastly.com/vcl/vcl-snippets/) en la documentación de VCL de Fastly.

### Ejemplo de código VCL: Block by country code

En este ejemplo se utiliza el código de país ISO 3166-1 de dos caracteres del país asociado a la dirección IP.

```json
{
  "name": "blockbycountrycode",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ( client.geo.country_code == \"HK\" ) { error 405 \"Not allowed\";}"
}
```

>[!NOTE]
>
>En lugar de usar un fragmento de VCL personalizado, puede usar la función [Bloqueo](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/BLOCKING.md) de Fastly en el Administrador de infraestructura en la nube de Adobe Commerce para configurar el bloqueo por código de país o una lista de códigos de país.

### Ejemplo de código VCL: Block by HTTP User-Agent request header

```json
{
  "name": "blockbyuseragent",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ( req.http.User-Agent ~ \"(UCBrowser|MQQBrowser|LieBaoFast|Mb2345Browser)\" ) {error 405 \"Not allowed\";}"
}
```

{{automate-vcl-snippet-deployment}}

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}
