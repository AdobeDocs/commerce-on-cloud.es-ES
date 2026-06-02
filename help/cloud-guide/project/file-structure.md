---
title: Estructura del proyecto
description: Obtenga información acerca de la estructura de archivos y las plantillas de proyecto para Adobe Commerce en la infraestructura en la nube.
exl-id: 364e40e4-a5b3-4d23-b86d-74fc0696ac19
TQID: https://experienceleague.adobe.com/B6fTvmHLFa5THSgLKsjl1smPC8ekPdXB9A-vyqFVwG8
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 473
ht-degree: 0%

---

# Estructura del proyecto

Un proyecto de Adobe Commerce en la nube incluye archivos esenciales para las credenciales y la configuración de la aplicación. Estos archivos están disponibles en como plantilla según la versión de Adobe Commerce. Consulte las plantillas de nube basadas en la versión de Adobe Commerce en el [`magento/magento-cloud` repositorio de GitHub](https://github.com/magento/magento-cloud).

En la tabla siguiente se describen los archivos incluidos en un proyecto de la nube:

| Archivo | Descripción |
| ------------------------- | ------------ |
| `/.magento/routes.yaml` | Archivo de configuración que redirige a `www` al dominio Apex y a la aplicación `php` para servir HTTP. Consulte [Configurar rutas](../routes/routes-yaml.md). |
| `/.magento/services.yaml` | Archivo de configuración que define una instancia de MySQL (MariaDB), Redis y OpenSearch o Elasticsearch. Consulte [Configurar servicios](../services/services-yaml.md). |
| `/app` | La carpeta `code` se usa para módulos personalizados. La carpeta `design` se usa para [temáticas personalizadas](../store/custom-theme.md). La carpeta `etc` contiene archivos de configuración para la aplicación. |
| `/m2-hotfixes` | Se utiliza para parches personalizados. |
| `/update` | Carpeta de servicio utilizada por el módulo de soporte técnico. |
| `.gitignore` | Especifique los archivos y directorios que desea omitir. Ver [`.gitignore` referencia](#ignoring-files). |
| `.magento.app.yaml` | Archivo de configuración que define las propiedades para generar la aplicación. Consulte [Configurar aplicación](../application/configure-app-yaml.md). |
| `.magento.env.yaml` | Archivo de configuración para las fases de compilación, implementación y posterior implementación. El paquete `ece-tools` incluye una muestra de este archivo. Consulte [Configurar entornos](../environment/configure-env-yaml.md). |
| `composer.json` | Obtiene Adobe Commerce y los scripts de configuración para preparar la aplicación. Consulte [Paquetes requeridos](../development/overview.md#required-packages). |
| `composer.lock` | Almacena las dependencias de versión de cada paquete. Consulte [Paquetes requeridos](../development/overview.md#required-packages). |
| `magento-vars.php` | Se usa para definir [varias tiendas](../store/multiple-sites.md) y sitios que usan variables. |

{style="table-layout:auto"}

>[!NOTE]
>
>Cuando inserta los cambios locales en el servidor remoto, el script de implementación utiliza los valores definidos por los archivos de configuración en el directorio `.magento` y, a continuación, el script elimina el directorio y su contenido. Su entorno de desarrollo local no se ve afectado.

## Directorio raíz de la aplicación

La ubicación del directorio raíz de la aplicación depende del entorno.

- **Integración de Starter y Pro**: `/app`
- **Producción inicial**: `/<project-ID>`
- **Ensayo profesional**: `/<project-ID>_stg`
- **Producción profesional**: `/<project-ID>`

### Directorios grabables

Los entornos remotos Integration, Staging y Production son de solo lectura. Los siguientes directorios son los directorios grabables *only* por motivos de seguridad:

- `var`
- `pub/static`
- `pub/media`
- `app/etc`
- `/tmp`

>[!NOTE]
>
>En los entornos Producción y Ensayo, cada nodo del clúster de tres nodos tiene un directorio `/tmp` que no se comparte con los demás nodos.

## Omitir archivos

Hay un archivo base `.gitignore` con el repositorio de proyectos de Adobe Commerce en la nube. Consulte el archivo [.gitignore más reciente en el repositorio de la nube de Magento](https://github.com/magento/magento-cloud/blob/master/.gitignore). Para agregar un archivo que se encuentra en la lista `.gitignore`, puede usar la opción `-f` (forzar) al almacenar en zona intermedia una confirmación:

```bash
git add <path/filename> -f
```

## Cambiar plantilla base

Puede seguir los siguientes pasos para cambiar la estructura de un proyecto existente y reflejar la plantilla base más reciente para Adobe Commerce en la infraestructura en la nube.

1. Clone el proyecto en una estación de trabajo local.

1. Actualice el archivo `composer.json` con los siguientes valores para la sección `extra`.

   ```json
   "extra": {
       "magento-force": true
       "magento-deploystrategy": "copy"
   }
   ```

1. Agregue el archivo `.gitignore` diseñado para la plantilla base. Por ejemplo, si necesita el archivo `.gitignore` para la plantilla de la versión 2.2.6, use el archivo [.gitignore para 2.2.6](https://github.com/magento/magento-cloud/blob/2.2.6/.gitignore) como referencia.

1. Borre la caché de Git.

   ```bash
   git rm -r --cached .
   ```

1. Agregar y confirmar cambios.

   ```bash
   git add -A && git commit -m "Update base template"
   ```

{{redeploy-warning}}
