---
title: Recuperar de un error de componente
description: Descubra cómo puede recuperarse si un componente no se implementa correctamente en Adobe Commerce en la infraestructura en la nube.
feature: Cloud, Deploy
exl-id: f5e79366-5548-40dd-ac2a-56dc84c5d4e2
TQID: https://experienceleague.adobe.com/1krotAWilGcnp3OUMk-fl5iHFObGpWFUC8EpYSPjQfU
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 220
ht-degree: 0%

---

# Recuperar de un error de componente

En este tema se explica cómo recuperar si un componente no se implementa correctamente. Algunos ejemplos típicos son componentes que tienen dependencias que su entorno remoto no cumple, como versiones de PHP incompatibles.

Puede recuperarse de una implementación fallida de cualquiera de las siguientes maneras:

- [Restaurar una copia de seguridad](../storage/snapshots.md#restore-a-snapshot)
- Limpiar el proyecto y el código de cambios anteriores y volver a implementarlos

## Limpiar, quitar y volver a implementar

Para realizar una limpieza desde la implementación anterior, identifique el componente que se añadió o actualizó y, a continuación, elimínelo. Primero, inicie sesión en el entorno remoto y borre manualmente el contenido del directorio `var`. A continuación, quite el componente del archivo `composer.json` y vuelva a implementar el entorno.

**Para limpiar los `var` directorios**:

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. Utilice SSH para iniciar sesión en el entorno remoto.

   ```bash
   magento-cloud ssh
   ```

1. Borrar los directorios `var`.

   ```shell
   rm -rf var/*
   ```

1. Cerrar sesión.

**Para quitar el componente**:

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. Borre la caché.

   ```bash
   composer clear-cache
   ```

1. Quitar el componente del archivo `composer.json`.

   ```bash
   composer remove <component-name>:<version>
   ```

   Si se muestra el siguiente mensaje, no es necesario que haga nada más:

   ```
   Package "<name>:<version>" listed for update is not installed. Ignoring.
   ```

1. Espere mientras se actualizan las dependencias.

1. Agregar, confirmar y enviar cambios de código.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "<message>"
   ```

   ```bash
   git push origin <environment-ID>
   ```

{{redeploy-warning}}

Vea más información sobre cómo restaurar un entorno sin una copia de seguridad en [Restaurar un entorno](../development/restore-environment.md).

{{stuck-deployment-tip}}
