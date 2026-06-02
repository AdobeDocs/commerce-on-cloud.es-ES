---
title: Pila de tecnología
description: Consulte la pila de tecnología que forma la infraestructura de Commerce en la nube.
feature: Cloud, Iaas, Paas
exl-id: 3fac1ab7-6440-4bf9-8169-9fadf51d70dd
TQID: https://experienceleague.adobe.com/2-uZdx1Oi-3LQUK-L7rC4kZWYcYobEhnVcUDNwOHpFs
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: b5f00040-57a0-4a6d-a39e-383b1936c2c9
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: c1256247-af4b-46d8-9dca-0c654ecfa157
  - id: d1e21356-0064-4f48-9089-16e3f0dbd2a6
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
subfeature_v2:
  - id: df5e974b-6742-4873-a687-a6bedaafdaa2
  - id: f2261633-201d-46c5-8a66-999e70527a83
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: efe0f95c16d9aab9af6a9c263034b08dce4d7c93
workflow-type: tm+mt
source-wordcount: 405
ht-degree: 0%

---

# Pila de tecnología

Considere Adobe Commerce en la infraestructura en la nube como cinco capas funcionales, como se muestra a continuación:

![Pila de nube](../../assets/CloudStack.png)

1. [**Infraestructura en la nube**](pro-architecture.md): Elija Amazon Web Service (AWS) o Microsoft Azure como base de Infrastructure as a Service (IaaS) para sus proyectos de Adobe Commerce en la nube Pro.

   Adobe analiza de forma rutinaria el uso de los recursos de computación virtual (vCPU) y asigna automáticamente recursos para optimizar el uso a largo plazo y mitigar el riesgo de superar la asignación máxima anual de días de vCPU. Si espera un aumento del tráfico del sitio durante períodos de tiempo específicos, debe seguir abriendo un ticket de soporte técnico para [solicitar un aumento temporal](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/how-to-request-temporary-magento-upsize.html).

1. [**Platform as a Service**](cloud-architecture.md): cada proyecto de infraestructura en la nube de Adobe Commerce proporciona un entorno de integración de Platform as a Service (PaaS) para desarrollar, probar e integrar servicios.
1. [**Adobe Commerce**](../project/overview.md): Adobe Commerce en la infraestructura en la nube proporciona una infraestructura aprovisionada previamente que incluye PHP, MySQL (MariaDB), Redis, servicios de cola de mensajes ([!DNL RabbitMQ] o [!DNL ActiveMQ]) y tecnologías de motores de búsqueda compatibles.
1. [**Herramientas de rendimiento**](../monitor/new-relic-service.md): Las herramientas de rendimiento de New Relic le permiten depurar, supervisar y administrar sus aplicaciones e infraestructura mediante la recopilación, el análisis y la visualización de datos de su Adobe Commerce en proyectos de infraestructura en la nube.
1. [**Red de distribución de contenido (CDN), firewall de aplicaciones web ([!DNL WAF]) y optimización de imágenes (IO)**](../cdn/fastly.md):

   * [Fastly CDN](../cdn/fastly.md#ddos-protection): proporciona servicios CDN seguros con protección integrada contra ataques de denegación de servicio (DDoS) distribuidos como [!DNL Ping of Death], [!DNL Smurf] y otros ataques de inundación basados en el Protocolo de mensajes de control de Internet (ICMP).
   * [Firewall de aplicaciones web (WAF)](../cdn/fastly-waf-service.md): los servicios de WAF garantizan la compatibilidad con PCI de las tiendas Adobe Commerce en entornos de producción y las directivas de WAF que protegen las aplicaciones web de Adobe Commerce de ataques de inyección, entradas malintencionadas, scripts entre sitios, exfiltración de datos, infracciones del protocolo HTTP y otras [[!DNL OWASP] diez amenazas de seguridad principales](https://owasp.org/www-project-top-ten/).
   * [Optimización de imágenes (IO)](../cdn/fastly-image-optimization.md): proporciona manipulación y optimización de imágenes en tiempo real para acelerar la entrega de imágenes y simplificar el mantenimiento de los conjuntos de fuentes de imágenes para aplicaciones web adaptables. Fastly IO descarga el procesamiento de imágenes y el cambio de tamaño de la carga, lo que libera a los servidores para procesar pedidos y conversiones de forma eficiente.

Las aplicaciones monolíticas consumen muchos recursos y son difíciles de escalar y servir rápidamente. Con la infraestructura en la nube, los clientes de Commerce obtienen un acceso inigualable a microservicios basados en SaaS que son enriquecidos, inteligentes y con rendimiento. Consulte [Software y servicios compatibles](cloud-architecture.md#supported-software-and-services).

Use la [guía de introducción a Commerce](../../get-started/overview.md) para configurar su nuevo programa de nube y comenzar a administrar su aplicación [!DNL Commerce] en un entorno nativo de la nube.
