---
title: Ejemplo de administración de la configuración específica del sistema
description: Consulte un ejemplo de cómo administrar y sincronizar las opciones de configuración de tienda en todos los entornos de Adobe Commerce en la nube.
hidefromtoc: true
source-git-commit: 0df07e865c3c4fc4ac14483972643eafa8814726
workflow-type: tm+mt
source-wordcount: '864'
ht-degree: 0%

---


# Ejemplo de administración de la configuración específica del sistema

Este ejemplo muestra cómo utilizar la administración de la configuración para mantener la coherencia de la configuración de las tiendas en todos los entornos.

El ejemplo usa el siguiente procedimiento definido en [Configuración de almacenamiento](store-settings.md):

1. Introduzca sus configuraciones en el entorno de integración y almacene el administrador.
1. Cree un archivo de `config.php` y transfiéralo a la estación de trabajo local.
1. Insertar `config.php` en el entorno de integración remota.
1. Compruebe que la configuración no se pueda editar en Admin.
1. Realice las modificaciones necesarias:

   * Cambie las opciones de configuración en el entorno de integración.
   * Para agregar configuraciones, ejecute de nuevo el comando para crear `config.php`. Las nuevas configuraciones se anexan al archivo.
   * Para quitar o editar las configuraciones existentes, edite manualmente el archivo.
   * Compromiso y push.

Por ejemplo, es posible que desee establecer la siguiente configuración:

* Deshabilite la configuración regional y de optimización de archivos estáticos en el entorno de integración
* Habilitar la optimización de archivos estáticos en entornos de ensayo y producción
* Configure Fastly en Ensayo y producción con credenciales específicas para cada

_Optimización de archivo estático_ significa combinar y minificar hojas de estilo en cascada y JavaScript, y minificar plantillas de HTML. Consulte [Estrategias de implementación de contenido estático](../deploy/static-content.md).

## Requisitos previos

Para completar estas tareas de administración de la configuración, necesita lo siguiente:

* Rol de lector de proyecto con privilegios de [administrador del entorno](../project/user-access.md)
* URL y credenciales de administrador para los entornos de integración, ensayo y producción

## Configuración del administrador de Commerce

En el entorno de integración, puede iniciar sesión en Admin para modificar los ajustes de configuración del sistema para tiendas, sitios web, módulos o extensiones, optimización de archivos estáticos y valores del sistema relacionados con la implementación de contenido estático. Ver [datos de configuración](store-settings.md#scd-performance).

**Para cambiar la configuración regional y de optimización de archivos estáticos**:

1. Inicie sesión en el administrador del entorno de integración. Puede obtener acceso a esta dirección URL a través de [[!DNL Cloud Console]](../project/overview.md).
1. Vaya a **Tiendas** > Configuración > **Configuración** > General > **General**.
1. En la navegación de la página, expanda **Opciones de configuración regional**.
1. En la lista **Configuración regional**, cambie la configuración regional. Puede volver a cambiarlo más tarde.

   ![Cambiar la configuración regional](../../assets/locale-options.png)

1. Haga clic en **Guardar configuración**.
1. Si se le solicita, [vacíe la caché](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/tools/cache-management).
1. Cierre la sesión del administrador.

## Exportar valores y transferir config.php a su sistema local

Este paso crea y transfiere el archivo de configuración `config.php` en el entorno de integración mediante un comando que ejecuta en el equipo local.

Este procedimiento corresponde al paso 2 del [procedimiento recomendado](store-settings.md). Después de crear `config.php`, transfiéralo al sistema local para que pueda agregarlo a Git.

**Para crear y transferir`config.php`**:

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. Cambio en el entorno de integración de.

   ```bash
   magento-cloud environment:checkout integration
   ```

1. Cree un volcado local de la base de datos remota.

   ```bash
   magento-cloud db:dump
   ```

El siguiente fragmento de `config.php` muestra un ejemplo de cómo cambiar la configuración regional predeterminada a `en_GB` y cambiar la configuración de optimización de archivos estáticos:

```php?start_inline=1
'general' => [
     'locale' => [
         'code' => 'en_GB',
         'timezone' => 'UTC',
     ],

     ... more ...

 'dev' => [
     'template' => [
         'allow_symlink' => '0',
         'minify_html' => '0',
     ],
     'js' => [
         'merge_files' => '0',
         'enable_js_bundling' => '0',
         'minify_files' => '0',
     ],
     'css' => [
         'merge_css_files' => '0',
         'minify_files' => '0',
     ],

     ... more ...
```

## Insertar e implementar config.php en entornos

Ahora que ha creado `config.php` y lo ha transferido a su sistema local, configúrelo en Git y envíelo a sus entornos. Este procedimiento corresponde a los pasos 3 y 4 del [procedimiento recomendado](store-settings.md).

El siguiente comando agrega, confirma y inserta la rama `master`:

```bash
git add app/etc/config.php && git commit -m "Add system-specific configuration" && git push origin master
```

Implementación completa del código en Ensayo y producción. Para empezar, debe insertar `staging` y `master` ramas. Para obtener más información sobre los comandos de implementación, consulte [Implementar tu tienda](../deploy/staging-production.md).

Espere a que se complete la implementación en todos los entornos.

## Verifique los cambios de configuración

Después de insertar `config.php` en sus entornos, cualquier valor que haya cambiado debería ser de solo lectura en el Administrador. En este ejemplo, la configuración regional predeterminada modificada y la configuración de optimización de archivos estáticos no deben poder editarse en Admin. Estas opciones de configuración se establecen en `config.php`.

Para comprobar los cambios de configuración:

1. Cierre la sesión del administrador en uno de los entornos.
1. Vuelva a iniciar sesión en Admin.
1. Haga clic en **Tiendas** > Configuración > **Configuración** > General > **General**.
1. En el panel derecho, expanda **Opciones locales**.

   Tenga en cuenta que no se pueden editar varios campos, como se muestra en el siguiente ejemplo. Estas opciones de configuración las mantiene `config.php`.

   ![Algunos valores ya no pueden editarse en el administrador](../../assets/locale-options-disabled.png)

1. Cierre la sesión del administrador.

## Cambiar y actualizar la configuración específica del sistema

Si necesita modificar cualquiera de estas opciones, modifique el archivo `config.php` manualmente con un editor de texto. Después de completar las ediciones o eliminaciones, puede confirmarlo y enviarlo al entorno remoto siguiendo los pasos anteriores.

Para agregar configuraciones, modifique el entorno de integración y ejecute de nuevo el comando para generar el archivo. Las nuevas configuraciones se anexan al código del archivo. Colóquelo en Git siguiendo los pasos anteriores.

Para este ejemplo, modifique la configuración de optimización de archivos estáticos y agregue una nueva configuración para JavaScript.

### Añadir configuraciones en la integración

Para añadir valores de configuración en el entorno de integración Administrador. En este ejemplo se combinan archivos JavaScript.

1. Cierre la sesión del administrador de integración.
1. Vuelva a iniciar sesión en el administrador de integración.
1. Haga clic en **Tiendas** > Configuración > **Configuración** > **Avanzado** > **Desarrollador**.
1. En el panel derecho, expanda **Configuración de JavaScript**.
1. En la lista **Combinar archivos de JavaScript**, haga clic en **Sí**.
1. Haga clic en **Guardar configuración**.
1. Si se le solicita, [vacíe la caché](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/tools/cache-management).
1. Cierre la sesión del administrador.

Al volver a ejecutar el comando de volcado, la nueva configuración se anexa al archivo.

```bash
magento-cloud db:dump
```

### Editar config.php con nuevos ajustes

En el archivo local, use un editor de texto para editar el archivo `app/etc/config.php` actualizado. Edite esta configuración para habilitar la minificación para archivos JavaScript, HTML y CSS.

```php?start_inline=1
 'dev' => [
     'template' => [
         'allow_symlink' => '0',
         'minify_html' => '0',
     ],

     ... more ...

     'js' => [
         'merge_files' => '0',
         'enable_js_bundling' => '0',
         'minify_files' => '0',
     ],
     'css' => [
         'merge_css_files' => '0',
         'minify_files' => '0',
     ],
```

Para modificar la configuración con el fin de permitir la minificación, edite `'0'` en `'1'` para `'minify_html'` y cada opción de `'minify_files'`:

```php?start_inline=1
 'dev' => [
     'template' => [
         'allow_symlink' => '0',
         'minify_html' => '1',
     ],

     ... more ...

     'js' => [
         'merge_files' => '0',
         'enable_js_bundling' => '0',
         'minify_files' => '1',
     ],
     'css' => [
         'merge_css_files' => '0',
         'minify_files' => '1',
     ],
```

Guarde los cambios en el archivo.

### Insertar los cambios en Git

Para insertar los cambios, escriba lo siguiente:

```bash
git add app/etc/config.php
```

```bash
git commit -m "Add system-specific configuration and edit settings"
```

```bash
git push origin master
```

Espere a que se complete la implementación.

Repita el proceso de implementación para insertar el código en todos los entornos.
