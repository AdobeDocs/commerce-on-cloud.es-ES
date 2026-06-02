---
title: Variables globales
description: Consulte la lista de variables de entorno que controlan las acciones en el proceso de implementación de Adobe Commerce en la nube.
feature: Cloud, Configuration, Build, Deploy, Eventing, Logs, SCD
recommendations: noDisplay, catalog
role: Developer
exl-id: 1f1ef6db-6836-4f71-b1e4-3629352d7e74
TQID: https://experienceleague.adobe.com/2aBPh7We4-KqoUVDfd4B-ZNWoaUVO-3mWVbqErdgyoQ
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: ba9e5be9-7de1-4f71-a5d2-baead0e425eeid: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: c1579802-ddd4-4214-8a91-97b2066abe11id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 774
ht-degree: 0%

---

# Variables globales

Las variables globales controlan las acciones en cada fase del proceso de implementación de [!DNL Commerce]: generación, implementación y posterior a la implementación. Dado que las variables globales afectan a cada fase, debe configurarlas en la fase `global` del archivo `.magento.env.yaml`:

```yaml
stage:
  global:
    GLOBAL_VARIABLE_NAME: value
```

Para obtener más información sobre cómo personalizar el proceso de generación e implementación:

- [Configuración de implementación](configure-env-yaml.md)
- [Proceso de implementación](../deploy/process.md)

## `ENABLE_EVENTING`

- **Predeterminado**-_No establecido_
- **Versión**: Adobe Commerce 2.4.5 y posterior

Cuando se establece en `true`, permite a cron ejecutar consumidores de cola de mensajes. Adobe I/O Events para Adobe Commerce utiliza colas de mensajes para acelerar la entrega de eventos críticos.

Adobe recomienda que también agregue la variable [`CRON_CONSUMERS_RUNNER`](./variables-deploy.md#cron_consumers_runner) a la fase `deploy` del archivo `.magento.env.yaml` con `cron_run` establecido en `true`.

El ejemplo siguiente muestra una variable `ENABLE_EVENTING` completamente configurada.

```yaml
stage:
  global:
    ENABLE_EVENTING: true
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 0
      consumers: []
```

## ENABLE_WEBHOOKS

- **Predeterminado**-_No establecido_
- **Versión**: Adobe Commerce 2.4.4 y posterior

Cuando se establece en `true`, habilita los webhooks de Commerce. El webhook se ejecuta en un punto final externo, como una acción de tiempo de ejecución de App Builder o un sistema de administración de inventario de terceros. La [_Guía de Webhooks_](https://developer.adobe.com/commerce/extensibility/webhooks) describe esta característica en detalle.

```yaml
stage:
  global:
    ENABLE_WEBHOOKS: true
```

## `MIN_LOGGING_LEVEL`

- **Predeterminado**—_No establecido_
- **Versión**: Adobe Commerce 2.1.4 y posterior

Anula el nivel de registro mínimo de todas las secuencias de salida sin cambiar el código, lo que ayuda a solucionar problemas con la implementación. Por ejemplo, si la implementación falla, puede utilizar esta variable para aumentar la granularidad del registro globalmente. Ver [Niveles de registro](log-handlers.md#log-levels). El valor `min_level` en Controladores de registro sobrescribe esta configuración.

```yaml
stage:
  global:
    MIN_LOGGING_LEVEL: debug
```

>[!WARNING]
>
>La configuración de la variable `MIN_LOGGING_LEVEL` no cambia la configuración del nivel de registro para el controlador de archivos, que está establecido en `debug` de forma predeterminada.

## `SCD_ON_DEMAND`

- **Predeterminado**—_No establecido_
- **Versión**: Adobe Commerce 2.1.4 y posterior

Habilitar la generación de contenido estático cuando lo solicite un usuario (SCD). El contenido estático bajo demanda es ideal para el flujo de trabajo de desarrollo y prueba, ya que reduce el tiempo de implementación.

La precarga de la caché mediante el vínculo [`post_deploy`](../application/hooks-property.md) reduce el tiempo de inactividad del sitio. El calentamiento de caché solo está disponible para proyectos Pro que contienen entornos de ensayo y producción en [!DNL Cloud Console] y para proyectos Starter. Agregar la variable de entorno `SCD_ON_DEMAND` al escenario `global` en el archivo `.magento.env.yaml`:

```yaml
stage:
  global:
    SCD_ON_DEMAND: true
```

La variable `SCD_ON_DEMAND` omite el SCD en ambas fases (generación e implementación), borra las carpetas `pub/static` y `var/view_preprocessed` y escribe lo siguiente en el archivo `app/etc/env.php`:

```php?start_inline=1
return array(
   ...
   'static_content_on_demand_in_production' => 1,
   ...
);
```

## `SCD_MAX_EXECUTION_TIME`

- **Predeterminado**—_No establecido_
- **Versión**—Adobe Commerce 2.2.0 y posterior

Permite aumentar el tiempo de ejecución máximo esperado para la implementación de contenido estático.

De forma predeterminada, Adobe Commerce establece el tiempo de ejecución máximo esperado en 900 segundos, pero en algunos casos puede necesitar más tiempo para completar la implementación de contenido estático para un proyecto de Cloud.

```yaml
stage:
  global:
    SCD_MAX_EXECUTION_TIME: 3600
```

{{scd-timing-warning}}

## `SCD_NO_PARENT`

- **Predeterminado**—_No establecido_
- **Versión**: Adobe Commerce 2.4.2 y posterior

Se establece en `true` para evitar la generación de contenido estático para las temáticas principales durante las fases de compilación e implementación. Cuando esta opción se establece en `true`, se genera menos contenido estático, lo que mejora los tiempos generales de compilación e implementación.

```yaml
stage:
  global:
    SCD_NO_PARENT: true
```

## `SCD_USE_BALER`

- **Predeterminado**—_No establecido_
- **Versión**: Adobe Commerce 2.3.0 y posterior

[Baler](https://github.com/magento/baler) es un módulo que analiza el código JavaScript generado y crea un paquete JavaScript optimizado. La implementación del paquete optimizado en el sitio puede reducir el número de solicitudes de red al cargar el sitio y mejorar los tiempos de carga de las páginas.

Se establece en `true` para ejecutar Baler después de realizar la implementación de contenido estático.

```yaml
stage:
  build:
    SCD_USE_BALER: true
```

>[!NOTE]
>
>Instale y configure el módulo Empacadora antes de utilizar esta función. Como Baler está en la versión alfa, active esta opción solo en entornos de ensayo.

## `SKIP_HTML_MINIFICATION`

- **Predeterminado**:
   - `true`: para `ece-tools` 2002.0.13 y posteriores
   - `false`: para versiones anteriores de `ece-tools`
- **Versión**: Adobe Commerce 2.1.4 y posterior

Habilita o deshabilita la copia de archivos de vista estática en el directorio `<magento_root>/init/` al final de la fase de compilación. Si se establece en `true`, los archivos no se copian y la minificación de HTML está disponible bajo solicitud. Establezca este valor en `true` para reducir el tiempo de inactividad al implementar en los entornos de ensayo y producción.

- **`false`**: copia el directorio `view_preprocessed` en el directorio `<magento_root>/init/` al final de la fase de compilación y restaura el directorio en el directorio `<magento_root>/var` al principio de la fase de implementación.
- **`true`**: habilita la minificación de HTML bajo demanda; _no_ copia `<magento_root>var/view_preprocessed` en el directorio `<magento_root>/init/` al final de la fase de compilación.

Agregar la variable de entorno `SKIP_HTML_MINIFICATION` al escenario `global` en el archivo `.magento.env.yaml`:

```yaml
stage:
  global:
    SKIP_HTML_MINIFICATION: true
```

## `X_FRAME_CONFIGURATION`

- **Predeterminado**—_No establecido_
- **Versión**: Adobe Commerce 2.1.4 y posterior

Utilice la variable `X_FRAME_CONFIGURATION` para cambiar la configuración del encabezado [`X-Frame-Options`](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/security/xframe-options.html) para su sitio de Adobe Commerce. Esta configuración controla cómo el explorador procesa una página en `<frame>`, `<iframe>` o `<object>`. Utilice una de las siguientes opciones:

- `DENY`: la página no se puede mostrar en un marco.
- `SAMEORIGIN`—(La configuración predeterminada de Adobe Commerce). La página solo se puede mostrar en un marco del mismo origen que la propia página.

>[!WARNING]
>
>La opción `ALLOW-FROM <uri>` se ha desaprobado porque los exploradores compatibles con Adobe Commerce ya no la admiten. Consulte [Compatibilidad de exploradores](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options#Browser_compatibility).

Agregar la variable de entorno `X_FRAME_CONFIGURATION` al escenario `global` en el archivo `.magento.env.yaml`:

```yaml
stage:
  global:
    X_FRAME_CONFIGURATION: SAMEORIGIN
```
