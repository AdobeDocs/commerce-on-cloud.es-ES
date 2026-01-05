---
source-git-commit: 305df8acb8c41369b34e2c20d9c7df6824222931
workflow-type: tm+mt
source-wordcount: '559'
ht-degree: 1%

---
# Contribución

¡Gracias por contribuir!

A continuación se proporciona un conjunto de directrices que se deben seguir al contribuir en este proyecto.

## Código de conducta

Este proyecto se adhiere al [código de conducta](code-of-conduct.md) de Adobe. Al participar,
se espera que respete este código. Informe de cualquier comportamiento inaceptable a
[Grp-opensourceoffice@adobe.com](mailto:Grp-opensourceoffice@adobe.com).

## Documentación de la guía del colaborador

Consulte la [Guía del colaborador](https://experienceleague.adobe.com/docs/contributor/contributor-guide/introduction.html?lang=es).

## ¿Tiene alguna pregunta?

Comience por rellenar un problema. Los supervisores de este proyecto trabajan para llegar a
consenso en torno a la dirección del proyecto y las soluciones de problemas dentro de los hilos de problemas
(cuando proceda).

## Acuerdo de licencia de colaborador

Todas las contribuciones de terceros a este proyecto deben estar acompañadas por un colaborador firmado
acuerdo de licencia. Esto otorga permiso a Adobe para redistribuir sus contribuciones
como parte del proyecto. [Firme nuestro contrato de licencia de colaborador](https://opensource.adobe.com/cla.html). Usted
solo necesita enviar un contrato de licencia de colaborador de Adobe, por lo que si ha enviado uno anteriormente,
¡estás listo para irte!

## Revisiones de código

Todos los envíos deben ser solicitudes de extracción y deben revisarse
por los supervisores del proyecto. Leer [documentación de solicitud de extracción de GitHub](https://docs.github.com/articles/about-pull-requests/)
para obtener más información sobre el envío de solicitudes de extracción.

Por último, siga la [plantilla de solicitud de extracción](PULL_REQUEST_TEMPLATE.md) cuando
envío de una solicitud de extracción

## De colaborador a supervisor

¡Nos encantan las contribuciones de nuestra comunidad! Si desea ir un paso más allá de colaborador
y convertirse en un supervisor con acceso de escritura completo y voz en el proyecto, debe
ser invitado al proyecto. Los supervisores existentes emplean una nominación interna
proceso que debe alcanzar un consenso diferido (el silencio es aprobación) antes de las invitaciones
se emitan. Si cree que está cualificado y desea implicarse más profundamente,
no dude en ponerse en contacto con los supervisores existentes para hablar sobre ello.

## Problemas de seguridad

Los problemas de seguridad no deben notificarse en este rastreador de problemas. En su lugar, [presente un problema a nuestros expertos en seguridad](https://helpx.adobe.com/es/security/alertus.html)

## Aspectos destacados de las novedades

Si los cambios introducen nuevos temas, actualizaciones significativas o correcciones que es necesario resaltar, puede agregar una breve descripción a la [sección Novedades](https://experienceleague.adobe.com/es/docs/commerce-cloud-service/user-guide/overview#whats-new) directamente desde el cuerpo de la solicitud de extracción.

Para añadir un resaltado de Novedades:

1. Incluya la etiqueta `whatsnew` con la descripción adecuada en el cuerpo de la solicitud de extracción al final. La descripción debe proporcionar contexto sobre el cambio y un vínculo al tema o temas de destino. Utilice el siguiente formato (las comillas de bloque de código son solo para representación, no las incluya en el cuerpo de la solicitud de extracción):

   ```text
   whatsnew
   Short description of the change in the [target topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/target-topic.html).
   ```

   o, si hay varios temas:

   ```text
   whatsnew
   Short description of the changes in the [first target topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/target-topic.html), [second target topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/second-target-topic.html), and [third target topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/third-target-topic.html).
   ```

   también puede utilizar listas para varios elementos destacados:

   ```text
   whatsnew
   - Short description of the first change in the [first topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/first-topic.html).
   - Short description of the second change in the [second topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/second-topic.html).
   ```

   ```text
   whatsnew
   The following changes were made to the documentation:
   - Short description of the first change in the [first topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/first-topic.html).
   - Short description of the second change in the [second topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/second-topic.html).
   ```

1. Añada etiquetas compatibles que indiquen el tipo de cambio. Las etiquetas admitidas incluyen etiquetas para cada tipo de cambio, como:

   - `new-topic` - para nuevos temas
   - `major-update`: para actualizaciones importantes que pueden incluir cambios significativos en el contenido, la estructura o la funcionalidad
   - `technical` - para cambios técnicos que no se consideran actualizaciones importantes pero que aún requieren atención

**Importante:**

1. La parte `whatsnew` debe comenzar desde la etiqueta `whatsnew` y estar al final del cuerpo de la solicitud de extracción.
1. Las descripciones de los cambios deben incluir vínculos de trabajo. Asegúrese de que los vínculos sean correctos y lleven a los temas deseados. Si el tema es nuevo, compruebe que los vínculos funcionan después de combinar la solicitud de extracción y publicar el nuevo tema. Es aceptable corregir los vínculos después de combinar la solicitud de extracción.

Por ejemplo, busque en solicitudes de extracción cerradas en el repositorio para ver el formato de los resaltados existentes y compárelos con la [sección Novedades](https://experienceleague.adobe.com/es/docs/commerce-cloud-service/user-guide/overview#whats-new) para ver cómo aparecen en la documentación.
