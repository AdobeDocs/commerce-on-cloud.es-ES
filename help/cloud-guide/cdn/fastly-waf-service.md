---
title: Firewall de aplicaciones web (WAF)
description: Descubra cómo el servicio Fastly WAF detecta, registra y bloquea el tráfico de solicitudes maliciosas antes de que pueda dañar la red o los sitios de Adobe Commerce.
feature: Cloud, Configuration, Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '930'
ht-degree: 0%

---

# Firewall de aplicaciones web (WAF)

Con la tecnología Fastly, el servicio de cortafuegos de aplicaciones web (WAF) para Adobe Commerce en la infraestructura en la nube detecta, registra y bloquea el tráfico de solicitudes malintencionadas antes de que pueda dañar sus sitios o red. El servicio WAF solo está disponible en entornos de producción.

El servicio WAF ofrece las siguientes ventajas:

- **Cumplimiento de PCI**: la habilitación de WAF garantiza que las tiendas Adobe Commerce en entornos de producción cumplan con los requisitos de seguridad de PCI DSS 6.6.
- **Directiva predeterminada de WAF**: la directiva predeterminada de WAF, configurada y mantenida por Fastly, proporciona una colección de reglas de seguridad diseñadas para proteger las aplicaciones web de Adobe Commerce de una amplia gama de ataques, incluidos ataques de inyección, entradas malintencionadas, scripts entre sitios, exfiltración de datos, infracciones del protocolo HTTP y otras amenazas de seguridad de [OWASP Top Ten](https://owasp.org/www-project-top-ten/).
- **Incorporación y habilitación de WAF**: el Adobe implementa y habilita la directiva predeterminada de WAF en su entorno de producción en un plazo de 2 a 3 semanas después de que el aprovisionamiento sea final.
- **Soporte de operaciones y mantenimiento**—
   - Configure y administre rápidamente sus registros, reglas y Adobes para el servicio de WAF.
   - El Adobe clasifica los tickets de asistencia al cliente relacionados con problemas del servicio de WAF que bloquean el tráfico legítimo como problemas de prioridad 1.
   - Las actualizaciones automatizadas de la versión del servicio WAF garantizan una cobertura inmediata para vulnerabilidades nuevas o en evolución. Ver [Mantenimiento y actualizaciones de WAF](#waf-maintenance-and-updates).

>[!TIP]
>
>Para obtener información adicional sobre cómo mantener la conformidad con PCI para su Adobe Commerce en los almacenes de infraestructura en la nube, consulte [conformidad con PCI](https://business.adobe.com/products/magento/pci-compliance.html).

## Activación de WAF

El Adobe habilita el servicio de WAF en nuevas cuentas en un plazo de 2 a 3 semanas después de que el aprovisionamiento sea final. WAF se implementa mediante el servicio Fastly de CDN. No tiene que instalar ni mantener ningún hardware o software.

>[!NOTE]
>
>Antes de poder utilizar el servicio de WAF, debe todo el tráfico externo a su proyecto de infraestructura de Adobe Commerce en la nube debe enrutarse a través del servicio de Fastly. Ver [Configuración rápida](fastly-configuration.md).

## Cómo funciona

El servicio WAF se integra con Fastly y utiliza la lógica de caché dentro del servicio CDN de Fastly para filtrar el tráfico en los nodos globales de Fastly. Habilitamos el servicio WAF en su entorno de producción con una directiva predeterminada de WAF basada en [reglas de ModSecurity de Trustwave SpiderLabs](https://github.com/owasp-modsecurity/ModSecurity) y las diez amenazas de seguridad principales de OWASP.

El servicio WAF inspecciona el tráfico HTTP y HTTPS (solicitudes del GET y del POST) con el conjunto de reglas de WAF y bloquea el tráfico que es malicioso o que no cumple con reglas específicas. El servicio inspecciona únicamente el tráfico enlazado al origen que intenta actualizar la caché. Como resultado, detenemos la mayoría del tráfico de ataques en la caché de Fastly, lo que protege el tráfico de origen de ataques maliciosos. Al procesar únicamente el tráfico de origen, el servicio WAF preserva el rendimiento de la caché e introduce solo una latencia estimada de 1,5 milisegundos a 20 milisegundos en cada solicitud no almacenada en caché.

## Solución de problemas de solicitudes bloqueadas

Cuando el servicio de WAF está habilitado, inspecciona todo el tráfico web y de administración en relación con las reglas de WAF déclencheur y bloquea cualquier solicitud web que infrinja una regla. Cuando se bloquea una solicitud, el solicitante ve una página de error predeterminada `403 Forbidden` que incluye un ID de referencia para el evento de bloqueo.

![Página de error de WAF](../../assets/cdn/fastly-waf-403-error.png)

Puede personalizar esta página de respuesta de error desde el Administrador. Consulte [Personalizar la página de respuesta de WAF](fastly-custom-response.md#customize-the-waf-error-page).

Si su tienda o página de administración de Adobe Commerce devuelve una página de error `403 Forbidden` en respuesta a una solicitud de URL legítima, envíe un [ticket de asistencia de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket). Copie el ID de referencia de la página de respuesta de error y péguelo en la descripción del ticket.

Para identificar la respuesta de WAF para una solicitud concreta mediante New Relic, consulte lo siguiente:

- `Agent_response`: indica el código de respuesta de WAF (`200` significa bueno y `406` significa bloqueado)
- Etiquetas `sigsci`: etiqueta la solicitud a una etiqueta de ciencias de señales determinada en función de la naturaleza de la solicitud

## Mantenimiento y actualizaciones de WAF

Actualiza y despliega rápidamente parches para nuevas CVE/reglas con plantilla en función de actualizaciones de reglas de terceros comerciales, investigación de Fastly y fuentes abiertas. Actualiza rápidamente las reglas publicadas en una directiva según sea necesario o cuando los cambios en las reglas están disponibles en sus respectivas fuentes. Además, puede agregar rápidamente reglas que coincidan con las clases de reglas publicadas en la instancia de WAF de cualquier servicio después de habilitar el servicio de WAF. Estas actualizaciones garantizan una cobertura inmediata para las vulnerabilidades nuevas o en evolución.

Guarde en Adobe y administre rápidamente el proceso de actualización para asegurarse de que las reglas de WAF nuevas o modificadas funcionen correctamente en el entorno de producción antes de que las actualizaciones se implementen en modo de bloqueo.

## Problemas

Si descubre que WAF está bloqueando solicitudes legítimas, a menudo se trata de falsos positivos y es necesario omitirlos o implementar una solución en el servicio de WAF. Envíe un ticket de asistencia e incluya la dirección URL afectada, los pasos exactos para reproducir el error y la referencia del error en forma de texto (a diferencia de una captura de pantalla) para evitar errores de transcripción.

## Limitaciones

El servicio estándar de WAF con tecnología de Fastly no admite las siguientes funciones:

- Protección contra malware o mitigación de bots: considere la posibilidad de usar [listas de control de acceso](./fastly-vcl-allowlist.md) o un servicio de terceros.
- Limitación de velocidad: consulte [Limitación de velocidad](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/RATE-LIMITING.md) en la documentación de Fastly o consulte [Limitación de velocidad](https://developer.adobe.com/commerce/webapi/get-started/rate-limiting/) en la sección de seguridad de la _API web de Commerce_.
- Configuración de un extremo de registro para el cliente: vea [Servicio PrivateLink](../development/privatelink-service.md) como alternativa.

El servicio WAF le permite bloquear o permitir el tráfico en función de las direcciones IP. Puede agregar listas de control de acceso (ACL) y fragmentos de VCL personalizados al servicio Fastly para especificar las direcciones IP y la lógica VCL para bloquear o permitir el tráfico. Ver [fragmentos personalizados de VCL de Fastly](fastly-vcl-custom-snippets.md).

El servicio WAF no admite el filtrado de solicitudes TCP, UDP o ICMP. Sin embargo, esta funcionalidad la proporciona la protección DDoS integrada incluida con el servicio Fastly CDN. Consulte [Protección DDoS](fastly.md#ddos-protection).
