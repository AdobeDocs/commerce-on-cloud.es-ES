---
title: Propiedad Hooks
description: Vea ejemplos sobre cómo configurar la propiedad hooks en el archivo de configuración de la aplicación  [!DNL Commerce] id.
feature: Cloud, Configuration, Build, Deploy
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '316'
ht-degree: 0%

---

# Propiedad Hooks

Utilice la sección `hooks` para ejecutar comandos del shell durante las fases de generación, implementación y posterior a la implementación:

- **`build`**: ejecute comandos _antes de_ empaquetar la aplicación. Los servicios, como la base de datos o Redis, no están disponibles porque la aplicación aún no se ha implementado. Agregue comandos personalizados _antes_ del comando predeterminado `php ./vendor/bin/ece-tools` para que el contenido generado a medida continúe con la fase de implementación.

- **`deploy`**: ejecute comandos _después de_ empaquetar e implementar la aplicación. En este punto, puede acceder a otros servicios. Dado que el comando predeterminado `php ./vendor/bin/ece-tools` copia el directorio `app/etc` en la ubicación correcta, debe agregar comandos personalizados _después_ al comando deploy para evitar que los comandos personalizados produzcan errores.

- **`post_deploy`**: ejecute comandos _después de_ implementar su aplicación y _después de_ el contenedor comienza a aceptar conexiones. El vínculo `post_deploy` borra la caché y la precarga (calienta). Puede personalizar la lista de páginas mediante la variable `WARM_UP_PAGES` en la [fase posterior a la implementación](../environment/variables-post-deploy.md). Aunque no es obligatorio, funciona en conjunto con la variable de entorno `SCD_ON_DEMAND`.

El ejemplo siguiente muestra la configuración predeterminada en el archivo `.magento.app.yaml`. Agregue comandos CLI en las secciones `build`, `deploy` o `post_deploy` _antes_ del comando `ece-tools`:

```yaml
hooks:
    # We run build hooks before your application has been packaged.
    build: |
        set -e
        composer install
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    # We run deploy hook after your application has been deployed and started.
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml
    # We run post deploy hook to clean and warm the cache. Available with ECE-Tools 2002.0.10.
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

Además, puede personalizar aún más la fase de compilación mediante los comandos `generate` y `transfer` para realizar acciones adicionales al generar código o mover archivos de forma específica.

```yaml
hooks:
    # We run build hooks before your application has been packaged.
    build: |
        set -e
        php ./vendor/bin/ece-tools build:generate
        # php /path/to/your/script
        php ./vendor/bin/ece-tools build:transfer
```

- `set -e`: hace que los enlaces fallen en el primer comando fallido, en lugar del último comando fallido.
- `build:generate`: aplica parches, valida la configuración, genera ID y genera contenido estático si SCD está habilitado para la fase de compilación.
- `build:transfer`: transfiere código generado y contenido estático al destino final.

Los comandos se ejecutan desde el directorio de la aplicación (`/app`). Puede usar el comando `cd` para cambiar el directorio. Los enlaces fallan si falla el comando final en ellos. Para provocar un error en el primer comando con error, agregue `set -e` al principio del vínculo.

**Para compilar archivos Sass usando grunt**:

```yaml
dependencies:
    ruby:
        sass: "3.4.7"
    nodejs:
        grunt-cli: "~0.1.13"

hooks:
    build: |
        cd public/profiles/project_name/themes/custom/theme_name
        npm install
        grunt
        cd
        php ./vendor/bin/ece-tools build
```

Compile archivos Sass utilizando `grunt` antes de la implementación de contenido estático, lo que sucede durante la compilación. Coloque el comando `grunt` antes del comando `build`.

{{scenarios}}
