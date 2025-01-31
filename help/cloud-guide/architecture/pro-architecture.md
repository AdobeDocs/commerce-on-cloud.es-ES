---
title: Arquitectura profesional
description: Obtenga información acerca de los entornos admitidos por la arquitectura Pro.
feature: Cloud, Auto Scaling, Iaas, Paas, Storage
topic: Architecture
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '1554'
ht-degree: 0%

---

# Arquitectura profesional

La arquitectura Pro de Adobe Commerce en la infraestructura en la nube admite varios entornos que puede utilizar para desarrollar, probar e iniciar su tienda.

- **Master**: proporciona una rama `master` implementada en contenedores de Platform as a service (PaaS).
- **Integración**: proporciona una sola rama `integration` para desarrollo, aunque puede crear una rama adicional. Esto permite hasta dos ramas _activas_ implementadas en contenedores de Platform as a service (PaaS).
- **Ensayo**: proporciona una sola rama `staging` implementada en contenedores de Infraestructura como servicio (IaaS) dedicados.
- **Producción**: Proporciona una sola rama de `production` implementada en contenedores de Infraestructura como servicio (IaaS) dedicados.

La siguiente tabla resume las diferencias entre entornos:

|                                        | INTEGRACIÓN | ENSAYO | PRODUCCIÓN |
| -------------------------------------- | ----------- | ----------------- | -------------------- |
| Admite la administración de configuración en [!DNL Cloud Console] | Sí | Limitado | Limitado |
| Admite varias ramas | Sí | No (solo ensayo) | No (solo producción) |
| Utiliza archivos YAML para la configuración | Sí | No | No |
| Funciona en hardware IaaS dedicado | No | Sí | Sí |
| Incluye Fastly CDN | No | Sí | Sí |
| Incluye el servicio New Relic | No | APM | APM + NRI |
| Copias de seguridad automáticas | No | Sí | Sí |

>[!NOTE]
>
>Adobe proporciona la herramienta Cloud Docker para Commerce para su implementación en un entorno local de Cloud Docker, de modo que pueda desarrollar y probar proyectos de Adobe Commerce. Consulte [Desarrollo de Docker](../dev-tools/cloud-docker.md).

## Arquitectura del entorno

Su proyecto es un único repositorio Git con tres ramas de entorno principales: `integration`, `staging` y `production`. El diagrama siguiente muestra la relación jerárquica de los entornos Pro:

![Vista de alto nivel de la arquitectura del entorno Pro](../../assets/pro-branch-architecture.png)

### Entorno principal

En proyectos Pro, la rama `master` proporciona un entorno PaaS activo con su entorno de producción. Inserte siempre una copia del código de producción en el entorno `master` para poder depurar el entorno de producción sin interrumpir los servicios.

**Advertencias:**

- **no** crea una rama basada en la rama `master`. Utilice el entorno de integración para crear ramas activas para el desarrollo.

- No utilice el entorno `master` para pruebas de desarrollo, UAT o de rendimiento

### Entorno de integración

El entorno de integración se ejecuta en un contenedor Linux (LXC) en una cuadrícula de servidores conocidos como PaaS. Cada entorno incluye un servidor web y una base de datos para probar el sitio. Consulte [Direcciones IP regionales](../project/regional-ip-addresses.md) para obtener una lista de direcciones IP de AWS y Azure.

**Casos de uso recomendados:**

Los entornos de integración están diseñados para pruebas y desarrollo limitados antes de trasladar los cambios a los entornos de ensayo y producción. Por ejemplo, puede utilizar el entorno de integración para completar las siguientes tareas:

- Asegúrese de que los cambios en los procesos de integración continua (CI) sean compatibles con la nube

- Pruebe flujos de trabajo críticos en páginas clave como Inicio, Categoría, Página de detalles del producto (PDP), Cierre de compra y Administración.

Para obtener el mejor rendimiento en el entorno de integración, siga estas prácticas recomendadas:

- Restringir el tamaño del catálogo: como referencia, los datos de muestra contienen unos 2048 productos. Pruebe a reducir el tamaño del catálogo a alrededor de 4000-5000 productos.
Para comprobar el número de productos en el catálogo, ejecute la siguiente consulta MySQL:

  ```sql
  select distinct count(entity_id) from catalog_product_entity;
  ```

- Reducir el número de grupos de clientes: tener demasiados grupos de clientes puede afectar al rendimiento de indexación y al rendimiento general.

- Limitar el uso a uno o dos usuarios simultáneos

- Deshabilite los trabajos cron y ejecute manualmente según sea necesario

**Advertencias:**

- Los servicios de CDN y New Relic no son accesibles en un entorno de integración

- La arquitectura del entorno de integración no coincide con la arquitectura de ensayo y producción

- No utilice el entorno `integration` para pruebas de desarrollo, pruebas de rendimiento o pruebas de aceptación de usuarios (UAT)

- No utilice el entorno `integration` para probar la funcionalidad B2B de Adobe Commerce

- No puede restaurar la base de datos en el entorno de integración desde la producción o el ensayo de la base de datos

{{enhanced-integration-envs}}

### Entorno de ensayo

El entorno de ensayo proporciona un entorno casi de producción para probar el sitio. Este entorno, que está alojado en hardware IaaS dedicado, incluye todos los servicios, como Fastly CDN, New Relic APM y search.

**Casos de uso recomendados:**

El entorno coincide con la arquitectura de producción y está diseñado para UAT, ensayo de contenido y revisión final antes de transferir características al entorno `production`. Por ejemplo, puede utilizar el entorno `staging` para completar las tareas siguientes:

- Pruebas de regresión con datos de producción

- Pruebas de rendimiento con almacenamiento en caché de Fastly habilitado

- Prueba de nuevas compilaciones en lugar de aplicar parches en Producción

- Pruebas UAT para nuevas compilaciones

- Prueba B2B para Adobe Commerce

- Personalizar la configuración de cron y probar los trabajos de cron

Consulte [Flujo de trabajo de implementación](pro-develop-deploy-workflow.md#deployment-workflow) y [Implementación de prueba](../test/staging-and-production.md).

**Advertencias:**

- Después de iniciar el sitio de producción, utilice el entorno de ensayo principalmente para probar parches para correcciones de errores críticas para la producción.

- No puede crear una rama a partir de la rama `staging`. En su lugar, inserta cambios de código de la rama `integration` a la rama `staging`.

{{second-staging}}

### Entorno de producción

El entorno de producción ejecuta sus tiendas públicas de uno o varios sitios. Este entorno se ejecuta en hardware IaaS dedicado con nodos redundantes de alta disponibilidad para un acceso continuo y protección contra fallos para sus clientes. El entorno de producción incluye todos los servicios en el entorno de ensayo, además del servicio [Infraestructura de New Relic (NRI)](../monitor/new-relic-service.md#new-relic-infrastructure), que se conecta automáticamente con los datos de la aplicación y los análisis de rendimiento para proporcionar una supervisión dinámica del servidor.

**Advertencia:**

No puede crear una rama a partir de la rama `production`. En su lugar, inserta cambios de código de la rama `staging` a la rama `production`.

### Pila de tecnología de producción

El entorno de producción tiene tres máquinas virtuales (VM) detrás de un equilibrador de carga elástico administrado por un HAProxy por VM. Cada VM incluye las siguientes tecnologías:

- **Fastly CDN**: almacenamiento en caché HTTP y CDN

- **NGINX**: servidor web que usa PHP-FPM, una instancia con varios trabajadores

- **GlusterFS**: servidor de archivos para administrar todas las implementaciones y sincronización de archivos estáticos con cuatro montajes de directorios:

   - `var`
   - `pub/media`
   - `pub/static`
   - `app/etc`

- **Redis**: un servidor por VM con solo uno activo y los otros dos como réplicas

- **Elasticsearch**: busque Adobe Commerce en la infraestructura en la nube 2.2 a 2.4.3-p2

- **OpenSearch**: busque Adobe Commerce en la infraestructura en la nube 2.3.7-p3, 2.4.3-p2, 2.4.4 y posterior

- **Galera**: clúster de base de datos con una base de datos MariaDB MySQL por nodo con una configuración de incremento automático de tres para identificadores únicos en cada base de datos

La siguiente figura muestra las tecnologías utilizadas en el entorno de producción:

![Pila de tecnología de producción](../../assets/az-stack-diagram.png)

## Hardware redundante

En lugar de ejecutar una configuración tradicional, activa-pasiva `master` o primaria-secundaria, Adobe Commerce en la infraestructura de la nube ejecuta una _arquitectura redundante_ en la que las tres instancias aceptan lecturas y escrituras. Esta arquitectura ofrece cero tiempo de inactividad al escalar y proporciona integridad transaccional garantizada.

Gracias al hardware único y redundante, Adobe puede proporcionar tres servidores de puerta de enlace. La mayoría de los servicios externos le permiten agregar varias direcciones IP a una lista de permitidos, por lo que tener más de una dirección IP fija no es un problema. Las tres puertas de enlace se asignan a los tres servidores del clúster de entornos de producción y conservan las direcciones IP estáticas. Es totalmente redundante y tiene una alta disponibilidad en todos los niveles:

- DNS
- Red de entrega de contenido (CDN)
- Equilibrador de carga elástico (ELB)
- Clúster de tres servidores que incluye todos los servicios de Adobe Commerce, incluida la base de datos y el servidor web

## Backup y recuperación ante desastres

Adobe Commerce en la infraestructura en la nube utiliza una arquitectura de alta disponibilidad que replica cada proyecto de Pro en tres zonas de disponibilidad de AWS o Azure independientes, cada una con un centro de datos independiente. Además de esta redundancia, los entornos de ensayo y producción Pro reciben copias de seguridad en vivo regulares que están diseñadas para usarse en casos de _fallo catastrófico_.

**Las copias de seguridad automáticas** incluyen datos persistentes de todos los servicios en ejecución, como la base de datos MySQL y los archivos almacenados en los volúmenes montados. Las copias de seguridad se guardan en un Elastic Block Storage (EBS) cifrado en la misma región que el entorno de producción. Las copias de seguridad automáticas no son de acceso público porque se almacenan en un sistema independiente.

>[!NOTE]
>
>Los volúmenes montados solo incluyen los [montajes grabables](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/app/properties/properties#mounts), o hacen referencia a ellos, y no incluirán todo el directorio `app/`. En cuanto a los demás archivos, se crean o generan mediante el [proceso de compilación e implementación](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/architecture/pro-develop-deploy-workflow#deployment-workflow), y también tendrá que comprobar si quedan archivos en el repositorio Git.

{{pro-backups}}

Puede crear una **copia de seguridad manual** de la base de datos para los entornos de ensayo y producción mediante comandos CLI. Consulte [Hacer una copia de seguridad de la base de datos](../storage/database-dump.md). Para entornos de `integration`, Adobe recomienda crear una copia de seguridad como primer paso después de acceder a su proyecto de Adobe Commerce en la nube y antes de aplicar cualquier cambio importante. Consulte [Administración de copias de seguridad](../storage/snapshots.md).

### Objetivo de punto de recuperación

Póngase en contacto con el administrador de éxito del cliente de Adobe para obtener más información sobre el objetivo de punto de recuperación tiempo hasta la última copia de seguridad. La frecuencia de las copias de seguridad depende de la programación de copias de seguridad del plan y del volumen de cambios para escribir en el servicio de almacenamiento.

### Política de retención

El Adobe retiene las copias de seguridad automáticas según la siguiente política de retención de datos:

| Período de tiempo | Política de retención de copia de seguridad |
| ------------------ | ----------------------- |
| Días 1 a 3 | Una copia de seguridad por hora |
| Días 4 a 7 | Una copia de seguridad al día |
| Semanas 2 a 6 | Una copia de seguridad por semana |
| Semanas 8 a 12 | Una copia de seguridad quincenal |
| Meses 3 a 5 | Una copia de seguridad al mes |

Esta política puede variar según su plan de infraestructura en la nube.

### Objetivo de tiempo de recuperación

RTO depende del tamaño del almacenamiento. Los volúmenes grandes de EBS tardan más tiempo en restaurarse. Los tiempos de restauración pueden variar según el tamaño de la base de datos. Póngase en contacto con el administrador de éxito del cliente de Adobe para obtener más información.

## Escalado de clúster Pro

El tamaño del clúster Pro y las configuraciones de _compute_ varían según el proveedor de nube elegido (AWS, Azure), la región y las dependencias del servicio. Las infraestructuras de nube de Adobe pueden escalar los clústeres Pro para adaptarse a las expectativas de tráfico y los requisitos de servicio a medida que cambian las demandas.

La arquitectura redundante permite que la infraestructura de nube de Adobe se amplíe sin tiempo de inactividad. Al ampliar, cada una de las tres instancias gira para actualizar la capacidad sin afectar al funcionamiento del sitio. Por ejemplo, puede agregar servidores web adicionales a un clúster existente si la constricción se encuentra en el nivel PHP en lugar de en el nivel de base de datos. Esto proporciona _escalado horizontal_ para complementar el escalado vertical proporcionado por las CPU adicionales en el nivel de base de datos. Consulte [Arquitectura a escala](scaled-architecture.md).

Si espera un aumento significativo del tráfico por un evento u otro motivo, puede solicitar un aumento temporal de la capacidad. Consulte [Cómo solicitar un cambio de tamaño temporal](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/how-to-request-temporary-magento-upsize.html) en el _Centro de ayuda de Commerce_.
