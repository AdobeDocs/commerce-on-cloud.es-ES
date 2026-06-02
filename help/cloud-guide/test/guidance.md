---
title: Guía de pruebas
description: Obtenga información sobre los tipos de pruebas y las prácticas recomendadas para iniciar Adobe Commerce en la infraestructura en la nube.
exl-id: 70fdfbbd-1763-4b1b-9ffd-9ffdc92f4f91
TQID: https://experienceleague.adobe.com/uCBGY5eP-R4zbUWeD-WE95F49k-U-nI058FR-rOJfsU
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 371
ht-degree: 0%

---

# Guía de pruebas

Después de configurar y personalizar el proyecto de Adobe Commerce en la nube, se recomienda probar la aplicación a fondo antes de iniciar el sitio web de la tienda. Las pruebas ayudan a administrar correctamente las expectativas del tamaño del clúster y a escalar adecuadamente para las necesidades futuras de la empresa.

## Pruebas funcionales

Durante el desarrollo, es importante realizar pruebas funcionales completas en su proyecto de Adobe Commerce en la nube. Consulte las siguientes directrices para realizar pruebas funcionales en el entorno Docker:

- **Pruebas de aplicaciones**: use [Magento Functional Testing Framework (MFTF)](https://developer.adobe.com/commerce/cloud-tools/docker/test/application-testing) para las pruebas de aplicaciones en el entorno Cloud Docker.

- **Pruebas de código**: use el marco de pruebas de decepción [para PHP](https://developer.adobe.com/commerce/cloud-tools/docker/test/code-testing) para validar el código destinado a la contribución a los repositorios de paquetes en la nube.

## Prácticas recomendadas antes del lanzamiento

Considere los siguientes tipos de pruebas como una práctica recomendada para realizar antes del lanzamiento del sitio:

- **Prueba de carga**: realice una prueba de carga para comprender el comportamiento del sistema en una carga esperada. Por ejemplo, pruebe un número simultáneo de usuarios activos en la aplicación, haciendo que cada usuario realice un número específico de transacciones dentro de la duración establecida. Esta prueba revela el tiempo de respuesta de transacciones importantes críticas para el negocio, como el comportamiento de la base de datos o del servidor de aplicaciones. Una prueba de carga puede ayudar a identificar cuellos de botella.

- **Prueba de esfuerzo**: desafíe los límites superiores de capacidad dentro del sistema para determinar si el sistema funciona lo suficiente cuando la carga actual supera con creces el máximo esperado.

- **Examen de seguridad**: Adobe proporciona una [Herramienta de examen de seguridad](../launch/overview.md#set-up-the-security-scan-tool) gratuita para sus sitios.

- **Prueba de penetración**: es un ataque cibernético simulado autorizado en un sistema informático diseñado para evaluar la seguridad del sistema. La prueba de penetración ayuda a identificar debilidades o vulnerabilidades, incluida la posibilidad de que partes no autorizadas obtengan acceso a las funciones y los datos del sistema.

>[!WARNING]
>
>No se permite a los clientes realizar ninguna evaluación de seguridad de la infraestructura de AWS ni de los propios servicios de AWS. Si encuentra un problema de seguridad en cualquiera de los servicios de AWS observados en su evaluación de seguridad, [póngase en contacto con el equipo de seguridad de AWS](mailto:aws-security@amazon.com) inmediatamente. Ver [directivas de cliente de AWS para pruebas de penetración](https://aws.amazon.com/security/penetration-testing/).
