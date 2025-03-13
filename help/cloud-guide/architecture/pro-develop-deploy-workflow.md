---
title: Flujo de trabajo de proyecto profesional
description: Aprenda a utilizar los flujos de trabajo de desarrollo e implementación de Pro.
feature: Cloud, Iaas, Paas
exl-id: efe41991-8940-4d5c-a720-80369274bee3
source-git-commit: b4905acf71e4cb71eb369cb6d4bb3abe9ada4e9d
workflow-type: tm+mt
source-wordcount: '800'
ht-degree: 0%

---

# Flujo de trabajo de proyecto profesional

El proyecto Pro incluye un único repositorio Git con una rama global `master` y tres entornos principales:

1. **Entorno de producción** para iniciar y mantener el sitio activo
1. **Entorno de ensayo** para pruebas con todos los servicios
1. Entorno de **integración** para desarrollo y pruebas

![Lista de entornos profesionales](../../assets/pro-environments.png)

Estos entornos son `read-only`, y aceptan cambios de código implementado desde ramas insertadas desde el espacio de trabajo local. Consulte [Arquitectura Pro](pro-architecture.md) para obtener una descripción general completa de los entornos Pro. Consulte [[!DNL Cloud Console]](../project/overview.md#cloud-console) para obtener una descripción general de la lista Entornos profesionales en la vista de proyecto.

El siguiente gráfico muestra el flujo de trabajo de desarrollo e implementación de Pro, que utiliza un enfoque simple de ramificación de Git. Usted [desarrolla](#development-workflow) código utilizando una rama activa basada en el entorno `integration`, _insertando_ y _extrayendo_ cambios de código hacia y desde su rama activa remota. Usted implementa código verificado al _combinar_ la rama remota con la rama base, lo cual activa un proceso automatizado de [compilación e implementación](#deployment-workflow) para ese entorno.

![Vista de alto nivel del flujo de trabajo de desarrollo de arquitectura Pro](../../assets/pro-dev-workflow.png)

## Flujo de trabajo de desarrollo

El entorno de integración proporciona una sola rama `integration` base que contiene su Adobe Commerce en el código de infraestructura de la nube. Puede crear una rama de entorno activa adicional. Esto permite implementar hasta dos ramas activas en contenedores de Platform as a service (PaaS). No hay límite en el número de entornos inactivos, sin embargo, cuantos más entornos inactivos haya, más tardará la consola de Cloud en cargarse.

{{enhanced-integration-envs}}

Los entornos de proyecto admiten un proceso de integración flexible y continuo. Comience por clonar la rama `integration` en la carpeta local del proyecto. Cree una o varias ramas, desarrolle nuevas funciones, configure cambios, añada extensiones e implemente actualizaciones:

- **Recuperar** cambios de `integration`

- **Rama** de `integration`

- **Desarrollar** código en una estación de trabajo local, incluidas [!DNL Composer] actualizaciones

- **Insertar** cambios de código en remoto y validar

- **Combinar** con `integration` y probar

Con una rama de código desarrollada y los archivos de configuración correspondientes, los cambios de código están listos para combinarse en la rama `integration` para realizar pruebas más completas. El entorno `integration` también es mejor para:

- **Integración de servicios de terceros**: no todos los servicios están disponibles en el entorno PaaS.

- **Generando archivos de administración de configuración**: algunas opciones de configuración son de _solo lectura_ en un entorno implementado.

- **Configuración de la tienda**: debe configurar completamente todos los ajustes de la tienda mediante el entorno de integración. Puede encontrar la **URL de administrador de tienda** en la vista del entorno _integration_ en _[!DNL Cloud Console]_.

## Flujo de trabajo de implementación

Cada vez que inserta código desde la estación de trabajo local en el entorno remoto o combina código en una rama de entorno, los scripts de compilación e implementación generan código nuevo y proporcionan los servicios configurados al entorno remoto.

Acciones de script de compilación:

- El sitio en el entorno de destino sigue ejecutándose durante la compilación

- Compruebe y ejecute Adobe Commerce en los parches y revisiones de la infraestructura en la nube

- Compilar código con un registro de compilación e implementación

- Compruebe la administración de la configuración, la implementación de contenido estático se produce durante esta fase

- Cree o utilice un slug de código no modificado para acelerar el proceso

- Aprovisionar todos los servicios y aplicaciones back-end

Implementar acciones de script:

- Coloque el sitio en el entorno de destino en modo _Mantenimiento_

- Implementar contenido estático si no se completa durante la compilación

- Instalar o actualizar Adobe Commerce en la infraestructura en la nube

- Configurar el enrutamiento para el tráfico

Después del proceso de compilación e implementación, la tienda vuelve a estar en línea con los cambios y configuraciones de código más recientes. Consulte [Proceso de implementación](../deploy/process.md).

### Combinar para integración

Combine todos los cambios de código comprobados combinando la rama de desarrollo activa en la rama de base `integration`. Puede probar todos los cambios en la rama `integration` antes de promocionar los cambios en el entorno de ensayo.

### Combinar para ensayo

El ensayo es un entorno de preproducción que proporciona todos los servicios y configuraciones lo más cerca posible del entorno de producción. Inserte siempre los cambios de código del entorno `integration` al entorno `staging` para que pueda realizar pruebas exhaustivas con todos los servicios. La primera vez que utilice el entorno de ensayo, deberá configurar servicios como [Fastly CDN](../cdn/fastly.md) y [New Relic](../monitor/new-relic-service.md). Configure puertas de enlace de pago, envíos, notificaciones y otros servicios vitales con credenciales de zona protegida o de prueba.

Es mejor probar a fondo cada servicio, verificar sus herramientas de prueba de rendimiento y realizar pruebas UAT como administrador y como cliente, hasta que sienta que su tienda está lista para el entorno de producción. Ver [Implementar tu tienda](../deploy/staging-production.md).

{{second-staging}}

### Combinar en producción

Después de realizar pruebas exhaustivas en el entorno de ensayo, combine con el entorno de producción y realice pruebas exhaustivas con credenciales activas. En el momento en que inicie el sitio de producción, los clientes deben poder realizar las compras y los administradores deben poder administrar la tienda en directo. Consulte los siguientes temas para ver una explicación detallada y clara sobre cómo implementar su tienda y ponerla en marcha:

- [Implementar la tienda](../deploy/staging-production.md)
- [Lanzamiento del sitio](../launch/overview.md)

### Combinar en el patrón global

Inserte siempre una copia del código de producción en el `master` global en caso de que surja una necesidad de depurar el entorno de producción sin interrumpir los servicios.

**no** crea una rama a partir de la información global `master`. Utilice la rama `integration` para crear ramas nuevas y activas para el desarrollo y las correcciones.
