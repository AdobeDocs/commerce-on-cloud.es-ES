---
title: Pasos del inicio
description: Obtenga información sobre cómo completar el lanzamiento del sitio.
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '344'
ht-degree: 0%

---

# Pasos del inicio

Después de probar y completar la lista de comprobación de inicio, puede iniciar los pasos finales para iniciar. Estos pasos incluyen introducir tickets, transferir el acceso al sitio y, finalmente, probar la tienda cuando esté activa.

El personal de soporte del Adobe trabajará con usted durante todo el proceso, comprobando su estado y ayudándole en caso de que surja alguna pregunta o problema. Todos los problemas deben seguirse con tickets para capturar mejor lo que ha sucedido y cómo se ha resuelto. Cuando se inician iteraciones continuas en la implementación de actualizaciones en la tienda iniciada, es posible que se vuelvan a producir problemas similares. Estos tickets pueden ayudar a identificar los problemas y a ajustar las tareas de implementación.

## Póngase en contacto con el Adobe para iniciar el sitio

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

El Adobe comprueba y supervisa el sitio para asegurarse de que todos los servicios y accesos están en verde. Permanecemos a mano según sea necesario para avanzar y comprobar que todos los registros del sistema, servicios, almacenamiento en caché y funciones funcionan según lo que usted y sus clientes necesiten.

Si se produce algún problema, cree y realice un seguimiento de los mismos con Asistencia. Incluya la mayor cantidad de información posible, incluida la fecha/hora, la característica específica con un problema, los síntomas y los comportamientos impares, las extensiones, etc. Investigamos los registros, el problema y trabajamos con usted para resolverlo lo antes posible.
