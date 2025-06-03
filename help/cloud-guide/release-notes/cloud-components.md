---
title: Componentes de nube para Commerce
description: Consulte la lista de las mejoras más recientes en el paquete de componentes en la nube.
recommendations: noDisplay, catalog
exl-id: 34aec593-e2ea-4060-a6b9-6f4cb95a11c0
source-git-commit: dcf71ffbdafae46e6a02735c090c33a8fe248bc6
workflow-type: tm+mt
source-wordcount: '637'
ht-degree: 0%

---

# Componentes de nube para Commerce

El paquete [Componentes de nube](https://github.com/magento/magento-cloud-components) proporciona funcionalidad principal extendida de Adobe Commerce para los sitios implementados en la infraestructura de la nube. Este paquete es una dependencia para el paquete ECE-Tools. En estas notas de la versión se describen las mejoras más recientes realizadas en este paquete, que es un componente de [Cloud Tools Suite para Commerce](cloud-tools-suite.md).

El paquete `magento/magento-cloud-components` usa la siguiente secuencia de versiones: `<major>.<minor>.<patch>`

Las notas de la versión incluyen:

- ![nuevo icono](../../assets/new.svg) Nuevas características
- ![icono de corrección](../../assets/fix.svg) Correcciones y mejoras

<!--Add release notes below-->

## Versión 1.1.2 {#latest}

Fecha de publicación: 3 de junio de 2025

- ![Icono de corrección](../../assets/fix.svg) **Se ha mejorado la compatibilidad con las bibliotecas de terceros de 2.4.8** y se han actualizado para mejorar la compatibilidad con 2.4.8<!-- MCLOUD-13707	 - -->

## Versión 1.1.1

Fecha de publicación: 6 de febrero de 2025

- ![nuevo icono](../../assets/new.svg) **PHP 8.4**—Se agregó compatibilidad con PHP 8.4.<!-- MCLOUD-13148	 - -->
- ![icono de corrección](../../assets/fix.svg) **Corrección para el calentamiento de la caché**—Se ha corregido un problema con las direcciones URL de categoría durante el calentamiento de la caché.<!-- MCLOUD-12454 - -->


## Versión 1.1.0

Fecha de la versión: 7 de octubre de 2024

- ![Icono de corrección](../../assets/fix.svg) **Código refactorizado**—Se ha eliminado la compatibilidad con las versiones 7.4, 7.3, 7.2 y bibliotecas relacionadas de PHP anteriores.<!-- MCLOUD-9278 - -->
- ![Icono de corrección](../../assets/fix.svg) **Versión de monólogo actualizada**: se ha agregado compatibilidad con monólogo 3.6.<!-- MCLOUD-12855 - -->

## Versión 1.0.14

Fecha de publicación: 8 de abril de 2024

- ![nuevo icono](../../assets/new.svg) **PHP**—Se agregó compatibilidad con PHP 8.3.

## Versión 1.0.13

Fecha de la versión: 10 de marzo de 2023

- ![nuevo icono](../../assets/new.svg) **Soporte mejorado para PHP 8.2**—Se corrigieron problemas de compatibilidad con ciertas versiones de PHP 8.2.x para admitir Commerce 2.4.6.

## Versión 1.0.12

Fecha de la versión: 13 de septiembre de 2022

- ![Icono de corrección](../../assets/fix.svg) **Errores en el calentamiento**—Se ha corregido un problema que intentaba [calentar](../environment/variables-post-deploy.md#warm_up_pages) cuando la visibilidad de la página se establece en [**No visible de forma individual**](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/data-transfer/data-attributes-product#simple-product-csv-file-structure) en el administrador, lo que da como resultado `ERROR: Warming up failed: <link to page>` errores en el registro de implementación.<!-- MCLOUD-9134 -->

## Versión 1.0.11

Fecha de lanzamiento: 4 de agosto de 2022

- ![Icono de corrección](../../assets/fix.svg) **Se agregó compatibilidad con Symfony 5.4**—Correcciones de compatibilidad con Symfony 5.4.<!-- AC-3550 -->

## Versión 1.0.10

Fecha de la versión: 10 de marzo de 2022

- ![nuevo icono](../../assets/new.svg) **Soporte PHP 8.1**—Se agregó soporte para PHP 8.1 y se eliminó soporte para PHP 7.1.

## Versión 1.0.9

Fecha de la versión: 25 de octubre de 2021

- ![Icono de corrección](../../assets/fix.svg) **Actualizar monólogo**: se ha actualizado la versión mínima requerida para el paquete `monolog` a `^2.3`.<!-- ACMP-1263 -->

## Versión 1.0.8

Fecha de la versión: 29 de julio de 2021

- ![Icono de corrección](../../assets/fix.svg) **Se eliminaron las barras finales de las direcciones URL generadas automáticamente**—Se eliminaron las barras finales de las direcciones URL de página de categoría generadas durante la calentamiento de la caché.<!--MCLOUD-7192-->

## Versión 1.0.7

Fecha de la versión: 9 de septiembre de 2020

- ![nuevo icono](../../assets/new.svg) **Mejoras en el registro**: reduzca el tamaño del archivo `cache.log` para mejorar el rendimiento.<!--MCLOUD-6859-->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un error de tipo en los valores de configuración de caché que hacía que fallara el comando CLI `php bin/magento cache:evict`.

## Versión 1.0.6

Fecha de lanzamiento: 5 de agosto de 2020

- ![nuevo icono](../../assets/new.svg) **Mejorar el rendimiento de Redis**—Se ha agregado el comando `./bin/magento cache:evict` para quitar las claves de Redis caducadas, lo que reduce el uso de memoria de Redis para mejorar el rendimiento.<!--MCLOUD-6023-->

- ![Icono de corrección](../../assets/fix.svg) eliminó la compatibilidad con *Registros de New Relic en contexto* para corregir un problema de rendimiento.<!--MCLOUD-6422-->

## Versión 1.0.5

Fecha de publicación: 25 de junio de 2020

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema introducido en la versión 1.0.4 de magento/magento-cloud-components que hacía que la operación de vaciado de caché fallara durante la fase de implementación e interrumpiera el proceso de implementación.

## Versión 1.0.4

Fecha de publicación: 25 de junio de 2020

- ![nuevo icono](../../assets/new.svg) **Se implementaron los registros de New Relic en contexto**: los registros de aplicación generados por Adobe Commerce ahora se muestran en los seguimientos dentro de New Relic para mejorar las capacidades de solución de problemas.<!--MCLOUD-6029-->

- ![nuevo icono](../../assets/new.svg) **Registro mejorado**—Se agregó el registro para hacer un seguimiento de los eventos de invalidación de caché y reindexación completa.<!--MCLOUD-6157-->

## Versión 1.0.3

Fecha de publicación: 27 de febrero de 2020

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema de compatibilidad con `ece-tools` versiones de 2002.0.x que usan versiones de PHP anteriores.

## Versión 1.0.2

Fecha de publicación: 6 de febrero de 2020

- ![nuevo icono](../../assets/new.svg) ha ampliado la funcionalidad de la variable de entorno `WARM_UP_PAGES` para admitir la precarga de caché en páginas de producto específicas. Consulte el tema [variables posteriores a la implementación](../environment/variables-post-deploy.md#warm_up_pages) para obtener una descripción detallada de la característica.<!--MAGECLOUD-4444-->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema por el que una dirección URL de almacén no válida hacía que fallara el vínculo posterior a la implementación al usar la funcionalidad `WARM_UP_PAGES` para rellenar la caché. Este problema se produjo solamente cuando se deshabilitaron las reescrituras de URL.<!-- MAGECLOUD-4094 -->

## Versión 1.0.1

Fecha de la versión: 23 de julio de 2019

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que afectaba a la funcionalidad de [**WARM_UP_PAGES**](../environment/variables-post-deploy.md#warm_up_pages) y que usa una dirección URL de almacén predeterminada. Ahora, si el comando `config:show:default-url` no puede obtener una dirección URL base, se utilizará la dirección URL de la variable MAGENTO_CLOUD_ROUTES.<!-- MAGECLOUD-3866 -->

## Versión 1.0.0

Fecha de publicación: 12 de junio de 2019

Esta es la primera versión del paquete [`magento/magento-cloud-components`](https://github.com/magento/magento-cloud-components), que es una nueva dependencia para la versión 2002.0.20 y posterior del paquete `ece-tools`.

- ![nuevo icono](../../assets/new.svg) agregó la capacidad de usar patrones regex para configurar la variable de entorno **WARM_UP_PAGES** con el fin de almacenar en caché páginas únicas, dominios múltiples y páginas múltiples. Ver [variables posteriores a la implementación](../environment/variables-post-deploy.md#warm_up_pages).<!--MAGECLOUD-3258-->
