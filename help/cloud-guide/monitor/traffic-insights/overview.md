---
title: Adobe Commerce Traffic Insights
description: Obtenga información acerca de la herramienta Adobe Commerce Traffic Insights y cómo puede ayudarle a comprender el tráfico de su proyecto de Adobe Commerce en la nube.
feature: Cloud, Observability
role: Admin
source-git-commit: 119c9415abd22221e3ae785445d537f0609eba14
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 0%

---

# Traffic Insights

Adobe Commerce Traffic Insights es una aplicación de New Relic One que visualiza [!DNL Adobe Commerce on Cloud Infrastructure] el tráfico de CDN de forma rápida. Lee las líneas de registro de acceso de Fastly CDN que ya se envían a New Relic como eventos `Log` y procesa un conjunto depurado de gráficos, con alcance a una cuenta de New Relic que seleccione y el intervalo de tiempo de la plataforma. Esto visualiza el tráfico Edge de una tienda sin escribir NRQL, el lenguaje de consulta de New Relic, a mano.

## Qué le ayuda a investigar

Traffic Insights está diseñado para ayudarle a resolver tres problemas comunes:

- **Ampliación de ancho de banda de CDN**: tendencias de tráfico por encima de la asignación de contrato. Atribuya el volumen a medios pesados, archivos grandes, páginas 404 no almacenables en caché o a una caché ineficiente, a un dominio, tipo de contenido, dirección URL o proyecto específicos.
- **Carga de rastreador y bot de búsqueda**: un motor de búsqueda o rastreador de IA que genera una proporción desproporcionada de solicitudes, lo que afecta a la eficacia de la caché y a la carga de origen. Vea qué bots con nombre son los más activos y exactamente qué recuperan.
- **Scripts y rascadores maliciosos**: Raspado, relleno de credenciales, pruebas de tarjetas, creación de cuentas falsas o abuso de nivel 7. Aparecen las señales de Fastly Next-Gen WAF y las IP, subredes y países detrás del tráfico sospechoso.

En cada caso, la aplicación identifica el *quién, qué y dónde* del tráfico. Actuando en base a esa información a través de las reglas VCL de Fastly, la optimización de imágenes, el ajuste de caché, la limitación de velocidad o el complemento [Advanced Security](../../cdn/advanced-security.md) de Adobe en su configuración de Commerce y Fastly. El manual de [investigación](investigation-playbook.md) cubre cada uno de estos problemas.

## Acceso a la aplicación

- **Vínculo directo:** [Perspectivas de tráfico de Adobe Commerce](https://one.newrelic.com/a9a0c3b8-3844-4ca1-8bad-c6742747be47).
- **Desde la pantalla de inicio de New Relic One** (one.newrelic.com) — una vez que la cuenta está suscrita a la aplicación, aparece como su propio mosaico, **Adobe Commerce Traffic Insights** en la página de inicio.
- **En la barra de búsqueda superior (Búsqueda rápida)**, busque `Adobe Commerce Traffic Insights` y selecciónelo en los resultados.
- **Para anclarlo para un acceso más rápido** - usa el control de estrella o pin en el mosaico de la aplicación o en el encabezado de página para agregarlo a Favoritos o a la navegación izquierda. La ubicación exacta de este control depende de la versión de la interfaz de usuario de New Relic que se use para la cuenta.

## En esta guía

- **[Explicación de la aplicación](understanding-the-app.md)**: qué es Traffic Insights, cómo manejarla con filtros, cómo se miden los números y qué pueden y no pueden decirle los datos.
- **[Libro de estrategias de investigación](investigation-playbook.md)**: enfoques recomendados para los tres problemas que la aplicación está diseñada para solucionar: uso excesivo del ancho de banda, carga de rastreadores y tráfico malintencionado. Cada uno de ellos hace referencia al gráfico que lo confirma y especifica la ruta de escalación [Advanced Security](../../cdn/advanced-security.md) nativa de Adobe para los casos en que la mitigación manual no sea suficiente.