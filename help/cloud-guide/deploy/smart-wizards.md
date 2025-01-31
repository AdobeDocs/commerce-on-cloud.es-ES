---
title: Asistentes inteligentes
description: Aprenda a utilizar asistentes inteligentes para evaluar si su proyecto de Adobe Commerce en la nube sigue las prácticas recomendadas de implementación.
feature: Cloud, Build, Deploy, SCD
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '322'
ht-degree: 0%

---

# Asistentes inteligentes

Los asistentes inteligentes pueden ayudarle a determinar si la configuración de Cloud cumple las prácticas recomendadas. Los asistentes disponibles le ayudarán con las siguientes configuraciones:

- Estado ideal para un tiempo de inactividad mínimo de la implementación
- Configuración de equilibrio de carga para la base de datos y Redis
- Implementación de contenido estático (SCD) para bajo demanda, la fase de compilación o la fase de implementación

Cada uno de los comandos del asistente inteligente proporciona una respuesta de verificación y, si procede, una recomendación para la configuración adecuada.

| Comando | Descripción |
| ------- | ------------|
| `wizard:ideal-state` | Compruebe que SCD se encuentra en el escenario _build_, que la variable `SKIP_HTML_MINIFICATION` es `true` y que el vínculo post_deploy está configurado en el entorno de la nube. No se debe utilizar en el entorno de desarrollo local. |
| `wizard:master-slave` | Compruebe que la variable `REDIS_USE_SLAVE_CONNECTION` y la variable `MYSQL_USE_SLAVE_CONNECTION` sean `true`. |
| `wizard:scd-on-demand` | Compruebe que la variable de entorno global `SCD_ON_DEMAND` sea `true`. |
| `wizard:scd-on-build` | Compruebe que la variable de entorno global `SCD_ON_DEMAND` sea `false` y la variable de entorno `SKIP_SCD` sea `false` para la fase _build_. Comprueba que el archivo `config.php` contiene información para tiendas, grupos de tiendas y sitios web. |
| `wizard:scd-on-deploy` | Compruebe que la variable de entorno global `SCD_ON_DEMAND` sea `false` y la variable de entorno `SKIP_SCD` sea `false` para la fase _deploy_. Comprueba que el archivo `config.php` no contiene _NOT_ la lista de tiendas, grupos de tiendas y sitios web con información relacionada. |

Por ejemplo, puede comprobar que la configuración habilita correctamente la función SCD bajo demanda:

```bash
./vendor/bin/ece-tools wizard:scd-on-demand
```

Una configuración correcta devuelve:

```
SCD on-demand is enabled
```

Una configuración fallida devuelve lo siguiente:

```
SCD on-demand is disabled
```

## Verificación de una configuración ideal

La configuración _ideal_ para su proyecto en la nube ayuda a minimizar el tiempo de inactividad de la implementación al calentar la caché y generar contenido estático cuando el usuario lo solicita. Este asistente se ejecuta automáticamente durante el proceso de implementación. Si su nube no está configurada para este _estado ideal_, recibirá un mensaje similar al siguiente:

```
- SCD on build is not configured
- Post-deploy hook is not configured
- Skip HTML minification is disabled

Ideal state is not configured
```

En función del resultado, debe realizar las siguientes correcciones en la configuración:

1. Habilite la variable de minificación Omitir HTML.

   > .magento.env.yaml

   ```yaml
   stage:
     global:
       SKIP_HTML_MINIFICATION: true
   ```

1. Configure el vínculo posterior a la implementación.

   > .magento.app.yaml

   ```yaml
       post_deploy: |
           php ./vendor/bin/ece-tools post-deploy
   ```

1. Inserte los cambios del código y ejecute la prueba de nuevo. Cuando la configuración sea _ideal_, recibirá el siguiente mensaje.

   ```
   Ideal state is configured
   ```
