---
title: Bloque protector
description: Obtenga información acerca de la función de bloque protector de Adobe Commerce en la infraestructura en la nube y cómo funciona para proteger su sitio contra vulnerabilidades de seguridad conocidas.
feature: Cloud, Configuration, Security
topic: Security
exl-id: 4a470e75-0b42-4ab7-b3dc-9f50b63bea14
TQID: 'https://experienceleague.adobe.com/E0lyCu6cFEaHR0KoauRFmQi9QThzoGDmNetnzYv3hVg'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
    internal-label: Security
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: 5a951749-fac9-5bc7-9a98-ebe4ff066437
    internal-label: Cloud
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
topic_v2:
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
    internal-label: Security
source-git-commit: e6e0bd8e116b2f0b93557b6aeb2aac7cbb8e1d8a
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 0%
---
# Bloque protector

Adobe Commerce en la infraestructura en la nube tiene una función de bloqueo protector que, en determinadas circunstancias, restringe el acceso a los sitios web con vulnerabilidades de seguridad. Este método de bloqueo parcial evita la explotación de vulnerabilidades de seguridad conocidas. El software obsoleto a menudo contiene vulnerabilidades, por lo que es importante protegerse contra estas vulnerabilidades bloqueando parcialmente el acceso a estos sitios.

## Cómo funciona el bloque protector

Adobe Commerce mantiene una base de datos de firmas de vulnerabilidades de seguridad conocidas en software de código abierto que normalmente se implementan en la infraestructura en la nube. La comprobación de seguridad sólo analiza las vulnerabilidades conocidas en los proyectos de código abierto; no puede examinar las personalizaciones que escriba.

Se ejecuta el análisis de seguridad:

- Al insertar código nuevo en Git
- Cuando se añaden nuevas vulnerabilidades a la base de datos

Si se detecta una vulnerabilidad crítica en la aplicación, se rechaza la notificación push de Git.

Existen dos tipos de bloques que se ejecutan:

1. **Bloque completo** para sitios web de desarrollo. El mensaje de error que acompaña a `git push` proporciona información detallada sobre la vulnerabilidad.

1. **Bloqueo parcial** para sitios web de producción, lo que permite que el sitio permanezca en línea principalmente. Según la naturaleza de la vulnerabilidad, partes de una solicitud, como una cadena de consulta, cookies o cualquier encabezado adicional, podrían eliminarse de las solicitudes GET. Todas las demás solicitudes pueden bloquearse por completo, como el inicio de sesión, el envío de formularios o el cierre de compra de productos.

El desbloqueo se automatiza al resolver el riesgo de seguridad. El bloque se elimina poco después de aplicar una actualización de seguridad que elimina la vulnerabilidad.

## Excluirse del bloque protector

El bloque protector está ahí para protegerle contra vulnerabilidades conocidas en el software que implementa Adobe Commerce en la infraestructura en la nube.

Sin embargo, puede excluirse del bloque protector agregando lo siguiente a [`.magento.app.yaml`](../application/configure-app-yaml.md):

```yaml
   preflight:
      enabled: false
```

Puede excluirse explícitamente de una comprobación específica, por ejemplo:

```yaml
   preflight:
      enabled: true
      ignore_rules: [ "drupal:SA-CORE-2014-005" ]
```
