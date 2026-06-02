---
title: Almacenamiento en caché
description: Obtenga información sobre cómo habilitar el almacenamiento en caché para su Adobe Commerce en entornos de infraestructura en la nube.
feature: Cloud, Cache, Routes
exl-id: e73c36d6-9a58-45c0-9220-86074c1f46f0
TQID: https://experienceleague.adobe.com/dCr0px-0XWXIznsg1w8tUnBaAeXvanY1h-mwiu6GfzU
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 430
ht-degree: 0%

---

# Almacenamiento en caché

Puede habilitar el almacenamiento en caché en el entorno de su proyecto de infraestructura en la nube. Si deshabilita el almacenamiento en caché, Adobe Commerce proporciona directamente los archivos.

{{route-placeholder}}

## Configuración del almacenamiento en caché

Habilite el almacenamiento en caché para su aplicación configurando las reglas de caché en el archivo `.magento/routes.yaml` de la siguiente manera:

```yaml
http://{default}/:
    type: upstream
    upstream: php:php
    cache:
        enabled: true
        headers: [ "Accept", "Accept-Language", "X-Language-Locale" ]
        cookies: ["*"]
        default_ttl: 60
```

## Almacenamiento en caché basado en rutas

Habilite el almacenamiento en caché detallado configurando reglas de almacenamiento en caché para varias rutas por separado, como se muestra en el siguiente ejemplo:

```yaml
http://{default}/:
    type: upstream
    upstream: php:php
    cache:
        enabled: true

http://{default}/path/:
    type: upstream
    upstream: php:php
    cache:
        enabled: false

http://{default}/path/more/:
    type: upstream
    upstream: php:php
    cache:
        enabled: true
```

El ejemplo anterior almacena en caché las rutas siguientes:

- `http://{default}/`
- `http://{default}/path/more/`
- `http://{default}/path/more/etc/`

Y las siguientes rutas están **no** en la caché:

- `http://{default}/path/`
- `http://{default}/path/etc/`

>[!NOTE]
>
>Se admiten expresiones regulares en las rutas **no**.

## Duración de caché

La duración de la caché está determinada por el valor del encabezado de respuesta `Cache-Control`. Si no hay ningún encabezado `Cache-Control` en la respuesta, se utiliza la clave `default_ttl`.

## Clave de caché

Para decidir cómo almacenar en caché una respuesta, Adobe Commerce crea una clave de caché que depende de varios factores y almacena la respuesta asociada a esta clave. Cuando una solicitud viene con la misma clave de caché, la respuesta se reutiliza. Su propósito es similar al del encabezado [&#128279;](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.44) HTTP `Vary`.

Los parámetros `headers` y `cookies` claves permiten cambiar esta clave de caché.

El valor predeterminado para estas claves es el siguiente:

```yaml
cache:
    enabled: true
    headers: ["Accept-Language", "Accept"]
    cookies: ["*"]
```

## Atributos de caché

### `enabled`

Cuando se establece en `true`, habilite la caché para esta ruta. Cuando se establece en `false`, deshabilitar la caché para esta ruta.

### `headers`

Define los valores de los que debe depender la clave de caché.

Por ejemplo, si la clave `headers` es la siguiente:

```yaml
cache:
    enabled: true
    headers: ["Accept"]
```

A continuación, Adobe Commerce almacena en caché una respuesta diferente para cada valor del encabezado HTTP `Accept`.

### `cookies`

La clave `cookies` define de qué valores debe depender la clave de caché.

Por ejemplo:

```yaml
cache:
    enabled: true
    cookies: ["value"]
```

La clave de caché depende del valor de la cookie `value` en la solicitud.

Existe un caso especial si la clave `cookies` tiene el valor `["*"]`. Este valor significa que cualquier solicitud con una cookie evita la caché. Este es el valor predeterminado.

>[!NOTE]
>
>No se pueden utilizar caracteres comodín en el nombre de la cookie. Use un nombre de cookie preciso o combine todas las cookies con un asterisco (`*`). Por ejemplo, `SESS*` o `~SESS` son actualmente **valores no** válidos.

Las cookies tienen las siguientes restricciones:

- Hay un máximo establecido de **50 cookies** en el sistema. De lo contrario, la aplicación genera una excepción `Unable to send the cookie. Maximum number of cookies would be exceeded`. Para aumentar el número de cookies a 200, aplique el [parche MDVA-12304](https://experienceleague.adobe.com/docs/commerce-operations/tools/quality-patches-tool/release-notes.html) con la [herramienta Parches de calidad](https://experienceleague.adobe.com/en/docs/commerce-learn/tutorials/tools/quality-patch-tool).
- Un tamaño máximo de cookie es de **4096 bytes**. De lo contrario, la aplicación genera una excepción `Unable to send the cookie. Size of '%name' is %size bytes`.

### `default_ttl`

Si la respuesta no tiene un encabezado `Cache-Control`, se usa la clave `default_ttl` para definir la duración de la caché, en segundos. El valor predeterminado es `0`, lo que significa que no se almacena nada en caché.
