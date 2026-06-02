---
title: Pasos del inicio
description: Obtenga información sobre cómo completar el lanzamiento del sitio.
exl-id: e7a3cd6b-32de-4fd0-9fbd-da8299e77114
TQID: https://experienceleague.adobe.com/Nl8YFJUZUkxtsm0eqBgJklsTysKuUE31PkEDsJmvBMU
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: c1256247-af4b-46d8-9dca-0c654ecfa157id: d1e21356-0064-4f48-9089-16e3f0dbd2a6id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 347
ht-degree: 0%

---

# Pasos del inicio

Después de probar y completar la lista de comprobación de inicio, puede iniciar los pasos finales para iniciar. Estos pasos incluyen introducir tickets, transferir el acceso al sitio y, finalmente, probar la tienda cuando esté activa.

El personal de soporte de Adobe colabora con usted a lo largo del proceso, comprobando el estado y ayudando en caso de que surjan preguntas o problemas. Todos los problemas deben seguirse con tickets para capturar mejor lo que ha sucedido y cómo se ha resuelto. Cuando se inician iteraciones continuas en la implementación de actualizaciones en la tienda iniciada, es posible que se vuelvan a producir problemas similares. Estos tickets pueden ayudar a identificar los problemas y a ajustar las tareas de implementación.

## Póngase en contacto con Adobe para iniciar el sitio

Póngase en contacto con el servicio de asistencia de Adobe Commerce y actualice los tickets de lanzamiento del sitio (lanzamiento) con la fecha y hora previstas para cambiar y lanzar la tienda.

## Cambiar DNS al nuevo sitio

El valor del cambio de tiempo de vida es importante para comprobar el dominio que ha cambiado. Cuando modifica los registros A y CNAME, la actualización tarda el TTL configurado en actualizarse correctamente. Para obtener más información sobre la configuración de DNS, consulte [Configuraciones de DNS](checklist.md#update-dns-configuration-with-production-settings).

### Para cambiar al nuevo sitio:

1. Acceda al servicio DNS.

1. Actualice los registros A y CNAME para sus dominios y nombres de host.

1. Espere a que transcurra el tiempo TTL y reinicie el explorador web.

1. Acceda a su tienda mediante el dominio de la tienda.

## Prueba de la tienda en directo

Complete algunas pruebas de UAT en su tienda en directo para confirmar que todo se está cargando y que las acciones se completan correctamente. Para obtener una lista de pruebas, consulte [Implementación de pruebas](../test/staging-and-production.md#complete-uat-testing).

## Posterior al lanzamiento

Adobe comprueba y supervisa el sitio para asegurarse de que todos los servicios y accesos están en verde. Permanecemos a mano según sea necesario para avanzar y comprobar que todos los registros del sistema, servicios, almacenamiento en caché y funciones funcionan según lo que usted y sus clientes necesiten.

Si se produce algún problema, cree y realice un seguimiento de los mismos con Asistencia. Incluya la mayor cantidad de información posible, incluida la fecha/hora, la característica específica con un problema, los síntomas y los comportamientos impares, las extensiones, etc. Investigamos los registros, el problema y trabajamos con usted para resolverlo lo antes posible.
