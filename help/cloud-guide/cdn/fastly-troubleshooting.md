---
title: Solución de problemas rápida
description: Obtenga información sobre cómo solucionar problemas y administrar el módulo y los servicios de Fastly CDN para Adobe Commerce.
feature: Cloud, Configuration, Cache, Services
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1834'
ht-degree: 0%

---

# Solución de problemas rápida

Utilice la siguiente información para solucionar los problemas y administrar el módulo CDN de Fastly para Magento 2 en los entornos de proyecto de Adobe Commerce en la nube. Por ejemplo, puede investigar los valores de encabezado de respuesta y el comportamiento de almacenamiento en caché para resolver los problemas de rendimiento y servicio de Fastly.

En entornos de ensayo y producción profesional, puede usar [registros de New Relic](../monitor/log-management.md) para ver y analizar los datos de registro de Fastly CDN y WAF para solucionar errores y problemas de rendimiento.

>[!NOTE]
>
>Para obtener información sobre cómo configurar Fastly, consulte [Configurar Fastly](fastly.md).

## Localizar ID de servicio de Fastly

Necesita el ID del servicio de Fastly para configurar Fastly desde el administrador o para enviar solicitudes de API de Fastly para una configuración avanzada de Fastly y la resolución de problemas.

Si Fastly está habilitado en el entorno del proyecto, puede obtener el ID de servicio del administrador. Ver [Obtener credenciales de Fastly](fastly-configuration.md#get-fastly-credentials).

Los desarrolladores y los usuarios avanzados de VCL pueden utilizar VCL personalizado para recuperar el ID del servicio mediante la variable de Fastly `req.service_id`. Por ejemplo, puede agregar `req.service_id` a la directiva de registro personalizada en su VCL para capturar el valor del ID de servicio:

```json
log {"syslog"} req.service_id {" my_logging_endpoint_name :: "}
```

Puede utilizar el mismo VCL para los entornos Producción y Ensayo. Ver [`vcl_log`](https://www.fastly.com/documentation/reference/vcl/subroutines/log/) en la _documentación de Fastly_.

## Problemas de rendimiento del sitio, depuración y caché

Utilice la siguiente lista para identificar y solucionar los problemas relacionados con la configuración del servicio Fastly para su entorno de infraestructura en la nube de Adobe Commerce.

- El menú **Almacenar no se muestra ni funciona**: es posible que esté usando un vínculo o un vínculo temporal directamente al servidor de origen en lugar de usar la dirección URL del sitio activo, o que haya usado `-H "host:URL"` en un [comando cURL](#check-live-site-through-fastly). Si omite Fastly en el servidor de origen, el menú principal no funciona y se muestran encabezados incorrectos que permiten el almacenamiento en caché en el explorador.

- **La navegación superior no funciona**: La navegación superior depende del procesamiento de Edge Side Includes (ESI), que está habilitado al cargar los fragmentos de VCL predeterminados de Magento de Fastly. Si la navegación no funciona, [cargue Fastly VCL](fastly-configuration.md#upload-vcl-to-fastly) y vuelva a comprobar el sitio.

- **La ubicación geográfica/GeoIP no funciona**— Los fragmentos de VCL predeterminados de Magento de Fastly anexan el código de país a la dirección URL. Si el código de país no funciona, [cargue Fastly VCL](fastly-configuration.md#upload-vcl-to-fastly) y vuelva a comprobar el sitio.

- **Las páginas no se almacenan en caché**: de forma predeterminada, Fastly no almacena en caché las páginas con el encabezado `Set-Cookies`. Adobe Commerce establece cookies incluso en páginas almacenables en caché (TTL > 0). El Magento predeterminado Fastly VCL elimina esas cookies en páginas almacenables en caché. Si las páginas no se almacenan en caché, [cargue Fastly VCL](fastly-configuration.md#upload-vcl-to-fastly) y vuelva a comprobar el sitio.

  Este problema también se puede producir si un bloque de página de una plantilla está marcado como no almacenable en caché. En ese caso, el problema se debe probablemente a que un módulo o extensión de terceros bloquee o elimine los encabezados de Adobe Commerce. Para resolver el problema, vea [X-Cache contiene solamente MISS, no HIT](#x-cache-contains-only-miss-no-hit).

- **Las solicitudes de purga están fallando**—Al enviar una solicitud de purga, devuelve rápidamente el siguiente error:

  ```text
  The purge request was not processed successfully.
  ```

  Este problema puede deberse a cualquiera de los siguientes problemas:

   - Credenciales de Fastly no válidas en la configuración del servicio Fastly para el entorno del proyecto de infraestructura de Adobe Commerce en la nube
   - Código no válido en un fragmento de VCL personalizado

  Para resolver el problema, consulte [Error al purgar la caché de Fastly en la nube](https://support.magento.com/hc/en-us/articles/115001853194-Error-purging-Fastly-cache-on-Cloud-The-purge-request-was-not-processed-successfully-) en el Centro de ayuda de Adobe Commerce.

## 503 errores de Fastly

Si Fastly devuelve errores de tiempo de espera 503, compruebe los registros de errores y la página de error 503 para identificar la causa raíz.

>[!NOTE]
>
>Si el tiempo de espera se agota al ejecutar operaciones masivas, puede [ampliar el tiempo de espera de Fastly para el administrador](fastly-custom-cache-configuration.md#extend-fastly-timeout).

Si recibe un error 503, compruebe el registro de errores del entorno de producción o ensayo y el registro de acceso a php para solucionar el problema.

**Para comprobar los registros de errores**:

- [Registro de errores](../test/log-locations.md#application-logs)

  ```text
  /var/log/platform/<project-ID>/error.log
  ```

  Este registro incluye cualquier error de la aplicación o del motor PHP, por ejemplo `memory_limit` o `max_execution_time exceeded` errores. Si no encuentra ningún error relacionado con Fastly, consulte el registro de acceso de PHP.

- Registro de acceso de PHP

  ```text
  /var/log/platform/<project-ID>/php.access.log
  ```

  Busque en el registro respuestas HTTP 200 para la dirección URL que devolvió el error 503. Si encuentra la respuesta 200, significa que Adobe Commerce devolvió la página sin errores. Esto indica que el problema podría haberse producido después del intervalo que supera el valor `first_byte_timeout` establecido en la configuración del servicio de Fastly.

Cuando se produce un error 503, Fastly devuelve el motivo en la página de error y mantenimiento. Es posible que no pueda ver el motivo si agregó código para una [página de respuesta personalizada](fastly-custom-response.md). Para ver el código de motivo en la página de error predeterminada, puede quitar el código de HTML de la página de error personalizada.

**Para comprobar la página de error de Fastly 503**:

{{admin-login-step}}

1. Haga clic en **Tiendas** > **Configuración** > **Configuración** > **Avanzado** > **Sistema**.

1. En el panel derecho, expanda **Caché de página completa**.

1. En la sección **Configuración rápida**, expanda **Páginas sintéticas personalizadas** como se muestra en la siguiente ilustración.

   ![Página de error personalizada 503](../../assets/cdn/fastly-custom-synthetic-pages-edit-html.png)

1. Haga clic en **Establecer HTML**.

1. Elimine el código personalizado. Puede guardarlo en un programa de texto para agregarlo de nuevo más tarde.

1. Haga clic en **Cargar** para enviar las actualizaciones a Fastly.

1. Haga clic en **Guardar configuración** en la parte superior de la página.

1. Vuelva a abrir la dirección URL que provocó el error 503. Devuelve rápidamente una página de error con el motivo, como se muestra en el siguiente ejemplo.

   ![Error rápido](../../assets/cdn/fastly-503-example.png)

## Apex y subdominios ya asociados a una cuenta de Fastly

Si el dominio Apex y los subdominios del proyecto de infraestructura de Adobe Commerce en la nube ya están asociados con una cuenta existente de Fastly con un ID de servicio asignado, no puede iniciar hasta que actualice la configuración de Fastly:

- Actualice la configuración de Apex y subdominio en la cuenta existente de Fastly. Ver [Varias cuentas de Facebook y dominios asignados](fastly.md#multiple-fastly-accounts-and-assigned-domains).

- [Habilitar y configurar Fastly](fastly-configuration.md#enable-fastly-caching) y completar la [configuración de DNS](../launch/checklist.md#update-dns-configuration-with-production-settings)

## Verificar o depurar los servicios de Fastly

Puede solucionar problemas de rendimiento o almacenamiento en caché para un Adobe Commerce en un sitio de infraestructura en la nube probando las direcciones URL del sitio y examinando los valores de encabezado devueltos en la respuesta.

### Compruebe el sitio en directo mediante Fastly

Utilice la API de Fastly para comprobar los encabezados de respuesta `Fastly-Magento-VCL-Uploaded` y `X-Cache` devueltos desde el sitio activo.

Las solicitudes de API de Fastly se pasan a través de la extensión de Fastly para obtener una respuesta de los servidores de origen. Si la respuesta devuelve encabezados incorrectos, pruebe los [servidores de origen directamente](#bypass-fastly-cache-to-check-adobe-commerce-sites).

**Para comprobar los encabezados de respuesta**:

1. En un terminal, use el siguiente comando `curl` para probar la dirección URL activa del sitio:

   ```bash
   curl https://<live URL> -vo /dev/null -H Fastly-Debug:1
   ```

   Si no ha establecido una ruta estática o completado la configuración DNS para los dominios del sitio activo, utilice el indicador `--resolve`, que omite la resolución de nombres DNS.

   ```bash
   curl -svo /dev/null --resolve '<your_hostname>:443:<IP-address-of-cache-node>' <https-URL>
   ```

   >[!NOTE]
   >
   >Para utilizar este comando con la opción `--resolve`, debe tener TLS habilitado con Fastly a través de un certificado SSL/TLS y encontrar la dirección IP del nodo de caché.

1. En la respuesta, compruebe los [encabezados](#check-cache-hit-and-miss-response-headers) para asegurarse de que Fastly funciona. Debería ver los siguientes encabezados únicos en la respuesta:

   ```http
   < Fastly-Magento-VCL-Uploaded: yes
   < X-Cache: HIT, MISS
   ```

Si los encabezados no tienen los valores correctos, consulte la siguiente información:

- [Comprobar carga de VCL](#fastly-vcl-has-not-been-uploaded)

- [X-Cache contiene solamente MISS, no HIT](#x-cache-contains-only-miss-no-hit)

### Omitir la caché de Fastly para comprobar los sitios de Adobe Commerce

Si el servicio Fastly devuelve encabezados incorrectos, puede crear un fragmento de VCL que le permita enviar solicitudes que omitan la caché de Fastly. Ver [Omitir caché de Fastly](fastly-vcl-bypass-to-origin.md).

Después de agregar el fragmento de VCL, utilice los comandos cURL para enviar solicitudes al servidor de origen desde la dirección IP especificada. A continuación, compruebe si hay errores en las respuestas.

### Comprobar encabezados de respuesta HIT y MISS de caché

Compruebe que la respuesta devuelta contiene la siguiente información:

- Incluye el encabezado `X-Magento-Tags`

- El valor del encabezado `Fastly-Module-Enabled` es `Yes` o el número de versión del módulo Fastly para el Magento de CDN 2 instalado en el entorno del proyecto

- [Cache-Control: max-age](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) es mayor que 0

- La configuración de [Pragma](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.32) es `cache`

El siguiente extracto del resultado del comando cURL muestra los valores correctos para los encabezados `Pragma`, `X-Magento-Tags` y `Fastly-Module-Enabled`:

```
* STATE: INIT => CONNECT handle 0x600057800; line 1402 (connection #-5000)
* Rebuilt URL to: https://www.mymagento.biz.c.sv7gVom4qrpek.ent.magento.cloud/
* Added connection 0. The cache now contains 1 members
* Trying 192.0.2.31...
* STATE: CONNECT => WAITCONNECT handle 0x600057800; line 1455 (connection #0)

% Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0* Connected to www.mymagento.biz.c.sv7gVom4qrpek.ent.magento.cloud (54.229.163.31) port 443 (#0)

* STATE: WAITCONNECT => SENDPROTOCONNECT handle 0x600057800; line 1562 (connection #0)
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0* ALPN, offering h2

... portion omitted for brevity ...

< Set-Cookie: mage-messages=%5B%5D; expires=Wed, 22-Nov-2017 17:39:58 GMT; Max-Age=31536000; path=/
< Pragma: cache
< Expires: Wed, 23 Nov 2016 17:39:56 GMT
< Cache-Control: max-age=86400, public, s-maxage=86400, stale-if-error=5, stale-while-revalidate=5
< X-Magento-Tags: cb_welcome_popup store cb cb_store_info_mobile cb_header_promotional_bar cb_store_info cb_discount-promo-bar cpg_2 cb_83 cb_81 cb_84 cb_85 cb_86 cb_87 cb_88 cb_89 p5646 catalog_product p5915 p6040 p6197 p6227 p7095 p6109 p6122 p6331 p7592 p7651 p7690
< Fastly-Module-Enabled: yes
< Strict-Transport-Security: max-age=31536000
    < Content-Security-Policy: upgrade-insecure-requests
    < X-Content-Type-Options: nosniff
    < X-XSS-Protection: 1; mode=block
    < X-Frame-Options: SAMEORIGIN
    < X-Platform-Server: i-dff64b52
    <
    * STATE: PERFORM => DONE handle 0x600057800; line 1955 (connection #0)
    * multi_done
      0     0    0     0    0     0      0      0 --:--:--  0:00:02 --:--:--     0
    * Connection #0 to host www.mymagento.biz.c.sv7gVom4qrpek.ent.magento.cloud left intact
```

>[!NOTE]
>
>Para obtener información detallada sobre visitas y errores, consulte [Explicación de los encabezados HIT y MISS de la caché con servicios blindados](https://docs.fastly.com/guides/performance-tuning/understanding-cache-hit-and-miss-headers-with-shielded-services) en la documentación de Fastly.

### Resolver errores encontrados en los encabezados de respuesta

Esta sección proporciona sugerencias para resolver los errores devueltos al comprobar los encabezados de respuesta mediante la API de Fastly.

#### El módulo de Fastly no está habilitado

Si el módulo de Fastly no está habilitado (`Fastly-Module-Enabled: no`) o si falta el encabezado, [use SSH para iniciar sesión](../development/secure-connections.md#connect-to-a-remote-environment) en el proyecto. A continuación, ejecute el siguiente comando para comprobar el estado del módulo.

```bash
php bin/magento module:status Fastly_Cdn
```

En función del estado devuelto, utilice las siguientes instrucciones para actualizar la configuración de Fastly.

- `Module does not exist`: si el módulo no existe [instale y configure](https://github.com/fastly/fastly-magento2/blob/master/Documentation/INSTALLATION.md) el módulo Fastly de CDN para el Magento 2 en una rama de integración. Una vez finalizada la instalación, habilite y configure el módulo. Ver [Configuración rápida](fastly-configuration.md).

- `Module is disabled`: si el módulo de Fastly está deshabilitado, actualice la configuración de entorno en una rama `integration` del entorno local para habilitarlo. A continuación, inserte los cambios en Ensayo y producción. Ver [Administrar extensiones](../store/extensions.md#install-an-extension).

  Si usa [Administración de configuración](../store/store-settings.md#configure-store), compruebe el estado del módulo CDN de Fastly en el archivo de configuración `app/etc/config.php` antes de insertar cambios en el entorno de producción o ensayo.

  Si el módulo no está habilitado (`Fastly_CDN => 0`) en el archivo `config.php`, elimine el archivo y ejecute el siguiente comando para actualizar `config.php` con la configuración más reciente.

  ```bash
  bin/magento magento-cloud:scd-dump
  ```

#### Fastly VCL no se ha cargado

Si no se ha cargado el VCL de Fastly (`Fastly-Magento-VCL-Uploaded`: `false`), use la opción *Cargar VCL* en el administrador para cargarlo. Ver [Cargar fragmentos de VCL rápidamente](fastly-configuration.md#upload-vcl-to-fastly).

#### X-Cache contiene solamente MISS, no HIT

Si el encabezado `X-Cache` contiene `HIT` (`HIT, HIT` o `HIT, MISS`), indica que Fastly devuelve el contenido almacenado en caché correctamente.

Si el encabezado `X-Cache` es `MISS, MISS` y no contiene `HIT`, ejecute de nuevo el comando `curl` para asegurarse de que la página no se haya purgado recientemente de la caché.

Si obtiene el mismo resultado, use los [`curl` comandos](#check-live-site-through-fastly) y compruebe los [encabezados de respuesta](#check-cache-hit-and-miss-response-headers):

- `Pragma` es `cache`
- `X-Magento-Tags` existe
- `Cache-Control: max-age` es mayor que 0

Si el problema persiste, es probable que otra extensión restablezca estos encabezados. Repita el siguiente procedimiento en el entorno de ensayo desactivando todas las extensiones y volviendo a activar cada una para determinar qué extensión está restableciendo los encabezados. Después de identificar la extensión que causa el problema, debe deshabilitarla en el entorno de producción.

**Para identificar una extensión que restablece los encabezados de respuesta:**

{{admin-login-step}}

1. Vaya a **Tiendas** > **Configuración** > **Configuración** > **Avanzada** > **Avanzada**.

1. En la sección *Deshabilitar salida de módulos* del panel derecho, busque todas las extensiones y desactívelas.

1. Haga clic en **Guardar configuración**.

1. Haga clic en **Sistema** > **Herramientas** > **Administración de caché**.

1. Haga clic en **Vaciar caché del Magento**.

1. Complete los siguientes pasos para cada extensión que pueda causar problemas con los encabezados de Fastly:

   - Habilite una extensión a la vez, guarde la configuración y vacíe la caché de Adobe Commerce.

   - Ejecute los [`curl` comandos](#check-live-site-through-fastly) para comprobar los [encabezados de respuesta](#check-cache-hit-and-miss-response-headers).

   Repita este proceso para cada extensión. Si los encabezados de respuesta rápida ya no se muestran, ha identificado la extensión que está causando problemas con Fastly.

Después de identificar la extensión que está restableciendo los encabezados de Fastly, póngase en contacto con el desarrollador de la extensión para obtener ayuda adicional. No podemos proporcionar correcciones o actualizaciones para que las extensiones de terceros funcionen con el almacenamiento en caché de Fastly.

## Deshacer configuración de Fastly

Si las actualizaciones de fragmentos de VCL personalizadas u otros cambios de configuración de Fastly provocan que un Adobe Commerce en el sitio de infraestructura de la nube rompa o devuelva errores, use el comando [activate](https://docs.fastly.com/api/config#version_0b79ae1ba6aee61d64cc4d43fed1e0d5) de la API de Fastly para revertir a una versión de VCL anterior. No se puede revertir la versión de VCL desde el administrador.

**Para revertir la versión de VCL**:

1. Para obtener una lista de las versiones de VCL disponibles para un servicio, ejecute el siguiente comando

   ```bash
   curl -H "Fastly-Key: <FASTLY_API_TOKEN>" -H "Accept: application/json" https://api.fastly.com/service/<FASTLY_SERVICE_ID>/version
   ```

1. Ejecute el siguiente comando para cambiar la versión activa de VCL a una especificada.

   ```bash
   curl -H "Fastly-Key: <FASTLY_API_TOKEN>" -H "Content-Type: application/x-www-form-urlencoded" -H "Accept: application/json" -X PUT https://api.fastly.com/service/<FASTLY_SERVICE_ID>/version/<VERSION_ID>/activate
   ```

Para obtener más información acerca del uso de la API de Fastly para revisar y administrar VCL, consulte [Administrar VCL mediante la API](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api).
