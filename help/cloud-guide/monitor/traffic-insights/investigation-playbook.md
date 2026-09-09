---
title: Guía de investigación
description: Aprenda a investigar el uso excesivo del ancho de banda de la CDN, la carga de bots de búsqueda y rastreadores, y el tráfico malicioso mediante Adobe Commerce Traffic Insights, además de cuándo escalar.
feature: Cloud, Observability
role: Admin
source-git-commit: 09318645dd341a74d72237f4d4162d1d26d03651
workflow-type: tm+mt
source-wordcount: '1774'
ht-degree: 0%

---

# Guía de investigación

La aplicación [!DNL Adobe Commerce Traffic Insights] está diseñada para ayudarle a investigar los siguientes problemas:

- Ampliación de ancho de banda
- carga de rastreador
- Tráfico malintencionado

Como alternativa, puede solicitar [Seguridad avanzada: administración de bots nativa, DDoS de nivel 7 y limitación de velocidad](#advanced-security-native-bot-management-layer-7-ddos--rate-limiting), ruta de escalación nativa de Adobe cuando la mitigación manual no es suficiente. Cada paso hace referencia al widget que muestra el síntoma, para que pueda pasar de una métrica a una acción concreta.

>[!WARNING]
>
>Las sugerencias de esta página son solo directrices. Valide siempre cualquier regla de bloqueo con respecto a su propio tráfico antes de implementarla.

## Ampliación de ancho de banda CDN

Antes de considerar los excesos de ancho de banda, comprenda cómo se factura el ancho de banda. El tráfico de **todos** los servicios de Fastly empaquetados con la cuenta de [!DNL Adobe Commerce on Cloud Infrastructure], incluidos todos los entornos de ensayo de **y** de producción, se contabiliza en el uso común en comparación con la asignación anual en su contrato. Empiece desde **Ancho de banda > Ancho de banda total** y, a continuación, atribuya el volumen con **Ancho de banda por tipo de contenido** y **Ancho de banda por detalles de dominio**.

### Contenido multimedia

Algunas tiendas proporcionan legítimamente una gran parte del ancho de banda como medios debido a su catálogo. Si **Ancho de banda por tipo de contenido** muestra una cantidad significativa de ancho de banda de medios, considere las siguientes mitigaciones:

- Experimente con [Conversión con grandes pérdidas](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/cdn/fastly-image-optimization#force-lossy-conversion) para ofrecer imágenes más pequeñas y de menor calidad.
- Investigue [Optimización de imágenes extremadamente profunda](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/cdn/fastly-image-optimization#deep-image-optimization) para generar imágenes con el tamaño cambiado en el lado de la red de distribución de contenido (CDN).

### Archivos grandes

Algunos sitios contienen archivos de gran tamaño o respuestas específicas y pesadas, por ejemplo, integraciones o exportaciones de Enterprise Resource Planning (ERP). Use **URL por ancho de banda** para revisar las columnas **BW** y **Tamaño promedio** y encontrar estos archivos grandes. Puede usar **Ruta de acceso al nivel 1 por ancho de banda** para obtener una vista de nivel superior.

### Heavy 404

No se encontró una página de Adobe Commerce **404** que suele ser una página con un estilo de tema pesado (~1,5 MB) y **no almacenable en caché**, por lo que los 404 repetidos pueden generar tráfico anómalo. Incluso un recurso trivial que falta como `favicon.ico` puede convertirse en una pesada página de `404` en lugar de un archivo pequeño. Use las columnas **404** y **404 BW** en **Ancho de banda por detalles de dominio**, **URL por ancho de banda**, **IP principales por ancho de banda** y **Estadísticas por subredes IP** para encontrar clientes, IP y direcciones URL que generen volumen 404 de forma coherente. A continuación, reduzca o limite ese acceso, por ejemplo, y devuelva un elemento ligero `403`.

### Proporción de visitas de FPC baja

[!DNL Adobe] recomienda habilitar Fastly [blindaje](https://www.fastly.com/documentation/guides/concepts/shielding/) para que un agregador de caché de CDN principal sirva al origen, lo que permite que menos solicitudes lleguen a él desde puntos de presencia locales ([POP](https://www.fastly.com/documentation/guides/getting-started/concepts/using-fastlys-global-pop-network/)) más cercanos al cliente. Ver [comprobando tu configuración](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/cdn/setup-fastly/fastly-custom-cache-configuration#configure-back-ends-and-origin-shielding).

El tráfico POP a cliente y de blindaje a POP se cuentan por separado y, mientras la respuesta del cliente está comprimida, el tráfico de blindaje a POP [no está comprimido](https://www.fastly.com/documentation/guides/concepts/compression/#compression-at-the-edge) para conservar la compatibilidad con Edge Side Includes ([ESI](https://www.fastly.com/documentation/reference/vcl/statements/esi/)). Esto significa que una proporción de visitas de caché de página completa (FPC) baja proporciona un ancho de banda mucho mayor en las páginas dinámicas. Confirme el síntoma con **proporción de visitas de FPC**, **estadísticas de FPC por dominio** y **ancho de banda del segmento de red CDN**.

Una tasa de visitas baja suele estar impulsada por un gran volumen de rastreadores de motores de búsqueda (véase [Buscar bots y rastreadores](#search-bots-and-crawlers)). Otra mitigación es [ofrecer una memoria caché obsoleta a los rastreadores](https://www.fastly.com/documentation/reference/vcl/variables/cache-object/stale-exists/) cuando esté disponible. Si la causa son invalidaciones de caché amplias y frecuentes, use **Invalidación de caché por etiquetas** y **Edad de FPC por direcciones URL principales** para encontrar las etiquetas/direcciones URL perdidas.

## Buscar bots y rastreadores

Para medir el impacto del rastreador, comienza en **Bots conocidos por ancho de banda** y **Detalles del impacto de bots conocidos** para ver qué bots son los más activos y luego [filtra](https://docs.newrelic.com/docs/query-your-data/explore-query-data/dashboards/filter-new-relic-one-dashboards-facets/#example-use) por un bot específico para estudiar solo sus solicitudes.

### Demasiadas solicitudes

La causa más común de que un bot de búsqueda envíe demasiadas solicitudes se produce al analizar páginas que contienen `<meta name="robots" content="index,follow">`. Los bots pueden seguir los vínculos de navegación por capas y de navegación superior en un bucle casi interminable. Tenga en cuenta las siguientes opciones para solucionar este problema:

>[!WARNING]
>
> Consulte con un experto en optimización de los motores de búsqueda (SEO) antes de restringir la actividad de rastreador. El reciclaje puede afectar negativamente a tu SEO.

- Agregue `nofollow` a los vínculos de navegación por niveles y de navegación superior, por ejemplo `<a rel="nofollow" href="https://example.com/sales.html">Sales</a>`.
- Cambie la metaetiqueta de la página a `index,nofollow`, ya sea como una [configuración de diseño](https://experienceleague.adobe.com/es/docs/commerce-admin/marketing/seo/seo-overview#configure-robotstxt) común o por tipo de página con extensiones personalizadas. Mantenga `sitemap.xml` con precisión para que los bots siempre tengan una lista actualizada de páginas para indexar.
- Actualice `robots.txt` para bloquear rutas y recursos a los que los bots no deben acceder.
- Tenga en cuenta que la directiva `crawl-delay` no forma parte del Protocolo de exclusión de robots oficial, pero sí funciona para algunos bots, como Bingbot, Slurp, SEMrushBot y algunos otros. Googlebot ignora esta directiva.
- Agregar reglas de límite de tasa. Hay [protección contra rastreadores abusivos](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/RATE-LIMITING.md#abusive-crawler-protection) nativos en el módulo Fastly. Para un control más preciso, un [fragmento de lenguaje de configuración de barniz (VCL) personalizado](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-custom-snippets) puede devolver `429` (Demasiadas solicitudes) o `405` (Método no permitido) para una regex de agente de usuario con un límite de tasa individual. Consulte en la documentación del rastreador el método preferido y el código de respuesta. Consulte la [guía de VCL de limitación de velocidad de Fastly](https://www.fastly.com/documentation/reference/vcl/functions/rate-limiting/ratelimit-check-rate/).
- La IA y los rastreadores del modelo de lenguaje grande (LLM) son un caso especial cada vez más frecuente. No siempre se identifican de forma coherente, por lo que las reglas de usuario-agente de VCL pueden ir por detrás. El complemento [Advanced Security](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/cdn/advanced-security) de Adobe tiene [administración de bots nativa](#advanced-security-native-bot-management-layer-7-ddos--rate-limiting) que puede distinguir los rastreadores de IA y los buscadores verificados de los sospechosos en el perímetro, algo que VCL por sí solo no puede.

### Bloqueo de rastreadores no deseados

Si ciertos motores de búsqueda generan tráfico significativo y no son importantes para la empresa, pueden bloquearse por completo:

- Algunos bots siguen `robots.txt` cambios de 1 a 2 días después, después de volver a leer y actualizar sus reglas de análisis.
- Si un rastreador omite `robots.txt`, bloquéelo con un fragmento de VCL personalizado ([ejemplo](https://experienceleague.adobe.com/es/docs/commerce-knowledge-base/kb/how-to/block-malicious-traffic-for-magento-commerce-on-fastly-level#block-traffic-by-user-agent)). Algunos rastreadores lo documentan explícitamente como el método preferido o único de control de frecuencia.

## Scripts y rascadores malintencionados

Utilice la aplicación de Traffic Insights para identificar las direcciones comunes de ataque y filtrar por áreas de enfoque según sea necesario. Si las solicitudes marcadas en rojo provienen principalmente de ciertas IP, subredes o geolocalizaciones (**IP principales por recuento de solicitudes**, **estadísticas por subredes IP**, **estadísticas por país**), considere la posibilidad de bloquearlas con VCL personalizado de Fastly.

Cada proyecto de infraestructura en la nube ya tiene una línea de base de protección automática independientemente de la configuración que realice. El cortafuegos de aplicaciones web (WAF) incluido bloquea inmediatamente la inyección de SQL y las señales IP malintencionadas conocidas (puerta trasera, herramientas de ataque, CMDEXE, Log4J-JNDI, travversal, XSS) y limita la velocidad de otras IP no malintencionadas una vez que se cruzan 50 solicitudes/minuto, 350 solicitudes/10 minutos o 1.800 solicitudes/hora. Esa línea de base es lo que indican **Solicitudes por respuesta de WAF** y las columnas de señales de WAF en las tablas de esta aplicación. Un pico en estas columnas no significa necesariamente que no esté protegido.

- Esté atento al relleno de credenciales, la adquisición de cuentas, la creación de cuentas falsas, las pruebas de tarjetas, el raspado de contenido y el acaparamiento de inventarios/carros de compras. Estos patrones de abuso impulsados por bots aparecen en la ficha **Análisis de la actividad de bots y las solicitudes**. El tráfico de gran volumen y baja diversidad que llega a los extremos de inicio de sesión, cuenta, cierre de compra o catálogo es la firma que se busca en **IP principales por recuento de solicitudes** y **Detalles de impacto de bots conocidos**.
- Proteja los extremos de las API de cierre de compra y cierre de compra de los ataques de bots con [Google reCAPTCHA](https://experienceleague.adobe.com/es/docs/commerce-admin/systems/security/captcha/security-google-recaptcha).
- Utilice el límite de velocidad nativo del módulo Fastly [protección de ruta](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/RATE-LIMITING.md#path-protection).
- Compruebe [señales WAF de próxima generación](https://www.fastly.com/documentation/guides/next-gen-waf/signals/using-system-signals/) en el campo `Sigsci_Tags` separado por comas y combine coincidencias de señales relevantes en una regla de bloqueo dirigida. El valor de una solicitud sospechosa puede ser `BOT-ANALYSIS,DATACENTER,SIGSCI-IP,SITE-FLAGGED-IP,SUSPECTED-BAD-BOT`. WAF etiqueta una dirección IP con `SITE-FLAGGED-IP` hasta un umbral antes de que comience a bloquearse automáticamente. Los widgets **Señales de anomalías y ataques de WAF**, **Señales de bots de WAF** y **Solicitudes de respuesta de WAF**, así como las columnas de WAF de las tablas de IP, subred y país, los muestran.
- Consulte el artículo de Adobe sobre [bloqueo del tráfico malintencionado para Adobe Commerce en el nivel Fastly](https://experienceleague.adobe.com/es/docs/commerce-knowledge-base/kb/how-to/block-malicious-traffic-for-magento-commerce-on-fastly-level) para conocer los enfoques comunes.
- En el caso de escenarios complejos en los que el bloqueo manual no es una opción viable, como campañas de bots sostenidas, ataques propagados a través de muchas IP/API o denegación de servicio distribuida (DDoS) de nivel 7, considere primero el complemento [Seguridad avanzada](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/cdn/advanced-security) de Adobe (consulte [administración de bots nativos](#advanced-security-native-bot-management-layer-7-ddos--rate-limiting)). Funciona en el mismo Fastly edge que sirve a tu tienda. Si necesita capacidades fuera de su ámbito, la alternativa sugerida es un servicio de mitigación de bots administrado de terceros con integración nativa de Fastly, como [Datadome](https://docs.datadome.co/docs/module-fastly) o [HUMAN Bot Defender](https://www.fastly.com/documentation/guides/integrations/non-fastly-services/human-bot-defender/) (anteriormente PerimeterX). Todas estas opciones añaden costes adicionales.

## Seguridad avanzada: administración de bots nativa, DDoS de nivel 7 y limitación de velocidad

Las secciones anteriores analizan qué se puede hacer con los datos de la aplicación Traffic Insights y el manual Fastly VCL. En escenarios donde no es suficiente, como campañas de bots sostenidas o en evolución, DDoS de nivel 7 (capa de aplicación) o abusos propagados por muy pocas IP y extremos de API, Adobe ofrece [seguridad avanzada](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/cdn/advanced-security).

La seguridad avanzada es un complemento de pago para [!DNL Adobe Commerce on Cloud Infrastructure] que agrega administración de bots perimetrales (incluida la detección de rastreador de IA y de buscadores), protección DDoS de nivel 7 y limitación avanzada de velocidad en la misma plataforma de Fastly que ya sirve a la tienda. Consulte [Seguridad avanzada](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/cdn/advanced-security) para conocer todas las funcionalidades, las limitaciones actuales y cómo solicitarlas.

Una vez adquirida y habilitada, utilice la aplicación de perspectivas de tráfico para comprobar que la seguridad avanzada funciona. Sus decisiones se notifican a través de los mismos campos `Sigsci_Tags` y `Agent_response` detrás de **Señales de anomalías y ataques de WAF**, **Señales de bots de WAF** y **Solicitudes de respuesta de WAF**. Compare esos widgets antes y después de permitirlo para confirmar que está actuando de forma activa en el tráfico.
