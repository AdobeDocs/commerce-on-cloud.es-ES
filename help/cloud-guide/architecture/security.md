---
title: Seguridad de infraestructura en nube
description: Obtenga información sobre cómo el Adobe mantiene seguro Adobe Commerce en la infraestructura en la nube.
feature: Cloud, Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1665'
ht-degree: 0%

---

# Seguridad

La arquitectura del plan Adobe Commerce [Pro](pro-architecture.md) está diseñada para proporcionar un entorno altamente seguro. Cada cliente se implementa en su propio entorno de servidor aislado, separado de otros clientes. A continuación se describen los detalles de seguridad del entorno de producción.

## Exploradores web

La mayor parte del tráfico que entra y sale del entorno de la nube proviene de los navegadores web de los consumidores. El tráfico de consumidores puede protegerse mediante HTTPS en todas las páginas del sitio web (mediante una certificación SSL compartida o el certificado SSL propio del cliente, por un cargo adicional). Las páginas de cierre de compra y de cuenta siempre se proporcionan mediante HTTPS. La práctica recomendada es utilizar HTTPS en todas las páginas.

## Red de distribución de contenido

Proporciona rápidamente una protección de red de entrega de contenido (CDN) y de denegación de servicio distribuido (DDoS). Fastly CDN ayuda a aislar el acceso directo a los servidores de origen. El DNS público solo apunta a la red de Fastly. La solución Fastly DDoS protege contra ataques de capa 3 y capa 4 altamente disruptivos, y ataques de capa 7 más complejos. Los ataques de nivel 7 se pueden bloquear mediante reglas personalizadas basadas en todas las solicitudes HTTP/HTTPS y en criterios de cliente y solicitud, incluidos encabezados, cookies, ruta de solicitud e IP de cliente, o indicadores como la geolocalización.

Ver [descripción general de los servicios de Fastly](../cdn/fastly.md).

## Cortafuegos de aplicación web

El firewall de aplicaciones web de Fastly (WAF) se utiliza para proporcionar protección adicional. WAF, basado en la nube de Fastly, utiliza reglas de terceros de fuentes comerciales y de código abierto como el conjunto de reglas principal de OWASP. Además, se emplean reglas específicas de Adobe Commerce. Los clientes están protegidos frente a ataques clave en el nivel de la aplicación, incluidos ataques de inyección e entradas malintencionadas, scripts entre sitios, exfiltración de datos, infracciones del protocolo HTTP y otras diez amenazas principales de OWASP.

Adobe Commerce actualiza las reglas de WAF en caso de que se detecten nuevas vulnerabilidades que permitan a Managed Services &quot;aplicar parches virtuales&quot; a los problemas de seguridad antes de aplicar parches de software. Fastly WAF no proporciona servicios de limitación de velocidad o detección de bots. Si lo desea, los clientes pueden obtener una licencia de un servicio de detección de bots de terceros compatible con Fastly.

Consulte [Firewall de aplicaciones web (WAF)](../cdn/fastly-waf-service.md).

## Virtual Private Cloud

El entorno de producción del plan Adobe Commerce Pro está configurado como una nube privada virtual (VPC) para que los servidores de producción estén aislados y tengan una capacidad limitada para conectarse dentro y fuera del entorno de la nube. Solo se permiten conexiones seguras a los servidores en la nube. Se pueden utilizar protocolos seguros como SFTP o rsync para transferencias de archivos.

Los clientes pueden utilizar túneles SSH para asegurar las comunicaciones con la aplicación. El acceso a AWS PrivateLink puede proporcionarse por un suplemento. Todas las conexiones a estos servidores se controlan mediante Grupos de seguridad de AWS, un cortafuegos virtual que limita las conexiones al entorno. Los recursos técnicos de los clientes pueden acceder a estos servidores mediante SSH.

## Cifrado

Almacén de bloques elásticos de Amazon (EBS) se utiliza para el almacenamiento. Todos los volúmenes EBS se cifran con el algoritmo AES-256, lo que significa que los datos se cifran en reposo. El sistema también cifra los datos en tránsito entre la CDN y el origen, y entre los servidores de origen. Las contraseñas de los clientes se almacenan como hashes. Las credenciales confidenciales, incluidas las credenciales de la puerta de enlace de pago, se cifran mediante el algoritmo SHA-256.

La aplicación Adobe Commerce no admite el cifrado o cifrado de nivel de columna o fila cuando los datos no están en reposo o no están en tránsito entre los servidores. El cliente puede administrar las claves de cifrado desde la aplicación. Las claves que utiliza el sistema se almacenan en el sistema de administración de claves de AWS y Managed Services debe administrarlas para proporcionar partes del servicio.

## Detección y respuesta de extremos

[!DNL CrowdStrike Falcon], un agente ligero de detección y respuesta de puntos de conexión (EDR) de próxima generación está instalado en todos los puntos de conexión (incluidos los servidores) de Adobe. Los agentes EDR protegen los datos y sistemas de Adobe con monitoreo y recolección continua en tiempo real, lo que permite identificar y responder a las amenazas rápidamente.

## Pruebas de penetración

Managed Services realiza pruebas de penetración regulares del sistema en la nube de Adobe Commerce con la aplicación predeterminada. Los clientes son responsables de cualquier prueba de penetración de su aplicación personalizada.

## Pasarela de pago

Adobe Commerce requiere integraciones de puerta de enlace de pago en las que los datos de la tarjeta de crédito se pasan directamente del explorador del consumidor a la puerta de enlace de pago. Los datos de la tarjeta nunca están disponibles en ninguno de los entornos Managed Services del plan Adobe Commerce Pro. Las acciones de las transacciones realizadas por la aplicación de comercio electrónico se completan mediante una referencia a la transacción desde la puerta de enlace.

## aplicación de Adobe Commerce

El Adobe prueba regularmente el código de la aplicación principal para detectar vulnerabilidades de seguridad. Los parches para defectos y problemas de seguridad se proporcionan a los clientes. El equipo de seguridad del producto valida los productos de Adobe Commerce siguiendo las directrices de seguridad de la aplicación de OWASP. Para probar y verificar el cumplimiento se utilizan varias herramientas de evaluación de vulnerabilidades de seguridad y proveedores externos. Las herramientas de seguridad incluyen:

- Escaneado estático y dinámico de Veracode
- Análisis del código fuente RIPS
- Servicios de análisis de vulnerabilidades de Trustwave y Alert Logic
- Burp Suite Pro
- OWASPZAP
- andSqlMap

La base de código completa se analiza con estas herramientas cada dos semanas. Los parches de seguridad se notifican a los clientes por correo electrónico directo, en la aplicación y en el [Centro de seguridad](https://helpx.adobe.com/es/security.html).

Los clientes deben asegurarse de que estos parches se aplican a su aplicación personalizada dentro de los 30 días posteriores al lanzamiento, según las directrices de PCI. El Adobe también proporciona una [Herramienta de análisis de seguridad](https://experienceleague.adobe.com/es/docs/commerce-admin/systems/security/security-scan) que permite a los comerciantes supervisar sus sitios con regularidad y recibir actualizaciones sobre riesgos de seguridad conocidos, malware y acceso no autorizado. Security Scan Tool es un servicio gratuito que se puede ejecutar en cualquier versión de Adobe Commerce.

Para animar a los investigadores de seguridad a identificar y notificar vulnerabilidades, Adobe Commerce tiene un [programa de devolución de errores](https://hackerone.com/magento), además de pruebas internas. Además, se proporciona al cliente el código fuente completo de la aplicación para su propia revisión si lo desea.

## Sistema de archivos de solo lectura

Todo el código ejecutable se implementa en una imagen de sistema de archivos de solo lectura, lo que reduce drásticamente las superficies disponibles para el ataque. El proceso de implementación crea una imagen Squash-FS para reducir las oportunidades de insertar código PHP o JavaScript en el sistema o modificar los archivos de la aplicación Adobe Commerce.

## Implementación remota

La única manera de obtener código ejecutable en el entorno de producción de Managed Services es ejecutarlo a través de un proceso de aprovisionamiento. El aprovisionamiento implica insertar el código fuente del repositorio de origen en un repositorio remoto que inicie un proceso de implementación. El acceso a ese destino de implementación está controlado, por lo que tiene control total sobre quién puede acceder al destino de implementación. El cliente controla todas las implementaciones de código de aplicación en el entorno que no sea de producción.

## Registro

Todas las actividades de AWS están registradas en AWS CloudTrail. Los registros del sistema operativo, del servidor de aplicaciones y de la base de datos se almacenan en los servidores de producción y se almacenan en copias de seguridad. Todos los cambios en el código fuente se registran en un repositorio Git. El historial de implementación está disponible en la [interfaz web de proyectos](../project/overview.md) de Adobe Commerce. Todo el acceso de asistencia se registra y las sesiones de asistencia se registran.

Ver [Ver y administrar registros](../test/log-locations.md).

## Datos confidenciales

Los datos confidenciales pueden incluir información personal de los consumidores o datos confidenciales de clientes de Managed Services. La protección de los datos confidenciales de clientes y consumidores es una obligación esencial para Adobe Commerce Managed Services. Tanto los clientes de Managed Services como los de Adobe tienen obligaciones legales en relación con la información personal identificable. Además de las características de seguridad de la arquitectura, existen otros controles para limitar la distribución y el acceso a los datos confidenciales.

Los clientes son propietarios de sus datos y tienen control sobre dónde se encuentran dichos datos. El cliente especifica la ubicación donde residen sus instancias de producción y desarrollo. También especifican qué ubicación se utiliza para el entorno de informes de Adobe Commerce con Commerce y si la aplicación de informes de Adobe Commerce tiene acceso a información personal o no. Las instancias de producción se pueden encontrar en la mayoría de las regiones de AWS, mientras que los entornos de desarrollo e informes de Adobe Commerce se pueden encontrar en Estados Unidos o en la Unión Europea actualmente.

Los datos confidenciales pueden pasar a través de la red del servidor de CDN de Fastly, pero no se almacenan en la red de Fastly. Todos los socios incluidos en la oferta de Managed Services tienen obligaciones contractuales para garantizar la protección de datos confidenciales. Managed Services no mueve datos confidenciales de clientes o consumidores desde las ubicaciones especificadas por el cliente.

Como parte de la prestación de los servicios incluidos en la oferta de Managed Services, el personal de Managed Services necesita tener acceso a sistemas de producción que contengan datos confidenciales. Estos empleados reciben formación sobre sus obligaciones de proteger los datos confidenciales de los clientes y los consumidores. El acceso a estos sistemas está limitado a los empleados que requieren acceso. Estos empleados solo tienen acceso durante el tiempo necesario para prestar estos servicios.

## Reglamento general de protección de datos

El Reglamento General de Protección de Datos (RGPD) es un marco legal que establece directrices para la recopilación y el procesamiento de información personal de personas en la Unión Europea (UE). Estas regulaciones se aplican independientemente de dónde se base el sitio y de si los visitantes de la UE tienen acceso a él.

Se debe notificar a los visitantes de los datos que un sitio recopila de ellos y dar su consentimiento explícito para la recopilación de información. Los sitios deben notificar a los visitantes si se infringen los datos personales que mantiene el sitio.

El comerciante o empresa que gestione el sitio debe tener un responsable de la protección de datos que supervise la seguridad de los datos del sitio. El responsable de privacidad de datos (o el equipo de administración del sitio web) debe estar disponible para ponerse en contacto con los visitantes si estos solicitan que se borren sus datos.

El RGPD requiere que toda información de identificación personal (como nombres, raza y fecha de nacimiento) recopilada sea anónima o seudónima.

>[!NOTE]
>
>Esta página proporciona una visión general de lo que hay que tener en cuenta para el RGPD. Consulte la _[Guía de seguridad y cumplimiento](https://experienceleague.adobe.com/es/docs/commerce-operations/security-and-compliance/overview)_ para obtener detalles sobre cómo Adobe Commerce almacena información personal. Para determinar cómo su empresa debe cumplir con las obligaciones legales, consulte con su asesor legal o consulte el [texto oficial](https://eur-lex.europa.eu/eli/reg/2016/679/oj).

## Copias de seguridad

Las copias de seguridad se realizan cada hora durante las últimas 24 horas de funcionamiento. Después del período de 24 horas, las copias de seguridad se conservan según una programación mediante el servicio de instantáneas de AWS EBS. Consulte [Política de retención](pro-architecture.md#retention-policy).

El servicio crea una copia de seguridad independiente en el almacenamiento redundante. Como los volúmenes EBS están cifrados, las copias de seguridad también están cifradas. Además, Managed Services realiza copias de seguridad bajo demanda según lo solicitado por los clientes.

Su enfoque de backup y recuperación de Managed Services utiliza una arquitectura de alta disponibilidad combinada con backups de todo el sistema. Cada proyecto se duplica (todos los datos, el código y los recursos) en tres zonas de disponibilidad de AWS independientes; cada zona tiene un centro de datos independiente.

Consulte [Administración de instantáneas y copias de seguridad](../storage/snapshots.md).
