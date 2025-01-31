---
title: Arquitectura a escala
description: Obtenga información acerca de la arquitectura de dos niveles y cómo se adapta a la demanda.
feature: Cloud, Auto Scaling, Iaas, Logs
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '816'
ht-degree: 0%

---

# Arquitectura a escala

La infraestructura de Cloud se adapta según los requisitos de sus recursos para lograr una mayor eficiencia. La infraestructura de Adobe Commerce en la nube supervisa sus aplicaciones y puede ajustar la capacidad para mantener un rendimiento estable y predecible. La conversión a esta arquitectura ayuda a mitigar problemas, como la latencia o los grandes picos de tráfico.

>[!NOTE]
>
>La arquitectura escalada está disponible para Adobe Commerce en cuentas de infraestructura en la nube con el clúster Pro 48 o superior.

## Arquitectura de varios niveles

Históricamente, la arquitectura Pro constaba de tres nodos, cada uno de los cuales contenía una pila tecnológica completa. Ahora, existe una infraestructura escalable que proporciona una arquitectura por niveles con un mínimo de seis nodos: tres nodos para la base de datos y los servicios principales y tres nodos para el servidor web. Esta arquitectura de niveles divididos proporciona la capacidad de escalar niveles de forma independiente para lograr un equilibrio óptimo de rendimiento.

### Nivel de servicio

Hay tres nodos de servicio para almacenamiento de datos, caché y servicios: **OpenSearch** o **Elasticsearch**, **MariaDB**, **Redis** y más. Cuando el nivel de servicio se acerca a la capacidad, la única manera de escalarlo es aumentar el tamaño del servidor, como aumentar la potencia y la memoria de CPU. La capacidad está limitada al tamaño del nodo disponible. Debido a que el clúster de base de datos está diseñado para alta disponibilidad, no puede escalar horizontalmente de forma fiable con las tecnologías utilizadas.

![Escala de nivel de servicio](../../assets/scaling-service.png)

Considere un ejemplo de que el tipo de instancia del nodo de servicio es _m5.2xlarge_ con 32 Gb de RAM. Un servicio, como la base de datos, utiliza una cantidad considerable de memoria (30 Gb). Ampliar al siguiente tamaño de instancia disponible _m5.4xlarge_ proporciona 64 Gb de RAM, lo que duplica la memoria y se adapta a las crecientes necesidades de la base de datos.

Puede optimizar aún más el rendimiento del nivel de servicio enrutando el tráfico en función del tipo de nodo. De forma predeterminada, el nodo de la base de datos está aislado del tráfico web. Por ejemplo, puede optar por servir tráfico web en el nodo de la base de datos.

### Nivel web

Hay tres nodos web para procesar solicitudes y tráfico web: **php-fpm** y **NGINX**. Además del escalado vertical al aumentar la potencia y la memoria, el nivel web puede escalarse horizontalmente al añadir servidores web a un clúster existente cuando se restringe al nivel de PHP. Consulte [Escalado automático](autoscaling.md) para conocer la escala automática de los nodos web.

![Escala de nivel web](../../assets/scaling-web.png)

Esto complementa la escala vertical proporcionada por el nivel de servicio. A medida que el nivel de servicio se amplía en tamaño y potencia para adaptarse a una base de datos y un uso de servicio cada vez mayores, el nivel web se amplía en tamaño, potencia e instancias para adaptarse a un aumento en las solicitudes de procesos y a los requisitos de tráfico más altos.

Imagine un ejemplo de que el tipo de instancia del nodo web es _C5.2xlarge con ocho CPU y 16 Gb de RAM_. El número de solicitudes al sitio aumentó considerablemente. Puede agregar un nodo C5.2xlarge para controlar el aumento de procesos php-fpm o puede cambiar cada tipo de instancia a _C5.4xlarge con 16 CPU y 32 Gb RAM_. Añadir un nodo reduce el riesgo de una capacidad de sobretensión insuficiente.

## Estructura del proyecto

Como mínimo, los proyectos Pro con la arquitectura a escala tienen seis nodos disponibles.

- 3 nodos web c5.2xlarge (8 CPU, 16 Gb RAM)

- 3 nodos de servicio m5.2xlarge (8 CPU, 32 Gb RAM)

Sin embargo, cada proyecto es único y requiere supervisión del rendimiento para analizar correctamente la administración de recursos. Cada cuenta incluye el [servicio New Relic](../monitor/new-relic-service.md), que se conecta automáticamente con los datos de la aplicación y el análisis de rendimiento para proporcionar supervisión dinámica del servidor. En concreto, puede utilizar el servicio New Relic para supervisar el uso de CPU y RAM con el fin de determinar qué nodos requieren recursos adicionales. A medida que un recurso alcanza su capacidad o observa una degradación del rendimiento basada en el análisis, puede crear una solicitud para escalar la infraestructura y satisfacer la demanda.

### Acceso SSH

Algunos archivos y registros, como el directorio `/app/<project-id>/var/log`, no se comparten entre nodos. Cada nodo tiene un acceso SSH único. No puede usar la CLI `magento-cloud` para iniciar sesión en el servicio o en los nodos web, pero puede encontrar las direcciones de los nodos en la lista de acceso SSH en su [!DNL Cloud Console].

```bash
ssh <node>.<project-ID>-<environment>-<user-ID>@ssh.<region>.magento.com
```

- `node` de 1 a 3: direcciones para acceder a los nodos de servicio

- `node` de 4 a _n_: direcciones para acceder a los nodos web

>[!TIP]
>
>Después de iniciar sesión, puede confirmar el identificador de servidor y el rol: los nodos de servicio utilizan el rol _Unified_ y los nodos web utilizan el rol _web_.

Respuesta de ejemplo al iniciar sesión en un **nodo de servicio** incluye el rol _unificado_:

```
 __  __                   _          ___ _             _
|  \/  |__ _ __ _ ___ _ _| |_ ___   / __| |___ _  _ __| |
| |\/| / _` / _` / -_) ' \  _/ _ \ | (__| / _ \ || / _` |
|_|  |_\__,_\__, \___|_||_\__\___/  \___|_\___/\_,_\__,_|
            |___/

 Welcome to Magento Cloud.

 This is server unique-server-id, role project-id:unified.

project-id@server-id:~$
```

Respuesta de ejemplo al iniciar sesión en un **nodo web** incluye el rol _web_:

```
 __  __                   _          ___ _             _
|  \/  |__ _ __ _ ___ _ _| |_ ___   / __| |___ _  _ __| |
| |\/| / _` / _` / -_) ' \  _/ _ \ | (__| / _ \ || / _` |
|_|  |_\__,_\__, \___|_||_\__\___/  \___|_\___/\_,_\__,_|
            |___/

 Welcome to Magento Cloud.

 This is server unique-server-id, role project-id:web.

project-id@server-id:~$
```

### Ubicaciones de registro

Las ubicaciones de registro varían ligeramente según el nodo. Por ejemplo, un registro de base de datos, como el **registro de errores MySQL**, está disponible en un nodo de servicio (`/var/log/mysql/mysql-error.log`), pero no está disponible en un nodo web.

Cada cuenta Pro incluye el [servicio New Relic Logs](../monitor/new-relic-service.md), que se conecta automáticamente con los datos de registro de la aplicación para proporcionar administración dinámica de registros. Los datos de registro agregados de todos los nodos se muestran en la aplicación New Relic Logs para que pueda solucionar problemas de rendimiento en nodos específicos desde un solo panel.
