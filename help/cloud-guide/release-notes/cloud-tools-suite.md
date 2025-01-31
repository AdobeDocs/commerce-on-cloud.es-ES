---
title: Notas de la versión de Cloud Tools Suite
description: Obtenga información sobre las últimas mejoras del conjunto de herramientas de la nube para Adobe Commerce.
feature: Cloud, Release Notes
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 1%

---

# Notas de la versión de Commerce Cloud Tools Suite

Esta información de la versión detalla las mejoras más recientes de Cloud Tools Suite para Commerce, paquetes diseñados para implementar y administrar instalaciones y actualizaciones de Adobe Commerce en la plataforma en la nube.

| Notas de la versión | Versión | Descripción | Source |
| ----------------- |-----------| ---------------------------------------- | --------------------------- |
| [`ece-tools` paquete](ece-tools-package.md) | 2002.2.0 | Un conjunto de scripts y herramientas diseñadas para administrar e implementar proyectos en la nube | [`magento/ece-tools`](https://github.com/magento/ece-tools/tree/2002.2.0) |
| [Parches de nube para Commerce](cloud-patches.md) | 1.1.0 | Un conjunto de parches que mejoran la integración de todas las versiones de Adobe Commerce con los entornos en la nube. Este paquete incluye revisiones de Adobe Commerce y revisiones disponibles que se aplican cuando se usa `ece-tools` para implementar | [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches/tree/1.1.0) |
| [Cloud Docker para Commerce](cloud-docker.md) | 1.4.0 | Archivos de funcionalidad y configuración para imágenes de Docker para implementar Adobe Commerce en un entorno de nube local | [`magento/magento-cloud-docker`](https://github.com/magento/magento-cloud-docker/tree/1.0) |
| [Componentes de nube de Commerce](cloud-components.md) | 1.1.0 | Funcionalidad principal extendida de Adobe Commerce para sitios implementados en la infraestructura en la nube | [`magento/magento-cloud-components`](https://github.com/magento/magento-cloud-components/tree/1.1.0) |

Al actualizar a ECE-Tools 2002.1.0 o posterior, se actualiza automáticamente a las versiones más recientes de los otros paquetes, que son dependencias del paquete `ece-tools`. Consulte [Metapackage de nube](../development/overview.md#cloud-metapackage) para obtener una lista de dependencias.

La nueva versión de `ece-tools` (2002.2.0) solo está disponible con PHP versión 8.1 y posteriores (8.2, 8.3). Las versiones anteriores de PHP estaban obsoletas (7.4, 7.3, 7.2). Puede usar versiones anteriores de `ece-tools` con versiones de PHP antiguas.
