---
title: Actualizar el paquete ECE-Tools
description: Aprenda a actualizar el paquete ECE-Tools para aprovechar las últimas correcciones y funciones aplicadas a Adobe Commerce en la infraestructura en la nube.
feature: Cloud, Upgrade
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 0%

---

# Actualizar el paquete ECE-Tools

Una actualización del paquete `ece-tools` también actualiza los otros paquetes de [Cloud Tools Suite para Commerce](../release-notes/cloud-tools-suite.md), que son dependencias de `ece-tools`. Por lo tanto, debe usar una versión de Adobe Commerce en la infraestructura en la nube que admita el paquete `ece-tools`.

{{ece-tools-package}}

**Requisitos previos**:

- Antes de actualizar `ece-tools`, revise las [notas de la versión de Cloud Tools Suite para Commerce](../release-notes/cloud-tools-suite.md).
- Si actualiza desde `ece-tools` 2002.0.22 o anterior a 2002.1.0, revise [cambios incompatibles con versiones anteriores](../release-notes/backward-incompatible-changes.md) y realice los cambios necesarios en su proyecto de Adobe Commerce en la nube.
- Revise [Actualizaciones y parches](../development/commerce-version.md#upgrade-from-older-versions) para determinar las versiones de ECE-Tools compatibles con su proyecto de Adobe Commerce en la nube.

{{upgrade-tip}}

**Para actualizar el paquete `ece-tools`**:

1. En la estación de trabajo local, realice una actualización con Composer.

   ```bash
   composer update magento/ece-tools --with-dependencies
   ```

   >[!NOTE]
   >
   >Si no puede actualizar más allá de `ece-tools` versión 2002.0.8, vea [Actualizar el proyecto para usar el paquete ECE-Tools](install-package.md).

1. Agregar, confirmar y enviar cambios de código.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Update magento/ece-tools"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. Después de la validación de prueba, combine esta rama con la rama de integración.
