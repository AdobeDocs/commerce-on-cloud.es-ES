---
title: Variables posteriores a la implementación
description: Consulte la lista de variables de entorno que controlan las acciones en la fase posterior a la implementación de Adobe Commerce en la infraestructura en la nube.
feature: Cloud, Configuration, Cache
recommendations: noDisplay, catalog
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '511'
ht-degree: 0%

---

# Variables posteriores a la implementación

Las siguientes variables _posteriores a la implementación_ controlan las acciones en la fase posterior a la implementación y pueden heredar y anular los valores de las [variables globales](variables-global.md). Inserte estas variables en la fase `post-deploy` del archivo `.magento.env.yaml`:

```yaml
stage:
  post-deploy:
    POST-DEPLOY_VARIABLE_NAME: value
```

Para obtener más información sobre cómo personalizar el proceso de generación e implementación:

- [Configuración de implementación](configure-env-yaml.md)
- [Proceso de implementación](../deploy/process.md)

## `TTFB_TESTED_PAGES`

- **Predeterminado**— `[]` (una matriz vacía)
- **Versión**: Adobe Commerce 2.1.4 y posterior

Configure las pruebas de _Tiempo hasta el primer byte_ (TTFB) para las páginas especificadas a fin de probar el rendimiento del sitio. Especifique una referencia de ruta absoluta o una dirección URL con protocolo y host para cada página que requiera la prueba.

```yaml
stage:
  post-deploy:
    TTFB_TESTED_PAGES:
       - "index.php"
       - "index.php/customer/account/create"
       - "https://example.com/catalog/some-category"
```

Después de especificar las páginas para probar y confirmar los cambios, la prueba de _Tiempo hasta el primer byte_ se ejecuta durante la fase posterior a la implementación y publica los resultados de cada ruta en el registro de nube:

```
[2019-06-20 20:42:22] INFO: TTFB test result: 0.313s {"url":"https://staging-tkyicst-xkmwgjkwmwfuk.us-4.magentosite.cloud/customer/account/create","status":200}
[2019-06-20 20:42:22] INFO: TTFB test result: 0.408s {"url":"https://staging-tkyicst-xkmwgjkwmwfuk.us-4.magentosite.cloud/checkout/cart","status":200}
```

Para las rutas redirigidas, el registro indica la ruta del destino de redirección en lugar de la configurada en la variable de entorno. Si especifica una ruta no válida, el registro muestra un mensaje de advertencia.

## `WARM_UP_CONCURRENCY`

- **Predeterminado**—_No establecido_
- **Versión**: Adobe Commerce 2.1.4 y posterior

Especifique el límite de solicitudes simultáneas que se enviarán durante las operaciones de calentamiento de caché para reducir la carga del servidor. Este valor limita el número de conexiones paralelas y resulta útil para las configuraciones de entorno en las que la variable posterior a la implementación de `WARM_UP_PAGES` especifica varias páginas para la precarga de la caché.

```yaml
stage:
  post-deploy:
    WARM_UP_CONCURRENCY: 4
```

## `WARM_UP_PAGES`

- **Predeterminado**— `index.php`
- **Versión**: Adobe Commerce 2.1.4 y posterior

Personalice la lista de páginas utilizadas para precargar la caché en la fase `post_deploy`. Debe configurar el vínculo posterior a la implementación. Consulte la [sección de vínculos](../application/hooks-property.md) del archivo `.magento.app.yaml`.

- **páginas únicas**: especifique una sola página para agregarla a la caché. No es necesario indicar la dirección URL base predeterminada. El siguiente ejemplo almacena en caché la página `BASE_URL/index.php`:

  ```yaml
  stage:
    post-deploy:
      WARM_UP_PAGES:
        - "index.php"
  ```

- **varios dominios**: enumera varias direcciones URL. El siguiente ejemplo almacena en caché páginas de dos dominios:

  ```yaml
  stage:
    post-deploy:
      WARM_UP_PAGES:
        - 'http://example1.com/test'
        - 'http://example2.com/test'
  ```

- **varias páginas**: utilice el siguiente formato para almacenar en caché varias páginas según un patrón de expresión regular específico:

  ```
  <entity_type>:<pattern|url|product_sku>:<store_id|store_code>
  ```

   - `entity_type`: posibles variantes `category`, `cms-page`, `product`, `store-page`
   - `pattern|url|product_sku`: use un patrón `regexp` o una coincidencia exacta `url` para filtrar las direcciones URL, o use un asterisco (\*) para todas las páginas. Usar SKU de producto para el tipo de entidad `product`
   - `store_id|store_code`: use el identificador o el código de la tienda o un asterisco (\*) para todas las tiendas, puede pasar varios identificadores o códigos de tienda separados por `|`

  El siguiente ejemplo almacena en caché los tipos de entidad `category` y `cms-page` según estos criterios:
   - todas las páginas de categoría de la tienda con id. `1`
   - todas las páginas de categoría para tiendas con código `store1` y `store2`
   - página de categoría `cars` para el almacén con código `store_en`
   - página de cms `contact` para todas las tiendas
   - página cms `contact` para tiendas con ID `1` y `2`
   - cualquier página de categoría que contenga `car_` y termine con `html` para la tienda con ID 2
   - cualquier página de categoría que contenga `tires_` para la tienda con código `store_gb`

     ```yaml
     stage:
       post-deploy:
         WARM_UP_PAGES:
           - "category:*:1"
           - "category:*:store1|store2"
           - "category:cars:store_en"
           - "cms-page:contact:*"
           - "cms-page:contact:1|2"
           - "category:|car_.*?\\.html$|:2"
           - "category:|tires_.*|:store_gb"
     ```

  El siguiente ejemplo almacena en caché el tipo de entidad `product` según estos criterios:
   - todos los productos para todas las tiendas (con una programación limitada a 100 por tienda para evitar problemas de rendimiento)
   - todos los productos de la tienda `store1`
   - productos con `sku1` para todas las tiendas
   - productos con `sku1` para tiendas con código `store1` y `store2`
   - productos con `sku1`, `sku2` y `sku3` para tiendas con código `store1` y `store2`

     ```yaml
     stage:
       post-deploy:
         WARM_UP_PAGES:
           - "product:*:*"
           - "product:*:store1"
           - "product:sku1:*"
           - "product:sku1:store1|store2"
           - "product:sku1|sku2|sku3:store1|store2"
     ```

  El siguiente ejemplo almacena en caché el tipo de entidad `store-page` según estos criterios:
   - página `/contact-us` para todas las tiendas
   - página `/contact-us` para tienda con ID `1`
   - página `/contact-us` para tiendas con código `code1` y `code2`

  ```yaml
        stage:
          post-deploy:
            WARM_UP_PAGES:
              - "store-page:/contact-us:*"
              - "store-page:/contact-us:1"
              - "store-page:/contact-us:code1|code2"
  ```
