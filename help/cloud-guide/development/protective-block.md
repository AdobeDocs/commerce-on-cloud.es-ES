---
title: Bloque protector
description: Obtenga información acerca de la función de bloque protector de Adobe Commerce en la infraestructura en la nube y cómo funciona para proteger su sitio contra vulnerabilidades de seguridad conocidas.
feature: Cloud, Configuration, Security
topic: Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '309'
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

1. **Bloqueo parcial** para sitios web de producción, lo que permite que el sitio permanezca en línea principalmente. Según la naturaleza de la vulnerabilidad, partes de una solicitud, como una cadena de consulta, cookies o cualquier encabezado adicional, podrían eliminarse de las solicitudes de GET. Todas las demás solicitudes pueden bloquearse por completo, como el inicio de sesión, el envío de formularios o el cierre de compra de productos.

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
