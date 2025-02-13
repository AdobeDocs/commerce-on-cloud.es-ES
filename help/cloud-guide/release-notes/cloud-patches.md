---
title: Parches de nube para Commerce
description: Consulte la lista de las mejoras más recientes del paquete Parches en la nube.
recommendations: noDisplay, catalog
last-substantial-update: 2025-02-06T00:00:00Z
exl-id: a4454ebc-72a4-42c1-b591-6237c97fe913
source-git-commit: 4c8da3e40561a43674906cdf7f461bbcb1066c30
workflow-type: tm+mt
source-wordcount: '2347'
ht-degree: 0%

---

# Parches de nube para Commerce

El paquete [Parches de nube](https://github.com/magento/magento-cloud-patches) proporciona un conjunto de parches necesarios que mejoran la integración de todas las versiones de Adobe Commerce con los entornos de nube y admite la entrega rápida de correcciones críticas.

El paquete Parches de nube para Commerce es una dependencia para el paquete ECE-Tools y se instala y actualiza al instalar o actualizar el paquete ECE-Tools. También puede utilizar y administrar Parches de nube para Commerce como paquete independiente para aplicar parches a un proyecto de Adobe Commerce que no esté en la plataforma en la nube. Estas notas de la versión describen las mejoras más recientes realizadas en este paquete.

>[!TIP]
>
>Para asegurarse de que su proyecto tenga todas las revisiones necesarias, actualice a la [última versión de ece-tools](../dev-tools/update-package.md).

>[!NOTE]
>
>Consulte [Aplicar parches](../development/apply-patches.md) para obtener instrucciones sobre cómo aplicar parches a sus proyectos.

El paquete `magento/magento-cloud-patches` usa la siguiente secuencia de versiones: `<major>.<minor>.<patch>`

<!--Add release notes below-->

## Versión 1.1.4 {#latest}

Fecha de publicación: 13 de febrero de 2025

- ![nuevo icono](../../assets/new.svg) **Se ha agregado un parche para Commerce 2.4.4 a 2.4.7**—Esta actualización revisa [CVE-2025-24434](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/security-update-available-for-adobe-commerce-apsb25-08).<!-- MCLOUD-13240	 - -->

## Versión 1.1.3

Fecha de publicación: 6 de febrero de 2025

- ![nuevo icono](../../assets/new.svg) **PHP 8.4**—Se agregó compatibilidad con PHP 8.4.<!-- MCLOUD-13149	 - -->

## Versión 1.1.2

Fecha de la versión: 5 de noviembre de 2024

- ![Icono de corrección](../../assets/fix.svg) **Se agregó un parche para Commerce 2.4.4 a 2.4.7**—Esta actualización corrige una vulnerabilidad crítica de [CVE-2024-45115](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/security-update-available-for-adobe-commerce-apsb24-73) para Adobe Commerce al usar el módulo B2B.<!-- MCLOUD-12980 - -->

## Versión 1.1.1

Fecha de la versión: 5 de noviembre de 2024

- ![Icono de corrección](../../assets/fix.svg) **Se ha agregado un parche para Commerce 2.4.4 a 2.4.7**—Esta actualización corrige una vulnerabilidad crítica de [CVE-2024-34102](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/security-update-available-for-adobe-commerce-apsb24-40-revised-to-include-isolated-patch-for-cve-2024-34102?lang=en) CosmicString.<!-- MCLOUD-12980 - -->

## Versión 1.1.0

Fecha de la versión: 7 de octubre de 2024

- ![Icono de corrección](../../assets/fix.svg) **Código refactorizado**—Se ha eliminado la compatibilidad con las versiones antiguas de PHP (7.4, 7.3, 7.2) y con las bibliotecas relacionadas.<!-- MCLOUD-9278 - -->
- ![Icono de corrección](../../assets/fix.svg) **Versión de monólogo actualizada**: se ha agregado compatibilidad con monólogo 3.6.<!-- MCLOUD-12855 - -->
- ![icono de corrección](../../assets/fix.svg) **Parche para Application Server**: resuelve un problema conocido con GraphQL Application Server. En concreto, `CatalogGraphQl\\Model\\Config\\AttributeReader` en la versión 2.4.7 contenía un error que podría provocar que las solicitudes de GraphQL recuperen respuestas basadas en una configuración de Atributos obsoleta.<!-- ACPT-1876 -->

## Versión 1.0.27

Fecha de la versión: 21 de mayo de 2024

- **Soporte para PHP 8.3**—Este parche resuelve errores de compatibilidad entre php 8.3 y la versión del paquete del compositor.

## Versión 1.0.26

Fecha de publicación: 8 de abril de 2024

- ![nuevo icono](../../assets/new.svg) **PHP** — Se agregó compatibilidad con PHP 8.3.

## Versión 1.0.25

Fecha de la versión: 16 de enero de 2024

- **Mejoras en la caché**: este parche mejora la eficacia de la caché de diseño y reduce considerablemente el uso de memoria para las versiones 2.4.4 y posteriores de Adobe Commerce.<!-- MCLOUD-11514 -->
- **Mejoras en los trabajos de CRON**: este parche corrige el problema por el que los trabajos perdidos esperan innecesariamente bloqueos de trabajos de CRON para las versiones 2.4.4 y posteriores de Adobe Commerce.<!-- MCLOUD-11329 -->

## Versión 1.0.24

Fecha de la versión: 15 de septiembre de 2023

- **Mejora del rendimiento**: este parche corrige un problema que afecta al rendimiento al reducir el número de veces que se cargan las mismas configuraciones de implementación para Adobe Commerce 2.4.6 a 2.4.6-p1<!-- MCLOUD-10604 -->

## Versión 1.0.23

Fecha de la versión: 31 de julio de 2023

- **Se quitó el parche MCLOUD-10604**- Este parche se movió a QPT.<!-- MCLOUD-10736 -->

## Versión 1.0.22

Fecha de publicación: 19 de junio de 2023

- **Asistente/salida de CLI de QPT mejorada**: se ha agregado una advertencia al asistente/salida de CLI de QPT que le recuerda que debe comprobar los detalles y requisitos de la revisión si hay dependencias.<!-- ACP2E-1963 -->
- **Se agregaron parches para Commerce 2.4.6:**
   - Se corrigió la validación `regexp cache tag`.<!-- MCLOUD-10226 -->
   - Se mejoró el rendimiento al reducir el número de veces que se cargan las mismas configuraciones de implementación.<!-- MCLOUD-10604 -->
- **Se agregaron parches para Commerce 2.3.7 a 2.4.6**—Se corrigió un problema que causaba un incremento por un valor aleatorio en lugar de un incremento por 1 para las tablas `catalog_product_entity_*`.<!-- MCLOUD-10032 -->
- **Se agregaron parches para Commerce 2.4.0 a 2.4.6**—Se corrigió un error que indica que `The file can't be deleted. Warning!unlink: No such file or directory`, que se produjo al vaciar la caché JS/CSS del administrador.<!-- MCLOUD-10279 -->

## Versión 1.0.21

Fecha de la versión: 10 de marzo de 2023

- **Compatibilidad mejorada con PHP 8.2**: se corrigieron problemas de compatibilidad con ciertas versiones de PHP 8.2.x para admitir Commerce 2.4.6.

## Versión 1.0.20

Fecha de la versión: 27 de octubre de 2022

- **Se ha agregado el parche** para las mejoras en la caché L2. Este parche corrige un problema que había al vaciar la caché L2 local para las versiones 2.4.0 y 2.4.1 de Commerce.<!-- MCLOUD-7845 -->

## Versión 1.0.19

Fecha de la versión: 13 de septiembre de 2022

- **Compatibilidad mejorada con PHP 8.1**: se corrigieron problemas de compatibilidad con ciertas versiones de PHP 8.1.x.

## Versión 1.0.18

Fecha de lanzamiento: 11 de agosto de 2022

Parche crítico para Adobe Commerce 2.4.5:

- **Problema con pedidos que utilizan Braintree payments**: este parche resuelve un problema crítico que impide a los administradores realizar nuevos pedidos o repedidos.<!-- MCLOUD-9137 -->

Ver [El administrador no puede crear un pedido/repedido cuando el pago de Braintree está habilitado](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/admin-cant-create-order-reorder-when-braintree-payment-enabled.html).

## Versión 1.0.17

Fecha de la versión: 24 de mayo de 2022

Se han corregido restricciones para las revisiones de seguridad en el archivo `patches.json`.

## Versión 1.0.16

Fecha de la versión: 31 de marzo de 2022

Parche crítico para Adobe Commerce 2.3.3-p1 y versiones posteriores:

Se han actualizado los parches para resolver una vulnerabilidad **critical** que provoca la ejecución de código remoto no autenticado.<!-- MCLOUD-8479 -->

Consulte [Boletín de seguridad de Adobe APSB22-12](https://helpx.adobe.com/security/products/magento/apsb22-12.html).

## Versión 1.0.15

Fecha de la versión: 10 de marzo de 2022

- **Soporte PHP 8.1**—Se agregó soporte para PHP 8.1 y se eliminó soporte para PHP 7.0 y 7.1.
- **Se agregó el parche para Adobe Commerce 2.3.3**: se corrigió la visualización de la moneda en la página del producto.

## Versión 1.0.14

Fecha de publicación: 13 de febrero de 2022

Parche crítico para Adobe Commerce 2.3.3-p1 y versiones posteriores:

Se ha agregado un parche para resolver una vulnerabilidad **critical** que da como resultado la ejecución de código remoto no autenticado.<!-- MCLOUD-8461 -->

Consulte [Boletín de seguridad de Adobe APSB22-12](https://helpx.adobe.com/security/products/magento/apsb22-12.html).

## Versión 1.0.13

Fecha de la versión: 25 de octubre de 2021

- **Actualizar monólogo**: se ha actualizado la versión mínima requerida para el paquete `monolog` a `^2.3`.<!-- ACMP-1263 -->
- **Método PHP incompatible**: se ha corregido un método PHP incompatible para las versiones 2.4.3 y 2.3.7-p1 de Adobe Commerce.<!-- AC-384 -->
- **Error de PHP**: se corrigió un error de `PHP error 'Undefined variable: errorMessage' ...` que se produjo al intentar aplicar un parche.<!-- ACP2E-138 -->

## Versión 1.0.12

Fecha de lanzamiento: 12 de agosto de 2021

Parche crítico para Adobe Commerce 2.4.3 y 2.3.7-p1:

- **Problema con limitación de velocidad de API**: este parche corrige un límite de velocidad predeterminado que impedía que las API web procesaran solicitudes con más de 20 elementos en una matriz. Este parche aumenta el valor predeterminado del límite de velocidad. Ver las notas de la versión de Adobe Commerce [2.4.3](https://experienceleague.adobe.com/en/docs/commerce-operations/release/notes/adobe-commerce/2-4-3#apply-mc-43048__set_rate_limits__243patch-to-address-issue-with-api-rate-limiting).<!-- MC-43048 -->

## Versión 1.0.11

Fecha de la versión: 29 de julio de 2021

- **Se ha corregido un problema que se producía al aplicar el parche de navegación por capas B2B**: Para los clientes que han aplicado el parche de navegación por capas B2B, esta corrección resuelve un error de `Undefined offset` que aparece en la página Buscar después de cambiar a la vista de la tienda.<!--MCLOUD-5287-->

- **Parche de pago y envío de PayPal**: corrige un problema de Adobe Commerce 2.3.7 con PayPal Express en el que se muestra el precio del pedido realizado anteriormente.<!--MC-42674-->

- **Compatibilidad con categorías de parches**: se ha agregado compatibilidad con categorías de parches de procesamiento y fuentes de origen asignadas a Parches de calidad. Las categorías permiten a los clientes usar filtros y ordenar para buscar parches más rápidamente al usar la [Herramienta de parches de calidad](https://github.com/magento/quality-patches) y la Herramienta de análisis de todo el sitio (SWAT). <!--MC-38577-->

## Versión 1.0.10

Fecha de la versión: 10 de mayo de 2021

- **Compatibilidad con Adobe Commerce 2.3.7**: se ha resuelto un conflicto de dependencias del compositor para la instalación en Adobe Commerce 2.3.7.<!--MC-42131-->
- **Se ha corregido un problema que se producía al aplicar un parche empaquetado varias veces**. Si se aplica un parche empaquetado (uno que incluye otros parches obsoletos) más de una vez, se podrían revertir los paquetes obsoletos incluidos. Ahora todos los parches se aplican solo una vez. Al intentar aplicar el mismo paquete de nuevo, aparece un mensaje que indica que el parche ya se ha aplicado.<!--MC-41912-->
- **Parche de navegación por capas B2B**: se ha corregido otro problema que impedía que la navegación por capas mostrara todas las opciones de productos cuando el usuario habilitaba el catálogo compartido B2B.<!--MCLOUD-7742-->

## Versión 1.0.9

Fecha de publicación: 1 de febrero de 2021

- **Parche de navegación por capas B2B**: se ha corregido el problema que impedía que la navegación por capas mostrara todas las opciones de productos cuando el catálogo compartido B2B estaba habilitado.<!--MCLOUD-6923-->
- **Compatibilidad con PHP 7.4**—Se ha corregido un problema de compatibilidad de parches en la nube con PHP 7.4.<!--MCLOUD-7367-->
- **Los parches obsoletos se vuelven visibles**: se ha corregido un problema de parches en la nube por el que los parches obsoletos se vuelven visibles en la tabla de parches después de aplicar un parche de reemplazo que contiene todo el contenido del parche obsoleto. Esto podría suceder si aplicara un parche que combinara varios parches más.<!--MC-40626-->
- **Errores silenciosos al aplicar parches**: se ha corregido un problema de parches en la nube en el que el comando `git apply` no podía aplicar parches de forma silenciosa en algunos entornos.<!--MC-40529-->

## Versión 1.0.8

Fecha de la versión: 14 de octubre de 2020

- **Actualizaciones de compatibilidad para magento/magento-cloud-patch**: se han actualizado las restricciones de versión `symfony` y `semver` en el archivo `composer.json` para comprobar la compatibilidad con Adobe Commerce 2.4.1 y versiones posteriores.<!--MCLOUD-7111-->

## Versión 1.0.7

Fecha de la versión: 14 de octubre de 2020

- **Revisiones de Redis para Adobe Commerce 2.3.0 a 2.3.5, 2.4.0**: se han actualizado las revisiones de Redis para admitir la adición de productos a una categoría al implementar una caché de nivel 2. <!--MCLOUD-6659-->

- **Parche de Braintree VBE**: corrige un problema que generaba un error cuando un administrador intentaba ver un informe de liquidación de Braintree. <!--MCLOUD-6684-->

- Ahora, el comando `ece-patches apply` usa el comando Unix `patch` para aplicar parches si Git no está disponible en el sistema host. <!--MCLOUD-7069-->

## Versión 1.0.6

Fecha de lanzamiento:

- **Redis parches para Adobe Commerce 2.3.0 - 2.3.4**: optimice la comunicación y mejore el rendimiento
   - Reducción del tamaño de las transferencias de red entre Redis y Adobe Commerce
   - Corrección de condiciones de carrera en operaciones de carga y escritura de Redis
   - Reescribir el adaptador de caché base para controlar los errores al guardar
   - Reducir consumo de CPU de Redis<!--MCLOUD-6139-->

- **Redis parches para Adobe Commerce 2.3.0 - 2.3.5**: mejore el rendimiento y corrija los errores
   - Corregir la implementación de bloqueo de caché para evitar bloqueos infinitos
   - Mejora del mecanismo de bloqueo actual
   - Implementar bloqueos firmados para evitar el desbloqueo de solicitudes en paralelo
   - Corrija el siguiente error que se produce en la operación de escritura de Redis: `OOM command not allowed when used memory > maxmemory`
   - Corrija el procesamiento de la caché limpia mediante la etiqueta `cat_p` que se ejecuta durante las actualizaciones del producto<!--MCLOUD-6110-->

- Se ha corregido un problema que provocaba un error al aplicar el parche `amzn/amazon-pay-module` necesario a Adobe Commerce en proyectos de infraestructura en la nube con Adobe Commerce v2.2.6 o 2.3.5, que no incluyen este módulo. Ahora, el proceso de aplicación de parches omite la revisión `amzn/amazon-pay-module` si el módulo no está instalado.<!--MCLOUD-6588-->

## Versión 1.0.5

Fecha de publicación: 26 de junio de 2020

- **Mejoras de rendimiento de Redis**: agrega características de optimización de Redis a las versiones 2.3.3 y 2.3.4 de Adobe Commerce. Estas correcciones se incluyeron en la versión 2.3.5 de Adobe Commerce.<!--MCLOUD-5771-->

- **Enriquecidor de registro de New Relic**: agrega la interfaz de procesador monólogo necesaria para admitir mejoras en las funciones de registro de New Relic introducidas en los componentes de nube de Commerce versión 1.0.4. Este parche es necesario para implementar Adobe Commerce 2.1.x. Si no se aplica el parche, la compilación falla durante el proceso `di:compile`.<!--MCLOUD-6029-->

## Versión 1.0.4

Fecha de la versión: 12 de mayo de 2020

- **Pago y envío de Amazon Pay**: corrige un problema con el widget de pago de Amazon Pay que impedía a los clientes cambiar el método de pago en el paso _Revisión y pagos_ durante el proceso de pago.<!--MCLOUD-5930-->

- **Visualización del producto en la página de categoría**: corrige un problema que impedía que los productos se mostraran en la página de categoría en la vista _Mostrar todas las páginas_.<!--MCLOUD-5684-->

- **Carga de imagen del Page Builder**: corrige un problema en la interfaz de Page Builder que a veces causaba el siguiente error al cargar imágenes en la galería de imágenes: `Destination folder is not writable or does not exist`<!--MCLOUD-5837-->

- **Suprimir advertencias innecesarias de generación de mapas del sitio**: agrega un intento de reintento cuando se producen errores durante la generación de mapas del sitio y omite la notificación por correo electrónico al cliente en los casos en que los errores se pueden recuperar automáticamente.<!--MCLOUD-3025-->

- **Mejora del rendimiento del sitio**: corrige un problema de rendimiento con la función `Magento\Framework\App\DeploymentConfig\Reader::load`, que periódicamente experimentaba largos tiempos de carga que afectaban al rendimiento del sitio. <!--MCLOUD-5650-->

- Se ha actualizado la asignación de parches para los parches de métodos de pago con el fin de que se dirijan a los módulos de pago en lugar del paquete base de Magento (base de magento/magento2), de modo que los parches de pago se apliquen únicamente si existen los módulos de pago.<!--MCLOUD-5666-->

- Se han actualizado los parches para la compatibilidad con Magento Open Source.<!--MCLOUD-5701-->

## Versión 1.0.3

Fecha de publicación: 28 de abril de 2020

- Se ha agregado una corrección para el parche &quot;FPC se está deshabilitando durante las implementaciones&quot; para admitir Adobe Commerce 2.3.5.

## Versión 1.0.2

Fecha de publicación: 27 de febrero de 2020

Esta versión de incluye las siguientes revisiones y correcciones críticas:

- **Actualizaciones de compatibilidad para parches en la nube de Magento/Magento**

   - Se han actualizado las restricciones de versión `symfony` y `semver` en el archivo `composer.json` por compatibilidad con Adobe Commerce 2.4 y versiones posteriores.<!--MAGECLOUD-5127-->

   - Se han actualizado las restricciones de `composer.json` por compatibilidad con `ece-tools` versiones 2002.0.22 y posteriores de 2002.0.x.

- **Pago y envío de PayPal Express**: publicado el 12 de febrero de 2020, este parche resuelve un problema que afecta a los pedidos realizados con Pago y envío de PayPal Express, en los que la dirección de envío del pedido especifica una región de país que se ha introducido manualmente en el campo de texto en lugar de seleccionarla en el menú desplegable de la página Envío. Consulte la descripción completa del parche en la página de descarga del parche.

- **Corrección de implementación de aplicación**: se agregó un parche para corregir un problema que deshabilitaba la caché de página completa durante el proceso de implementación. Este parche se aplica a Adobe Commerce 2.3.2 y versiones posteriores.

- **Parámetro de ámbito para API asincrónica/masiva**: se ha actualizado este parche para corregir un error de sintaxis en el archivo `composer.json`. Este parche se aplica a Magento Open Source 2.3.1 y 2.3.2. Consulte la descripción completa del parche en la página de descarga del parche.

## Versión 1.0.1

Fecha de publicación: 6 de febrero de 2020

Hemos incluido todos los parches de Magento Open Source 2.x desde la página de descargas de software en la versión 1.0.1 de magento/magento-cloud-patch. Si ha copiado parches en el proyecto anteriormente, elimínelos para evitar conflictos.

Esta versión de incluye las siguientes revisiones y correcciones críticas:

- **Corregir interbloqueos cron y mejorar el bloqueo cron**—

   - Corrige un problema con algunos trabajos cron que no se ejecutaban debido a un valor de estado incorrecto en la tabla `cron_schedule`. Ahora, utilizamos el marco de bloqueo de Adobe Commerce para comprobar y actualizar el estado del trabajo cron en lugar de utilizar la tabla `cron_schedule`. Los trabajos cron que han finalizado con un estado de error se vuelven a intentar durante la siguiente ejecución cron en lugar de esperar 24 horas.

   - Agrega una operación _retry_ para evitar interbloqueos durante las actualizaciones de los datos en la tabla `cron_schedule`.

- **Se ha actualizado `magento/magento-cloud-patches` para incluir todos los parches disponibles para Magento Open Source 2.x**—Se ha actualizado el paquete magento/magento-cloud-patches para incluir todos los parches de Magento Open Source 2.x disponibles en la página de descargas de software. Si anteriormente copió parches de Magento Open Source en su proyecto de Adobe Commerce en la nube, quítelos para evitar conflictos.<!--MAGECLOUD-4606-->

- **Corrección de paginación del catálogo de Elasticsearch**: Se ha reemplazado el parche de paginación del catálogo de Elasticsearch entregado en magento/magento-cloud-patches v1.0 con una corrección más eficaz.<!--MAGECLOUD-4847-->

- **Parches de Page Builder**: en Parches de Cloud para Commerce 1.0.0, hemos incorporado parches de Page Builder para resolver una vulnerabilidad conocida de ejecución de código remoto (RCE) de Page Builder, con la corrección inicial basada en Adobe Commerce 2.3.3. Hemos actualizado estos parches con una implementación más estable basada en Adobe Commerce 2.3.4., que incluye varias optimizaciones para solucionar el problema.<!--MAGECLOUD-4884-->

  Si tiene el paquete magento/magento-cloud-patch 1.0.0, aún estará protegido de los problemas de vulnerabilidad RCE de Page Builder. Si actualiza a 1.0.1 o posterior, tendrá una mejor implementación de la misma corrección.

## Versión 1.0.0

Fecha de la versión: 14 de noviembre de 2019

Esta es la primera versión del paquete [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches), que es una nueva dependencia para la versión 2002.0.22 del paquete `ece-tools` o versiones posteriores.

Esta versión de incluye las siguientes revisiones y correcciones críticas:

- **Revisiones de seguridad de Page Builder para las versiones 2.3.1.x y 2.3.2.x**: corrige un problema en la vista previa de Page Builder que permite a los usuarios no autenticados acceder a algunos métodos de creación de plantillas que se pueden usar para almacenar en déclencheur la ejecución de código arbitrario a través de la red (RCE), lo que provoca fugas de información global. Este problema puede producirse al utilizar versiones no compatibles de Page Builder con Adobe Commerce 2.3.1 y 2.3.2.<!--MAGECLOUD-4649-->

- **parches MSI**: corrige los problemas que causaban errores de indización y problemas de rendimiento al usar la configuración de inventario predeterminada para administrar existencias.<!--MAGECLOUD-4428-->

- **Compatibilidad con versiones anteriores de las nuevas interfaces de correo**: corrige un problema de incompatibilidad con versiones anteriores causado por la interfaz PHP `Magento\Framework\Mail\EmailMessageInterface` introducida en Adobe Commerce v2.3.3. En el ámbito de esta revisión, el nuevo `EmailMessageInterface` hereda del antiguo `MessageInterface`, y los módulos principales de Adobe Commerce se revierten para depender de `MessageInterface`.<!--MAGECLOUD-4422-->

- **La paginación del catálogo no funciona en Elasticsearch 6.x**: corrige un problema crítico con la paginación de resultados de búsqueda que afecta a los clientes que usan Elasticsearch 6.x como motor de búsqueda del catálogo.<!--MAGECLOUD-4448-->
