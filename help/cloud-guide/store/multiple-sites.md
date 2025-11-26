---
title: Configurar varios sitios web o tiendas
description: Obtenga información sobre cómo configurar varios sitios web o tiendas para Adobe Commerce en la infraestructura en la nube.
feature: Cloud, Configuration, Routes, Site Navigation
exl-id: 773d8d64-d235-4c2b-87e9-aadbf8471b2c
source-git-commit: 0d84d29c470a098c7238b6ca7cc9538463dda695
workflow-type: tm+mt
source-wordcount: '1013'
ht-degree: 0%

---

# Configurar varios sitios web o tiendas

Puede configurar Adobe Commerce para que tenga varios sitios web o tiendas, como una tienda en inglés, una tienda en francés y una tienda en alemán. Ver [Explicación de sitios web, tiendas y vistas de tiendas](best-practices.md#store-views).

>[!WARNING]
>
>Los datos de catálogo se amplían a medida que aumenta el número de sitios web y tiendas. Según la arquitectura del proyecto, las tiendas adicionales pueden provocar un proceso de indexación más largo y tiempos de respuesta más lentos para las páginas de catálogo no almacenadas en caché. Adobe recomienda monitorizar atentamente el rendimiento del sitio.

El proceso para configurar varios almacenes depende de si elige utilizar dominios únicos o compartidos.

Varios almacenes con dominios únicos:

```
https://first.store.com/
https://second.store.com/
```

Varios almacenes con el mismo dominio:

```
https://store.com/first/
https://store.com/second/
```

>[!TIP]
>
>Para agregar una vista de tienda a la dirección URL base del sitio, no es necesario crear varios directorios. Consulte [Agregar el código de almacén a la dirección URL base](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-admin.html?lang=es) en la _Guía de configuración_.

## Añadir dominios

Los dominios personalizados se pueden añadir a Pro Staging y a cualquier entorno de producción; no se pueden añadir a entornos de integración.

El proceso para agregar un dominio depende del tipo de cuenta de Cloud:

- Para ensayo y producción profesionales

  Agregue el nuevo dominio a Fastly; consulte [Administrar dominios](../cdn/fastly-custom-cache-configuration.md#manage-domains) o abra un ticket de asistencia para solicitar ayuda. Además, debe [enviar un vale de soporte de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=es#submit-ticket) para solicitar que se agreguen nuevos dominios a un clúster.

- Solo para producción inicial

  Agregue el nuevo dominio a Fastly; consulte [Administrar dominios](../cdn/fastly-custom-cache-configuration.md#manage-domains) o [Enviar un ticket de asistencia de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=es#submit-ticket) para solicitar ayuda. Además, debe agregar el nuevo dominio a la ficha **Dominios** en [!DNL Cloud Console]: `https://<zone>.magento.cloud/projects/<project-ID>/edit`

## Configuración de la instalación local

Para configurar la instalación local de modo que utilice varias tiendas, consulte [Varias tiendas o sitios web][config-multiweb] en la _Guía de configuración_.

Después de crear y probar correctamente la instalación local para utilizar varias tiendas, debe preparar su entorno de integración:

1. **Configurar rutas o ubicaciones**: especifique cómo Adobe Commerce administra las direcciones URL entrantes

   - [Rutas para dominios independientes](#configure-routes-for-separate-domains)
   - [Ubicaciones para dominios compartidos](#configure-locations-for-shared-domains)

1. **Configurar sitios web, tiendas y vistas de tiendas**: configúrelos con la IU de administración de Adobe Commerce
1. **Modificar variables**: especifique los valores de las variables `MAGE_RUN_TYPE` y `MAGE_RUN_CODE` en el archivo `magento-vars.php`
1. **Implementar y probar entornos**—implementar y probar la rama `integration`

>[!TIP]
>
>Puede utilizar un entorno local para configurar varios sitios web o tiendas. Consulte las instrucciones de Cloud Docker para [configurar varios sitios web o tiendas](https://developer.adobe.com/commerce/cloud-tools/docker/configure/multiple-sites).

### Actualizaciones de configuración para entornos Pro

{{pro-self-service-warning}}

### Configuración de rutas para dominios independientes

Las rutas definen cómo procesar las direcciones URL entrantes. Varias tiendas con dominios únicos requieren que defina cada dominio en el archivo `routes.yaml`. La forma de configurar las rutas depende de cómo desee que funcione el sitio.

**Para configurar rutas en un entorno de integración**:

1. En la estación de trabajo local, abra el archivo `.magento/routes.yaml` en un editor de texto.

1. Defina el dominio y los subdominios. El valor ascendente `mymagento` es el mismo valor que la propiedad name del archivo `.magento.app.yaml`.

   ```yaml
   "http://{default}/":
       type: upstream
       upstream: "mymagento:http"
   
   "http://<second-site>.{default}/":
       type: upstream
       upstream: "mymagento:http"
   ```

1. Guarde los cambios en el archivo `routes.yaml`.

1. Continuar a [Configurar sitios web, tiendas y vistas de tiendas](#set-up-websites-stores-and-store-views).

### Configuración de ubicaciones para dominios compartidos

Donde la configuración de rutas define cómo se procesan las direcciones URL, la propiedad `web` del archivo `.magento.app.yaml` define cómo se expone la aplicación a la web. Las _ubicaciones_ web permiten una mayor granularidad para las solicitudes entrantes. Por ejemplo, si su dominio es `store.com`, puede usar `/first` (sitio predeterminado) y `/second` para solicitudes a dos tiendas diferentes que comparten un dominio.

**Para configurar una nueva ubicación web**:

1. Cree un alias para la raíz (`/`). En este ejemplo, el alias es `&app` en la línea 3.

   ```yaml
   web:
       locations:
           "/": &app
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
   ```

1. Cree un paso a través para el sitio web (`/website`) y haga referencia a la raíz con el alias del paso anterior.

   El alias permite que `website` tenga acceso a los valores desde la ubicación raíz. En este ejemplo, el sitio web `passthru` está en la línea 21.

   ```yaml
   web:
       locations:
           "/": &app
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
           "/media":
               root: "pub/media"
               ...
           "/static":
               root: "pub/static"
               allow: true
               scripts: false
               passthru: "/front-static.php"
               rules:
                   ^/static/version\d+/(?<resource>.*)$:
                       passthru: "/static/$resource"
           "/<website>": *app
             ...
   ```

**Para configurar una ubicación con un directorio diferente**:

1. Cree un alias para las ubicaciones raíz (`/`) y estáticas (`/static`).

   ```yaml
   web:
       locations:
           "/": &root
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
           "/static": &static
               root: "pub/static"
   ```

1. Cree un subdirectorio para el sitio web bajo el directorio `pub`: `pub/<website>`

1. Copie el archivo `pub/index.php` en el directorio `pub/<website>` y actualice la ruta de acceso `bootstrap` (`/../../app/bootstrap.php`).

   ```
   try {
       require __DIR__ . '/../../app/bootstrap.php';
   } catch (\Exception $e) { 
   ```

1. Cree un paso a través para el archivo `index.php`.

   ```yaml
   web:
       locations:
           "/": &root
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
           "/media":
               root: "pub/media"
               ...
           "/static": &static
               root: "pub/static"
               allow: true
               scripts: false
               passthru: "/front-static.php"
               rules:
                   ^/static/version\d+/(?<resource>.*)$:
                       passthru: "/static/$resource"
           "/<website>":
               <<: *root
               passthru: "<website>/index.php"
           "/<website>/static": *static
             ...
   ```

1. Confirme y envíe los archivos modificados.

   - `pub/<website>/index.php` (si este archivo está en `.gitignore`, la inserción puede requerir la opción de forzar ).
   - `.magento.app.yaml`

### Configuración de sitios web, tiendas y vistas de tiendas

En la _IU de administración_, configure sus **sitios web**, **tiendas** y **vistas de tiendas** de Adobe Commerce. Consulte [Configurar varios sitios web, tiendas y vistas de tiendas en Admin](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-admin.html?lang=es) en la _Guía de configuración_.

Es importante utilizar el mismo nombre y código de sus sitios web, tiendas y vistas de tiendas del administrador al configurar la instalación local. Necesita estos valores cuando actualice el archivo `magento-vars.php`.

### Modificar variables

En lugar de configurar un host virtual NGINX, pase las variables `MAGE_RUN_CODE` y `MAGE_RUN_TYPE` mediante el archivo `magento-vars.php` en el directorio raíz del proyecto.

**Para pasar variables usando el archivo `magento-vars.php`**:

1. Abra el archivo `magento-vars.php` en un editor de texto.

   El [archivo `magento-vars.php` predeterminado](https://github.com/magento/magento-cloud/blob/master/magento-vars.php) debe tener el siguiente aspecto:

   ```php
   <?php
   // enable, adjust and copy this code for each store you run
   // Store #0, default one
   //if (isHttpHost("example.com")) {
   //    $_SERVER["MAGE_RUN_CODE"] = "default";
   //    $_SERVER["MAGE_RUN_TYPE"] = "store";
   //}
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
           return false;
       }
       return $_SERVER['HTTP_HOST'] === $host;
   }
   ```

1. Mover el bloque `if` comentado para que sea _después de_ el bloque `function` y ya no se comente.

   ```php
   <?php
   // enable, adjust and copy this code for each store you run
   // Store #0, default one
   
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
           return false;
       }
       return $_SERVER['HTTP_HOST'] ===  $host;
   }
   
   if (isHttpHost("example.com")) {
       $_SERVER["MAGE_RUN_CODE"] = "default";
       $_SERVER["MAGE_RUN_TYPE"] = "store";
   }
   ```

1. Reemplace los siguientes valores en el bloque `if (isHttpHost("example.com"))`:
   - `example.com`: con la dirección URL base de su _sitio web_
   - `default`: con el código único para tu _sitio web_ o _vista de tienda_
   - `store`: con uno de los siguientes valores:
      - `website`: carga el _sitio web_ en la tienda
      - `store`: carga una _vista de tienda_ en la tienda

   Para varios sitios con dominios únicos:

   ```php
   <?php
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
       return false;
       }
       return $_SERVER['HTTP_HOST'] ===  $host;
   }
   
   if (isHttpHost("second.store.com")) {
       $_SERVER["MAGE_RUN_CODE"] = "<second-site>";
       $_SERVER["MAGE_RUN_TYPE"] = "website";
   }elseif (isHttpHost("store.com")){
       $_SERVER["MAGE_RUN_CODE"] = "base";
       $_SERVER["MAGE_RUN_TYPE"] = "website";
   }
   ```

   Para varios sitios con el mismo dominio, debe comprobar el _host_ y el _URI_:

   ```php
   <?php
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
       return false;
       }
       return $_SERVER['HTTP_HOST'] ===  $host;
   }
   
   if (isHttpHost("store.com")) {
      $code = "base";
      $type = "website";
   
      $uri = explode('/', $_SERVER['REQUEST_URI']);
      if (isset($uri[1]) && $uri[1] == 'second') {
        $code = '<second-site>';
        $type = 'website';
      }
      $_SERVER["MAGE_RUN_CODE"] = $code;
      $_SERVER["MAGE_RUN_TYPE"] = $type;
   }
   ```

1. Guarde los cambios en el archivo `magento-vars.php`.

### Implementación y pruebas en el servidor de integración

Inserte los cambios en su entorno de integración de Adobe Commerce en la infraestructura de la nube y pruebe su sitio.

1. Agregar, confirmar y enviar cambios de código a la rama remota.

   ```bash
   git add -A && git commit -m "Implement multiple sites" && git push origin <branch-name>
   ```

1. Espere a que se complete la implementación.

1. Después de la implementación, abra la URL de la tienda en un explorador web.

   Con un dominio único, use el formato: `http://<magento-run-code>.<site-URL>`

   Por ejemplo, `http://french.master-name-projectID.us.magentosite.cloud/`

   Con un dominio compartido, use el formato: `http://<site-URL>/<magento-run-code>`

   Por ejemplo, `http://master-name-projectID.us.magentosite.cloud/french/`

1. Pruebe exhaustivamente el sitio y combine el código en la rama `integration` para una implementación posterior.

## Implementar en ensayo y producción

Siga el proceso de implementación de [implementación en Ensayo y producción](../deploy/staging-production.md). Para los entornos Starter y Pro, utiliza [!DNL Cloud Console] para insertar el código en todos los entornos.

Adobe recomienda realizar todas las pruebas en el entorno de ensayo antes de pasar al entorno de producción. Realice cambios en el código en el entorno de integración e inicie el proceso para implementar de nuevo en todos los entornos.

<!-- link definitions -->

[config-multiweb]: https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-overview.html?lang=es
