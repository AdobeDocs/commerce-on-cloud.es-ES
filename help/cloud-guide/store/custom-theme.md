---
title: Tema personalizado
description: Aprenda a instalar una temática personalizada con Adobe Commerce en la infraestructura en la nube.
feature: Cloud, Themes
exl-id: 3ae4b0d5-9179-42c4-bb07-8ec09bd057d0
TQID: https://experienceleague.adobe.com/rk-VP6z1tQSY-HMU-dD9hv6O9Wpesbp1o5KQMYapCJE
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: d1e21356-0064-4f48-9089-16e3f0dbd2a6
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 296
ht-degree: 0%

---

# Tema personalizado

Puede instalar uno o varios temas para usarlos en una o en todas las tiendas y sitios del proyecto. Los temas incluyen varios archivos estáticos, incluidas imágenes, fuentes, CSS, JavaScript, PHP y más, para diseñar completamente sus tiendas. Puede agregar la temática extrayendo su código en el sistema de archivos o utilizando Composer.

## Instalar una temática manualmente

Para instalar una temática manualmente, debe tener el código de la temática en un archivo comprimido o en una estructura de directorios similar a la siguiente:

```text
<VendorName>
  ├── composer.json
      ├── etc
      │   └── view.xml
      ├── media
      ├── registration.php
      ├── theme.xml
      └── web
          ├── css
          │   └── source
          ├── fonts
          ├── images
          └── js
```

**Para instalar un tema manualmente**:

1. Copie el código de la temática en `<Project root dir>/app/design/frontend` para una temática de tienda o en `<Project root dir>/app/design/adminhtml` para una temática de administrador. Compruebe que el directorio de nivel superior sea `<VendorName>`; de lo contrario, el tema no se instalará correctamente.

   ```bash
   cp -r ExampleTheme <project-root>/app/design/frontend
   ```

1. Confirme que la temática se ha copiado en el lugar correcto.

   * Tema de tienda: `ls <project-root>/app/design/frontend`
   * Tema del administrador: `ls <project-root>/app/design/adminhtml`

   A continuación se muestra un ejemplo:

   Ejemplo de Adobe Commerce del tema

1. Agregue y confirme archivos.

   ```bash
   git add -A && git commit -m "Add theme"
   ```

1. Inserte los archivos en la rama.

   ```bash
   git push origin <branch name>
   ```

1. Espere a que se complete la implementación.
1. Inicie sesión en Admin.
1. Haga clic en **Contenido** > Diseño > **Temas**.

   La temática se muestra en el panel derecho.

## Instalar una temática mediante Composer

La instalación de una temática mediante Composer es igual que instalar cualquier otra extensión mediante Composer. Consulte [Instalar, administrar y actualizar módulos](extensions.md) para obtener más información.

Para instalar una temática con Composer:

1. Adquiera la temática en Commerce Marketplace.
1. Obtenga el nombre del compositor de la temática.
1. Cambie al directorio raíz de Adobe Commerce e introduzca el comando:

   ```bash
   composer require <vendor>/<name>:<version>
   ```

   Por ejemplo,

   ```bash
   composer require zero1/theme-fashionista-theme:1.0.0
   ```

1. Espere a que se actualicen las dependencias.
1. Introduzca los siguientes comandos:

   ```bash
   git add -A && git commit -m "Add theme"
   ```

   ```bash
   git push origin <branch name>
   ```

1. Inicie sesión en Admin.
1. Haga clic en **Contenido** > Diseño > **Temas**.

   La temática se muestra en el panel derecho.

## Múltiples temas

Cuando utilice varias temáticas, como diferentes temáticas por configuración regional, revise la variable de entorno `SCD_MATRIX` para personalizar la implementación de temáticas. Consulte las fases [build](../environment/variables-build.md#scd_matrix) o [deploy](../environment/variables-deploy.md#scd_matrix) en la [configuración del entorno](../environment/configure-env-yaml.md).
