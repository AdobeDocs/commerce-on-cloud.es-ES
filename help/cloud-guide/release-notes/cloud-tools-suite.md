---
title: Notas de la versión de Cloud Tools Suite
description: Obtenga información sobre las últimas mejoras del conjunto de herramientas de la nube para Adobe Commerce.
feature: Cloud, Release Notes
exl-id: ee2bc2e9-bdf4-4f7b-9724-8f4dd1e61378
TQID: https://experienceleague.adobe.com/eQQvGGEwj4D6pOlhZqNA-SMdc6JxH-Wg-hBRZaR1C-M
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
subfeature_v2:
  - id: db6b6496-d1b5-4ad4-9e18-dea78dae3aa8
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: 8470b1b07adc4180f2ed1f07d3987bf33f9813ac
workflow-type: tm+mt
source-wordcount: 220
ht-degree: 3%

---

# Notas de la versión de Commerce Cloud Tools Suite

Esta información de la versión detalla las mejoras más recientes de Cloud Tools Suite para Commerce, paquetes diseñados para implementar y administrar instalaciones y actualizaciones de Adobe Commerce en la plataforma en la nube.

| Notas de la versión | Versión | Descripción | Source |
| ----------------- |----------| ---------------------------------------- | --------------------------- |
| [paquete ece-tools](ece-tools-package.md) | 2002.2.11 | Un conjunto de scripts y herramientas diseñadas para administrar e implementar proyectos en la nube | [`magento/ece-tools`](https://github.com/magento/ece-tools/tree/2002.2.11) |
| [Parches de nube para Commerce](cloud-patches.md) | 1.1.15 | Un conjunto de parches que mejoran la integración de todas las versiones de Adobe Commerce con los entornos en la nube. Este paquete incluye revisiones de Adobe Commerce y revisiones disponibles que se aplican cuando se usa `ece-tools` para implementar | [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches/tree/1.1.15) |
| [Cloud Docker para Commerce](cloud-docker.md) | 1.4.8 | Archivos de funcionalidad y configuración para imágenes de Docker para implementar Adobe Commerce en un entorno de nube local | [`magento/magento-cloud-docker`](https://github.com/magento/magento-cloud-docker/tree/1.4.8) |
| [Componentes de nube de Commerce](cloud-components.md) | 1.1.4 | Funcionalidad principal extendida de Adobe Commerce para sitios implementados en la infraestructura en la nube | [`magento/magento-cloud-components`](https://github.com/magento/magento-cloud-components/tree/1.1.4) |

Al actualizar a ECE-Tools 2002.1.0 o posterior, se actualiza automáticamente a las versiones más recientes de los otros paquetes, que son dependencias del paquete `ece-tools`. Consulte [Metapackage de nube](../development/overview.md#cloud-metapackage) para obtener una lista de dependencias.
