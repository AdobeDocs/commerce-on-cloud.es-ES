---
title: Inclusiones del lado del servidor
description: Aprenda a utilizar las inclusiones del lado del servidor con Adobe Commerce en la infraestructura en la nube.
feature: Cloud, Routes
exl-id: 826a9c9a-d082-4ec4-8fd2-00ca357522ab
TQID: https://experienceleague.adobe.com/iLal9p5QiG4U0sHrskzFV8buCCVWZAa3FLixND0Nw24
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 182
ht-degree: 0%

---

# Inclusiones del lado del servidor

[Las inclusiones del lado del servidor](https://nginx.org/en/docs/http/ngx_http_ssi_module.html) (SSI) son directivas en páginas de HTML que se evalúan en el servidor mientras se procesan las páginas. SSI le permite añadir contenido generado dinámicamente a una página de HTML existente sin necesidad de ofrecer la página completa.

Puede activar o desactivar SSI por ruta en su `.magento/routes.yaml`; por ejemplo:

```yaml
    "http://{default}/":
        type: upstream
        upstream: "myapp:php"
        cache:
            enabled: false
            ssi:
                enabled: true
    "http://{default}/time.php":
        type: upstream
        upstream: "myapp:php"
        cache:
            enabled: true
```

SSI le permite incluir en sus directivas de respuesta de HTML que hacen que el servidor rellene partes de HTML, respetando cualquier [configuración de almacenamiento en caché](caching.md) existente.

En el ejemplo siguiente se muestra cómo insertar un control de fecha dinámico en la parte superior de una página y otro control de fecha en la parte inferior que se actualiza cada 600 segundos:

Agregue lo siguiente a cualquier página, como `/index.php`:

```php?start_inline=1
echo date(DATE_RFC2822);
<!--#include virtual="time.php" -->
```

Agregar lo siguiente a `time.php`:

```php?start_inline=1
header("Cache-Control: max-age=600");
echo date(DATE_RFC2822);
```

Vaya a la página en la que agregó el control. Actualice la página varias veces y observe que la hora en la parte superior de la página cambia, pero la hora en la parte inferior solo cambia cada 600 segundos.
