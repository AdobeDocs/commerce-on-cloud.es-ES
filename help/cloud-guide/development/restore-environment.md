---
title: Restaurar un entorno
description: Obtenga información sobre cómo desinstalar la aplicación de Adobe Commerce de un proyecto de infraestructura en la nube y restaurar un entorno a un estado estable.
role: Developer
topic: Development
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '493'
ht-degree: 0%

---

# Restaurar un entorno

Si encuentra problemas en el entorno de integración y no tiene una [copia de seguridad válida](../storage/snapshots.md), o si desea restablecer el entorno a una pizarra en blanco, puede restaurar o restablecer su entorno mediante uno de los siguientes métodos:

- Restablecer o revertir el código en la rama Git
- Desinstalar la aplicación [!DNL Commerce]
- Forzar un redespliegue
- Restablecer manualmente la base de datos

{{stuck-deployment-tip}}

## Restablecer la rama Git

Restablecer la rama Git convierte el código a un estado estable en el pasado.

**Para restablecer su rama**:

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. Revise el historial de confirmaciones de Git. Usar `--oneline` para mostrar confirmaciones abreviadas en una línea:

   ```bash
   git log --oneline
   ```

   Respuesta de ejemplo:

   ```
   6bf9f45 (HEAD -> master, magento/master, magento/develop, magento/HEAD, develop) Create composer.lock
   34d7434 2.4.6 upgrade
   b69803c Update composer.lock
   c1bca24 Add sample data
   ec604c3 Update magento/ece-tools
   ...
   ```

1. Elija un hash de confirmación que represente el último estado estable conocido del código.

   Para restablecer la rama a su estado inicializado original, busque la primera confirmación que creó la rama. Puede usar `--reverse` para mostrar el historial en orden cronológico inverso.

1. Utilice la opción de restablecimiento completo para restablecer la rama. Tenga cuidado al utilizar este comando, ya que descarta todos los cambios desde la confirmación elegida.

   ```bash
   git reset --hard <commit>
   ```

1. Inserte los cambios para almacenar en déclencheur una nueva implementación, que reinstala Adobe Commerce.

   ```bash
   git push --force <origin> <branch>
   ```

## Desinstalación de Commerce

Al desinstalar la aplicación [!DNL Commerce], el entorno vuelve a un estado original; para ello, restaura la base de datos, quita la configuración de implementación y borra los subdirectorios `var/`. Esta guía también restablece la rama de Git a un estado estable anterior. Si no tiene una copia de seguridad reciente, pero puede acceder al entorno remoto mediante SSH, siga estos pasos para restaurar el entorno:

- Deshabilitar la administración de configuración
- Desinstalación de Adobe Commerce
- Restablezca la rama de Git

Al desinstalar el software de Adobe Commerce, se borra y restaura la base de datos, se quita la configuración de implementación y se borran los subdirectorios `var/`. Es importante deshabilitar [Administración de configuración](../store/store-settings.md) para que no aplique automáticamente las opciones de configuración anteriores durante la siguiente implementación. Asegúrese de que el directorio `app/etc/` no contenga el archivo `config.php`.

**Para desinstalar el software de Adobe Commerce**:

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. Utilice SSH para iniciar sesión en el entorno remoto.

   ```bash
   magento-cloud ssh
   ```

1. Elimine el archivo de configuración.
   - Para Adobe Commerce 2.2 y versiones posteriores:

     ```bash
     rm app/etc/config.php
     ```

   - Para Adobe Commerce 2.1:

     ```bash
     rm app/etc/config.local.php
     ```

1. Desinstale la aplicación de Adobe Commerce.

   ```bash
   php bin/magento setup:uninstall -n
   ```

1. Confirme que Adobe Commerce se ha desinstalado correctamente.

   El siguiente mensaje se muestra para confirmar que la desinstalación se ha realizado correctamente:

   ```
   [SUCCESS]: Magento uninstallation complete.
   ```

1. Borre los subdirectorios `var/`.

   ```bash
   rm -rf var/*
   ```

1. Cerrar sesión.

>[!TIP]
>
>De forma opcional, se recomienda limpiar las cachés de compilación.
>
>```bash
>magento-cloud project:clear-build-cache
>```

## Forzar un redespliegue

Si ha intentado desinstalar Adobe Commerce y la implementación sigue fallando, puede intentar forzar manualmente una reimplementación.

```bash
git commit --allow-empty -m "<message>" && git push <origin> <branch>
```

## Restablecer la base de datos

Si ha intentado desinstalar Adobe Commerce y el comando ha fallado o no se ha podido completar, puede restablecer manualmente la base de datos.

**Para restablecer la base de datos**:

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. Utilice SSH para iniciar sesión en el entorno remoto.

   ```bash
   magento-cloud ssh
   ```

1. Conéctese a la base de datos.

   ```bash
   mysql -h database.internal
   ```

1. Soltar la base de datos `main`.

   ```shell
   drop database main;
   ```

1. Crear una base de datos `main` vacía.

   ```shell
   create database main;
   ```

1. Elimine los siguientes archivos de configuración.

   - `config.php`
   - `config.php.bak`
   - `env.php`
   - `env.php.bak`

1. Cierre la sesión y almacene en déclencheur una nueva implementación.

   ```bash
   magento-cloud environment:redeploy
   ```

{{redeploy-warning}}
