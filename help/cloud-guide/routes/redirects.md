---
title: Redirecciones
description: Obtenga información sobre cómo administrar las reglas de redirección para su proyecto de Adobe Commerce en la nube.
feature: Cloud, Routes
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '646'
ht-degree: 0%

---

# Redirecciones

La administración de reglas de redirección es un requisito común de las aplicaciones web, especialmente en los casos en que no desea perder los vínculos entrantes que han cambiado o se han eliminado con el tiempo.

A continuación se muestra cómo administrar las reglas de redirección en Adobe Commerce en proyectos de infraestructura en la nube mediante el archivo de configuración `routes.yaml`. Si los métodos de redirección mencionados en este tema no funcionan, puede utilizar encabezados de almacenamiento en caché para hacer lo mismo.

{{route-placeholder}}

## Actualizaciones en entornos Pro

{{pro-self-service-warning}}

>[!WARNING]
>
>Para Adobe Commerce en proyectos de infraestructura en la nube, configurar numerosas redirecciones y reescrituras que no son de regex en el archivo `routes.yaml` puede causar problemas de rendimiento. Si el archivo de `routes.yaml` tiene 32 KB o más, descargue las redirecciones que no sean de regex y vuelva a escribir en Fastly. Ver [Descarga de redirecciones no regex a Fastly en lugar de a Nginx (rutas)](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/offload-non-regex-redirects-to-fastly-instead-of-nginx-routes.html) en el _Centro de ayuda de Adobe Commerce_.

## Redirecciones de ruta completa

Con las redirecciones de ruta completa, puede definir rutas simples mediante el archivo `routes.yaml`. Por ejemplo, puede redirigir de un dominio Apex a un subdominio `www` de la siguiente manera:

```yaml
http://{default}/:
    type: redirect
    to: http://www.{default}/
```

## Redirecciones de ruta parcial

En el archivo `.magento/routes.yaml`, puede agregar reglas de redireccionamiento parciales a rutas existentes basándose en la coincidencia de patrones:

```yaml
http://{default}/:
    redirects:
        expires: 1d
        paths:
          "/from": { to: "http://example.com/" }
          "/regexp/(.*)/matching": { to: "http://example.com/$1", regexp: true }
```

Las redirecciones parciales funcionan con cualquier tipo de ruta, incluidas las rutas servidas directamente por la aplicación.

Hay dos claves disponibles en `redirects`:

- **expires**: opcional; especifica la cantidad de tiempo para almacenar en caché la redirección en el explorador. Algunos ejemplos de valores válidos son `3600s`, `1d`, `2w`, `3m`.

- **rutas**: uno o más pares de clave-valor que especifican la configuración de las reglas de redireccionamiento de ruta parcial.

  Para cada regla de redirección, la clave es una expresión para filtrar las rutas de solicitud para la redirección. El valor es un objeto que especifica el destino de destino de la redirección y las opciones para procesar la redirección.

  El objeto value tiene las propiedades siguientes:

  | Propiedad | Descripción |
  | ---------- | ----------- |
  | `to` | Obligatorio, una ruta absoluta parcial, una dirección URL con protocolo y host o un patrón que especifique el destino de destino de la regla de redirección. |
  | `regexp` | Opcional, el valor predeterminado es `false`. Especifica si la clave de ruta de acceso debe interpretarse como una expresión regular PCRE. |
  | `prefix` | Especifica si la redirección se aplica tanto a la ruta de acceso como a todas sus rutas secundarias, o solo a la propia ruta de acceso. El valor predeterminado es `true`. Este valor no se admite si `regexp` es `true`. |
  | `append_suffix` | Determina si el sufijo se transfiere con el redireccionamiento. El valor predeterminado es `true`. Este valor no se admite si la clave `regexp` es `true` o* si la clave `prefix` es `false`. |
  | `code` | Especifica el código de estado HTTP. Los códigos de estado válidos son [`301` (movido permanentemente)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.3.2), [`302`](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.3.3), [`307`](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.3.8) y [`308`](https://www.rfc-editor.org/rfc/rfc7238). El valor predeterminado es `302`. |
  | `expires` | Opcional, especifica la cantidad de tiempo para almacenar en caché el redireccionamiento en el explorador. El valor predeterminado es `expires`, definido directamente bajo la clave `redirects`, pero en este nivel puede ajustar la caducidad de la caché para las redirecciones parciales individuales. |

## Ejemplos de redirecciones de ruta parcial

Los siguientes ejemplos muestran cómo especificar redirecciones de ruta parcial en el archivo `routes.yaml` mediante varias opciones de configuración de `paths`.

### Coincidencia de patrones de expresión regular

Utilice el siguiente formato para configurar solicitudes de redirección basadas en una expresión regular.

```yaml
http://{default}/:
    type: upstream
    redirects:
      paths:
        "/regexp/(.*)/match": { to: "http://example.com/$1", regexp: true }
```

Esta configuración filtra las rutas de solicitud en una expresión regular y redirige las solicitudes coincidentes a `https://example.com`. Por ejemplo, una solicitud a `https://example.com/regexp/a/b/c/match` redirige a `https://example.com/a/b/c`.

### Coincidencia de patrones de prefijo

Utilice el siguiente formato para configurar solicitudes de redirección para rutas que comienzan con un patrón de prefijo especificado.

```yaml
http://{default}/:
    type: upstream
    redirects:
      paths:
        "/from": { to: "https://{default}/to", prefix: true }
```

Esta configuración funciona de la siguiente manera:

- Redirige las solicitudes que coinciden con el patrón `/from` a la ruta de acceso `http://{default}/to`.

- Redirige las solicitudes que coinciden con el patrón `/from/another/path` a `https://{default}/to/another/path`.

- Si cambia la propiedad `prefix` a `false`, las solicitudes que coinciden con el patrón `/from` se redirigirán, pero las solicitudes que coinciden con el déclencheur `/from/another/path` no.

### Coincidencia de patrones de sufijo

Utilice el siguiente formato para configurar solicitudes de redireccionamiento que adjuntan el sufijo de ruta de la solicitud al destino de destino:

```yaml
http://{default}/:
    type: upstream
    redirects:
      paths: "/from": { to: "https://{default}/to", append_suffix: false }
```

Esta configuración funciona de la siguiente manera:

- Redirige las solicitudes que coinciden con el patrón `/from/path/suffix` a la ruta de acceso `https://{default}/to`.

- Si cambia la propiedad `append_suffix` a `true`, las solicitudes que coincidan con `/from/path/suffix` se redirigirán a la ruta de acceso `https://{default}/to/path/suffix`.

### Configuración de caché específica de la ruta

Utilice el siguiente formato para personalizar el tiempo para almacenar en caché una redirección desde una ruta específica:

```yaml
http://{default}/:
    type: upstream
    redirects:
    expires: 1d
      paths:
        "/from": { to: "https://example.com/" }
        "/here": { to: "https://example.com/there", expires: "2w" }
```

Esta configuración funciona de la siguiente manera:

- Las redirecciones de la primera ruta (`/from`) se almacenan en caché durante un día.

- Las redirecciones de la segunda ruta (`/here`) se almacenan en caché durante dos semanas.
