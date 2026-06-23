---
title: Optimizar implementación en la nube
description: Obtenga información acerca de las formas de optimizar el proceso de implementación de Adobe Commerce en proyectos de infraestructura en la nube, incluida la reducción del tiempo de inactividad, la implementación de contenido estático, la implementación basada en escenarios y los asistentes inteligentes.
feature: Cloud, Deploy, SCD
exl-id: 4315e2f4-06af-4a5c-9db9-e7b2f63660df
TQID: https://experienceleague.adobe.com/bd9n9CFrpyn1UZG6SX8qkoZGBOFd2N7z9Hoa1hQ8rew
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
source-git-commit: d863fc70609dcc66d21eb95e709db80e29114714
workflow-type: tm+mt
source-wordcount: 230
ht-degree: 0%

---

# Optimizar implementación

El rendimiento del sitio puede verse afectado durante el proceso de implementación. El tiempo que un sitio está en modo de mantenimiento al implementarlo en un sitio de producción depende de muchos factores, como la configuración del entorno y la cantidad de contenido que contenga un sitio. La primera práctica recomendada para optimizar su implementación de Cloud es [actualizar para usar `ece-tools`](../dev-tools/install-package.md) y beneficiarse de las características del paquete, como comandos para crear una copia de seguridad de la base de datos y verificar la configuración del entorno.

Los siguientes temas pueden ayudarle a comprender mejor cómo optimizar el proceso de implementación:

- [Proceso de implementación en la nube](process.md)
El proceso de implementación en la nube consta de tres fases y puede aprovechar los puntos fuertes y débiles de cada una de ellas.

- [Implementación sin tiempo de inactividad](reduce-downtime.md)
Comprenda qué sucede durante la implementación y cómo reducir el tiempo de inactividad que su tienda experimenta durante una actualización del entorno de producción.

- [Implementación de contenido estático](static-content.md)
La mejor manera de optimizar la implementación de Cloud es controlar cómo y cuándo generar contenido estático.

- [asistentes inteligentes](smart-wizards.md)
El paquete `ece-tools` proporciona los comandos del asistente inteligente para evaluar rápidamente la configuración del proyecto.

- [Seguimiento de implementaciones con New Relic](../monitor/track-deployments.md)
Utilice el servicio New Relic para monitorizar los eventos de implementación y analizar el impacto de la implementación en el rendimiento general.

