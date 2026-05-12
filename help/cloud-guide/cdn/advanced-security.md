---
title: Seguridad avanzada de Adobe Commerce
description: Descubra cómo la seguridad avanzada añade la administración de bots, la limitación avanzada de velocidad y la protección DDoS de nivel 7 a Adobe Commerce en la infraestructura en la nube.
feature: Cloud, Configuration, Security
source-git-commit: 8a7c1c297092fdf2b75d22ce99c360c85eac0495
workflow-type: tm+mt
source-wordcount: '1986'
ht-degree: 0%

---

# [!DNL Adobe Commerce Advanced Security]

[!DNL Adobe Commerce Advanced Security] es un producto que funciona con [!DNL Adobe Commerce on Cloud Infrastructure] para mantener tu tienda en línea rápida, disponible y segura. Esto puede ayudar a proteger los ingresos, reducir el tiempo de inactividad y mantener la confianza de los clientes durante los períodos de tráfico máximo y los ataques automatizados.

[!DNL Adobe Commerce on Cloud Infrastructure] incluye [protección DDoS de nivel 3 y 4](./fastly.md#ddos-protection) integrada y [Firewall de aplicaciones web (WAF)](./fastly-waf-service.md). Bajo el modelo de responsabilidad compartida [1}, la detección DDoS de nivel 7, la protección de bots y el bloqueo proactivo de IP son responsabilidades del comerciante, que [!DNL Adobe Commerce Advanced Security] está diseñado para abordar.](https://experienceleague.adobe.com/en/docs/commerce-operations/security-and-compliance/shared-responsibility)

[!DNL Advanced Security] amplía la protección de la tienda mediante funciones de seguridad perimetral con tecnología Fastly, que ofrece administración de bots, limitación avanzada de velocidad y protección DDoS de nivel 7 como parte de una plataforma perimetral unificada que combina escala, rendimiento y seguridad en el perímetro de la red.

>[!NOTE]
>
>[!DNL Advanced Security] solo está disponible para [!DNL Adobe Commerce on Cloud Infrastructure] proyectos (PaaS).

## Funcionalidades principales

[!DNL Adobe Commerce Advanced Security] incluye las siguientes protecciones adicionales:

- **[Administración de bots](https://docs.fastly.com/products/bot-management)**: identifica y mitiga la actividad de bots no deseados en las aplicaciones web. El servicio de administración de bots distingue entre bots legítimos (rastreadores de motores de búsqueda, bots de medios sociales) y maliciosos, lo que proporciona una clasificación en tiempo real en el extremo de la red con opciones para bloquear, permitir, desafiar o limitar la velocidad del tráfico.

- **[Protección DDoS](https://docs.fastly.com/products/fastly-ddos-protection)**: proporciona protección DDoS de nivel 7 (capa de aplicación) más allá de la protección actual de nivel 3 y 4 incluida en todos los [!DNL Adobe Commerce on Cloud Infrastructure] proyectos. El servicio de protección DDoS absorbe los ataques volumétricos a gran escala y garantiza la disponibilidad continua de las aplicaciones durante los eventos de denegación de servicio distribuida (DDoS), lo que protege los ingresos durante los períodos de tráfico máximo.

- **[Limitación avanzada de velocidad](https://www.fastly.com/documentation/guides/next-gen-waf/rules/working-with-advanced-rate-limiting-rules/)**: proporciona reglas configurables de limitación de velocidad que protegen de abusos las direcciones URL, los extremos de API y los recursos de la aplicación específicos. El servicio Advanced Rate Limiting va más allá de la [limitación de velocidad básica](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/RATE-LIMITING.md) disponible a través del módulo Fastly CDN para dirigirse a patrones de tráfico específicos y vectores de ataque, reduciendo la tensión de la infraestructura y los costos de la nube.

>[!NOTE]
>
>Actualmente, [!DNL Advanced Security] configuraciones requieren el envío de un ticket de asistencia. La configuración de autoservicio mediante la IU de administración está planificada para una versión futura. Consulte [Solicitud [!DNL Advanced Security]](#request-advanced-security) para obtener más información.

## Cobertura de amenazas

[!DNL Advanced Security] protege los escaparates de una serie de amenazas automatizadas y de nivel de aplicación.

![Posición de seguridad avanzada en la pila de seguridad de Adobe Commerce](../../assets/advanced-security.svg)

### Abuso impulsado por bots

- **Relleno de credenciales**: intentos automatizados de iniciar sesión con credenciales robadas de violaciones de datos.
- **Adquisición de cuenta**: bots que intentan obtener acceso no autorizado a cuentas de clientes.
- **Abuso en la creación de cuentas**: creación automatizada de cuentas falsas con fines de fraude o abuso.
- **Pruebas de tarjetas**: bots que comprueban números de tarjetas de crédito robados en el procesador de pagos.
- **Eliminación de contenido**: extracción automatizada de datos de productos, precios o contenido de tu tienda.
- **Acumulación de inventario**: bots que contienen productos en los carros de compras para evitar compras legítimas.

### Administración de bots de IA

- **Detección de rastreador de IA**: identifica y administra los rastreadores de IA que rastrean contenido para entrenar modelos de idiomas grandes sin consentimiento.
- **Control de recuperador de IA**: controla los recuperadores de IA utilizados en los resultados de Búsqueda por IA en tiempo real.
- **Políticas de bots de IA configurables**: distingue entre bots de IA verificados y sospechosos con tipos de señales configurables para la aplicación de políticas.

### Ataques de capa de aplicación

- **Ataques DDoS de nivel 7**: ataques distribuidos dirigidos a la capa de la aplicación que omiten las protecciones integradas de nivel 3 y 4. [!DNL Advanced Security] absorbe estos ataques volumétricos en el perímetro antes de que lleguen a los servidores de origen.
- **Abuso de URL y API**: Ataques dirigidos a direcciones URL o extremos de API específicos distribuidos en un gran número de direcciones IP, donde el bloqueo de IP individual no es efectivo.
- **Ataques de eliminación de caché**: solicitudes con parámetros de consulta manipulados diseñados para evitar el almacenamiento en caché de CDN y saturar el servidor de origen.

### Funciones adicionales

- **Desafíos dinámicos**: asigna automáticamente el desafío óptimo al tráfico sospechoso. Aprovecha los tokens de acceso privado (PAT) para validar sin problemas una parte de las solicitudes sin afectar a la experiencia del usuario.
- **Tecnología de engaño**: soluciona los intentos de apropiación de cuentas devolviendo información falsa a los atacantes, mitigando su ataque e interrumpiendo su capacidad para operar a escala.

## Elección de la protección adecuada

Utilice la siguiente guía para determinar si [!DNL Advanced Security] es la solución correcta para sus necesidades de protección de tienda, o si las protecciones existentes o las soluciones alternativas son más apropiadas.

### Cuándo usar [!DNL Advanced Security]

Se recomienda abordar los siguientes escenarios con [!DNL Advanced Security]:

| Escenario | Cómo ayuda [!DNL Advanced Security] |
|---|---|
| Su sitio experimenta ataques impulsados por bots como relleno de credenciales, raspado de contenido o acumulación de inventario | Administración de bots identifica y mitiga las amenazas automatizadas en el perímetro antes de que lleguen a su aplicación |
| Necesita protección DDoS de nivel 7 más allá de la cobertura integrada de nivel 3 y 4 | La protección DDoS absorbe los ataques en la capa de aplicación que evitan las protecciones en el nivel de red |
| Las direcciones URL o los extremos de API específicos están dirigidos a tráfico distribuido de gran volumen que la dirección IP no puede bloquear | La limitación avanzada de velocidad proporciona controles granulares para extremos específicos y patrones de tráfico |
| Desea administrar los rastreadores de IA y los buscadores que acceden al contenido de la tienda | La administración de bots incluye políticas configurables de detección y aplicación de bots de IA |
| Necesita una solución de seguridad perimetral compatible con Adobe integrada con su CDN de Fastly existente | [!DNL Advanced Security] se ejecuta en la misma plataforma de Fastly edge que ya atiende tu tienda |

### Cuándo utilizar las protecciones existentes

Los siguientes escenarios se abordan mejor con las protecciones existentes:

| Escenario | Enfoque recomendado |
|---|---|
| Una sola IP o un pequeño conjunto de IP identificables está inundando su sitio con solicitudes | Bloquee las IP mediante el administrador de Commerce o la API de Fastly. Use la protección DDoS de [nivel 3/4 integrada](./fastly.md#ddos-protection) y los fragmentos de VCL de [lista de bloqueados IP](./fastly-vcl-blocking.md) existentes. |
| Debe bloquear la inyección de SQL, la ejecución de scripts en sitios múltiples (XSS) u otras amenazas de OWASP Top Ten | El [servicio WAF](./fastly-waf-service.md) incluido bloquea estas amenazas automáticamente. |
| Sus patrones de ataque DDoS pueden ser controlados con reglas básicas de bloqueo de VCL | Use los [fragmentos de VCL personalizados](./fastly-vcl-custom-snippets.md) existentes que ya están disponibles con Adobe Commerce. |

### Cuándo usar protecciones alternativas

Los siguientes escenarios se abordan mejor con protecciones alternativas que pueden complementar [!DNL Advanced Security]:

| Escenario | Enfoque recomendado |
|---|---|
| Necesita puntuación de fraude a nivel de transacción o prevención de fraude de pago | Utilice una plataforma específica de prevención del fraude. [!DNL Advanced Security] protege en el nivel de red perimetral y no evalúa transacciones de pago individuales. |
| Necesita administración de identidad y acceso (IAM) | Implemente una solución IAM dedicada. La autenticación de usuarios y la administración de sesiones siguen siendo responsabilidades de los clientes. |
| Necesita pruebas de seguridad de aplicaciones estáticas o dinámicas (SAST/DAST) | Utilice las herramientas de prueba de seguridad de aplicaciones dedicadas. No se ha proporcionado el análisis de vulnerabilidades a nivel de código. |
| Necesita una seguridad de API completa que supere la limitación de velocidad (como validación de esquemas o funciones de puerta de enlace de API) | Considere una plataforma de seguridad de API dedicada. |
| Necesita herramientas de cumplimiento normativo, como el escaneo PCI o la creación de informes SOC | Utilice herramientas de administración de cumplimiento dedicadas. |

>[!TIP]
>
>Si actualmente usa un proveedor de protección contra bots de terceros, consolidarlo en [!DNL Advanced Security] puede reducir la complejidad operativa y eliminar una cobertura de seguridad incoherente entre los proveedores. Póngase en contacto con el equipo de su cuenta de Adobe para evaluar [!DNL Advanced Security] para su proyecto.

## Colocación de pila de seguridad

[!DNL Advanced Security] se ajusta a la arquitectura de seguridad de Adobe Commerce más amplia como un nivel adicional de protección basada en Edge. Funciona junto con las protecciones DDoS de nivel 3/4 y no las reemplaza, ya incluidas con [!DNL Adobe Commerce on Cloud Infrastructure]. Las siguientes secciones aclaran cómo se relaciona con las protecciones existentes y las responsabilidades que siguen en el cliente.

### Protecciones incluidas

[!DNL Adobe Commerce on Cloud Infrastructure] incluye las siguientes características de seguridad:

- **[Firewall de aplicaciones web (WAF)](./fastly-waf-service.md)**: protección administrada contra la inyección de SQL, ejecución de scripts en sitios múltiples (XSS) y otras amenazas principales de Open Web Application Security Project (OWASP). Solo disponible en entornos de producción.
- **[Protección DDoS de nivel 3 y 4](./fastly.md#ddos-protection)**: protección integrada contra ataques de nivel de red, como inundaciones SYN, inundaciones UDP, ataques basados en ICMP y ataques de nivel TCP. Se habilita automáticamente con Fastly CDN.
- **[Certificados SSL/TLS](./fastly-configuration.md#provision-ssltls-certificates)**: certificados de cifrado validados por el dominio para tráfico HTTPS seguro.
- **[Ocultación de origen](./fastly.md#origin-cloaking)**: garantiza todas las rutas de tráfico a través de Fastly, bloqueando el acceso directo a los servidores de origen.
- **[fragmentos de seguridad basados en VCL](./fastly-vcl-custom-snippets.md)**: reglas de lenguaje de configuración de barniz personalizado (VCL) para el bloqueo de IP, la inclusión en la lista de permitidos y el filtrado de solicitudes.

### [!DNL Advanced Security]

[!DNL Advanced Security] proporciona una mayor protección más allá de las protecciones integradas incluidas en [!DNL Adobe Commerce on Cloud Infrastructure], pero con un costo adicional:

- **Administración de bots**: detección y mitigación de bots basada en Edge con administración de bots de IA.
- **Protección DDoS de nivel 7**: absorción y defensa DDoS de nivel de aplicación.
- **Limitación avanzada de tarifas**: controles granulares de tarifas para direcciones URL y extremos de API.
- **Tecnología de Engaños y Desafíos Dinámicos**: Asignación de Desafíos Automatizada y Mitigación de Adquisición de Cuentas.

### Responsabilidad del cliente

- **Prevención de fraude**: Puntuación de fraude a nivel de transacción y detección de fraude de pago.
- **Administración de identidad y acceso**: autenticación del cliente, autorización y administración de sesiones.
- **Pruebas de seguridad de aplicaciones**: SAST/DAST y análisis de vulnerabilidades.
- **Configuraciones de seguridad personalizadas**: reglas basadas en VCL, listas de permitidos IP y listas de bloqueados.
- **Herramientas de cumplimiento**: análisis PCI, informes de cumplimiento SOC y herramientas de auditoría regulatoria.
- **Protección de nivel de aplicación**: autenticación de API basada en tokens, normalización de parámetros de consulta y diseño de estrategias de almacenamiento en caché.

Para obtener una descripción general completa de las responsabilidades de seguridad del cliente y Adobe, consulte el [modelo de responsabilidad compartida](https://experienceleague.adobe.com/en/docs/commerce-operations/security-and-compliance/shared-responsibility).

## Patrones de ataque y protecciones comunes

La siguiente tabla asigna patrones de ataque comunes a la capa de protección adecuada dentro de la pila de seguridad de Adobe Commerce.

| Patrón de ataque | Tipo | Protección |
|---|---|---|
| Una sola IP o un conjunto de IP identificables que envían un gran número de solicitudes | DDoS + Bot | Bloquear direcciones IP mediante la API de Commerce Admin o Fastly. La protección DDoS de nivel 3/4 incorporada filtra este tráfico en el perímetro de la red. |
| Los ataques a direcciones URL o API específicas se propagan a través de un gran número de direcciones IP | DDoS + Bot | **[!DNL Advanced Security]**: la limitación de velocidad avanzada restringe el volumen de solicitudes por dirección URL. Administración de bots identifica y bloquea el tráfico de bots distribuido. |
| Ataques automatizados en puntos finales de API de REST sin la autenticación adecuada | Bot + DDoS | Compruebe que los extremos de la API utilizan autenticación basada en token. Rotar las credenciales si el token se ve comprometido. **[!DNL Advanced Security]**: la limitación avanzada de velocidad puede proteger los extremos expuestos. |
| Ataques de eliminación de caché mediante parámetros de consulta manipulados | Bot + DDoS | Excluya los parámetros de consulta no esenciales de las claves de caché. Normalice y restrinja los parámetros de consulta en el nivel de aplicación. **[!DNL Advanced Security]**: Administración de bots detecta y bloquea el tráfico de eliminación automatizada de caché. |
| Intentos de inyección de SQL o de ejecución de scripts en sitios múltiples (XSS) | WAF | El [servicio WAF](./fastly-waf-service.md) incluido bloquea estas amenazas automáticamente mediante las reglas de seguridad administradas. |

### Comportamiento de bloqueo de WAF

El siguiente comportamiento de WAF se aplica a todos los [!DNL Adobe Commerce on Cloud Infrastructure] proyectos, independientemente de si [!DNL Advanced Security] está habilitado o no. El servicio WAF incluido utiliza el siguiente comportamiento de bloqueo para las señales de ataque comunes:

- **Las solicitudes de inyección de SQL** se bloquean inmediatamente, incluso para una única solicitud que coincida.
- Las solicitudes identificadas con las siguientes señales de amenaza de una IP maliciosa conocida se bloquean inmediatamente: Backdoor, Attack Tool, CMDEXE, Log4J JNDI, Traversal y XSS.
- Las solicitudes de direcciones IP no malintencionadas que muestran las señales de amenaza anteriores se bloquean cuando superan los umbrales siguientes:

| Intervalo | Umbral | Frecuencia de comprobación |
|---|---|---|
| 1 minuto | 50 solicitudes | Cada 20 segundos |
| 10 minutos | 350 solicitudes | Cada 3 minutos |
| 1 hora | 1.800 solicitudes | Cada 20 minutos |

## Solicitud [!DNL Advanced Security]

Para solicitar [!DNL Advanced Security]:

>[!NOTE]
>
>[!DNL Advanced Security] está disponible con un costo adicional y requiere una suscripción activa a [!DNL Adobe Commerce on Cloud Infrastructure] (PaaS).

1. Póngase en contacto con el equipo de su cuenta de Adobe o con el representante de ventas de Adobe para hablar sobre [!DNL Advanced Security] para su proyecto.

1. Después de comprar [!DNL Advanced Security], [envíe un ticket de soporte de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) solicitando la habilitación de [!DNL Advanced Security]. Incluya su ID de proyecto [!DNL Adobe Commerce on Cloud Infrastructure] y los entornos que requieren habilitación (por ejemplo, Producción y ensayo).

1. Adobe activa [!DNL Advanced Security] en su servicio de Fastly y configura las directivas de protección iniciales. La habilitación suele completarse en un plazo de pocos días hábiles a partir del envío del ticket.

1. Recibirá la confirmación de que [!DNL Advanced Security] está activo, junto con detalles sobre las protecciones habilitadas para sus entornos.

>[!NOTE]
>
>Los cambios de configuración de [!DNL Advanced Security] actualmente requieren [enviar un ticket de soporte](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket). La configuración de autoservicio mediante la IU de administración está planificada para una versión futura.

## Limitaciones

[!DNL Advanced Security] proporciona protección de tienda de nivel de borde. Las siguientes capacidades no están disponibles y se abordan mejor con soluciones complementarias:

- **Puntuación de fraude a nivel de transacción**—[!DNL Advanced Security] no evalúa las transacciones de pago individuales por riesgo de fraude. Utilice una plataforma específica de prevención del fraude para la puntuación a nivel de transacción.
- **Administración de identidad y acceso (IAM)**—[!DNL Advanced Security] no administra la autenticación de usuarios, la autorización ni la administración de sesiones. Son responsabilidades del cliente.
- **Las pruebas de seguridad de aplicaciones estáticas y dinámicas (SAST/DAST)**—[!DNL Advanced Security] no incluyen el análisis de vulnerabilidades a nivel de código ni las pruebas de penetración.
- **Seguridad de la API**: aunque la limitación avanzada de velocidad puede proteger los extremos de la API contra abusos, no se proporcionan funciones completas de seguridad de la API, como validación de esquemas y administración de puertas de enlace de API.
- **Prevención total del fraude**—[!DNL Advanced Security] se centra en la protección de tiendas de nivel perimetral y no es una plataforma completa de administración del fraude.
- **Herramientas de cumplimiento**—[!DNL Advanced Security] no proporciona las funciones de análisis PCI, informes de cumplimiento SOC o auditoría regulatoria.
