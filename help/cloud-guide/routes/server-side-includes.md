---
title: Inclusiones del lado del servidor
description: Aprenda a utilizar las inclusiones del lado del servidor con Adobe Commerce en la infraestructura en la nube.
feature: Cloud, Routes
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '172'
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

SSI le permite incluir en las directivas de respuesta del HTML que hacen que el servidor rellene partes del HTML, respetando cualquier [configuración de almacenamiento en caché](caching.md) existente.

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
