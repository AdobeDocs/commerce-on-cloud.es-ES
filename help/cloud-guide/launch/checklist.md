---
title: Iniciar lista de comprobación
description: Revise los elementos de la lista de comprobación para el lanzamiento del sitio.
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1104'
ht-degree: 0%

---

# Iniciar lista de comprobación

Antes de implementar en el entorno de producción, descargue la [lista de comprobación de Launch](../../assets/adobe-commerce-cloud-prelaunch-checklist.pdf) y utilícela con estas instrucciones para confirmar que ha completado todas las pruebas y la configuración necesarias. Vea una descripción general del proceso de implementación completo para Starter y Pro en [Implementar su tienda](../deploy/staging-production.md).

## Pruebas completas en producción

Consulte [Implementación de prueba](../test/staging-and-production.md) para probar todos los aspectos de sus sitios, tiendas y entornos. Estas pruebas incluyen la verificación de Fastly, las pruebas de aceptación de usuarios (UAT) y las pruebas de rendimiento.

## TLS y Fastly

El Adobe de proporciona un certificado Let&#39;s Encrypt SSL/TLS para cada entorno. Este certificado es necesario para que Fastly pueda servir tráfico seguro a través de HTTPS.

Para utilizar este certificado, debe actualizar la configuración DNS para que el Adobe pueda completar la validación del dominio y aplicar el certificado a su entorno. Cada entorno tiene un certificado único que cubre los dominios de Adobe Commerce en los sitios de infraestructura de la nube implementados en ese entorno. Se recomienda completar y actualizar la configuración durante el [proceso de configuración rápida](../cdn/fastly-configuration.md).

## Actualizar la configuración de DNS con la configuración de producción

Cuando esté listo para iniciar el sitio, debe actualizar la configuración DNS para enrutar el tráfico desde el entorno de producción a través del servicio Fastly.

**Requisitos previos:**

- [Configure y pruebe Fastly en su entorno de desarrollo](../cdn/fastly-configuration.md#)

- La configuración del entorno de producción se ha actualizado con todos los dominios necesarios

  Normalmente, trabajará con el asesor técnico del cliente para añadir todos los dominios de nivel superior y subdominios necesarios para sus tiendas. Para agregar o cambiar los dominios del entorno de producción, [Envíe un ticket de soporte de Adobe Commerce](https://support.magento.com/hc/en-us/articles/360019088251). Espere a que se confirme que la configuración del proyecto se ha actualizado.

  En Proyectos iniciales, debe agregar los dominios al proyecto. Consulte [Administrar dominios](../cdn/fastly-custom-cache-configuration.md#manage-domains).

- Certificado SSL/TLS aprovisionado para los entornos de producción.

  Si agregó los registros de desafío ACME para los dominios de producción durante el proceso de configuración de Fastly, Adobe carga el certificado SSL/TLS en el entorno de producción automáticamente al actualizar la configuración DNS para enrutar el tráfico al servicio Fastly. Si no ha aprovisionado previamente el certificado o si ha actualizado los dominios, el Adobe debe completar la validación del dominio y proporcionar el certificado, lo que puede tardar hasta 12 horas.

### Para actualizar la configuración DNS para el inicio del sitio:

1. Actualice la siguiente configuración DNS para el sitio de producción:

   - Configure todas las redirecciones necesarias, especialmente si está migrando desde un sitio existente

   - Establezca el registro de recursos raíz de la zona para indicar el nombre de host

   - Reduzca el valor del tiempo de vida (TTL) para actualizar la información de DNS y dirigir a los clientes al almacén de producción correcto

     Recomendamos un valor TTL significativamente menor al cambiar el registro DNS. Este valor indica al DNS cuánto tiempo debe almacenar en caché el registro DNS. Cuando se acorta, actualiza el DNS más rápido. Por ejemplo, puede cambiar el valor TTL de tres días a 10 minutos cuando esté actualizando el sitio. Tenga en cuenta que acortar el valor TTL agrega carga a la infraestructura DNS. Restaure el valor anterior y superior después del inicio del sitio.


1. Agregue registros CNAME para señalar los subdominios del entorno de producción al servicio Fastly `prod.magentocloud.map.fastly.net`, por ejemplo:

   | Dominio o subdominio | CNAME |
   | ----------------------- | -------------------------------- |
   | `www.<domain-name>.com` | prod.magentocloud.map.fastly.net |
   | `mystore.<domain-name>.com` | prod.magentocloud.map.fastly.net |

1. Si es necesario, agregue registros A para asignar el dominio Apex (`<domain-name>.com`) a las siguientes direcciones IP de Fastly:

   | Dominio Apex | ANAME |
   | --------------- | ----------------- |
   | `<domain-name>.com` | `151.101.1.124` |
   | `<domain-name>.com` | `151.101.65.124` |
   | `<domain-name>.com` | `151.101.129.124` |
   | `<domain-name>.com` | `151.101.193.124` |

>[!IMPORTANT]
>
>Las instrucciones DNS de [RFC1034](https://www.rfc-editor.org/rfc/rfc1912) (**sección 2.4**) indican que:
>_No se permite que un registro CNAME coexista con otros datos. En otras palabras, si suzy.podunk.xx es un alias de sue.podunk.xx, no puede tener también un registro MX para suzy.podunk.edu, un registro A o incluso un registro TXT._
>
>Por este motivo, los registros DNS deben ser del tipo `CNAME` para subdominios y del tipo `A` para dominios Apex (dominios raíz). Descartar esta regla puede provocar interrupciones en el servicio de correo o en la propagación de DNS, ya que se pierde la capacidad de agregar otros registros, como MX o NS. Algunos proveedores DNS pueden evitar esto utilizando personalizaciones internas, pero seguir el estándar garantiza estabilidad y flexibilidad (como el cambio del proveedor DNS).

1. Actualice la dirección URL base.

   - Utilice SSH para iniciar sesión en el entorno de producción.

     ```bash
     magento-cloud ssh -e production
     ```

   - Utilice la CLI para cambiar la dirección URL base de su tienda.

     ```bash
     php bin/magento setup:store-config:set --base-url="https://www.<domain-name>.com/"
     ```

   **NOTA**: también puede actualizar la dirección URL base desde el administrador. Ver [URL de la tienda](https://experienceleague.adobe.com/docs/commerce-admin/stores-sales/site-store/store-urls.html) en la _Guía de experiencia de compra y tiendas Adobe Commerce_.

1. Espere unos minutos para que el sitio se actualice.

1. Pruebe el sitio.

## Verificar configuración de producción

Realice una última pasada para validar la configuración de producción de una o varias tiendas. Puede actualizar la configuración en el entorno de producción. Si la configuración es de solo lectura, es posible que tenga que abrir una conexión SSH y utilizar comandos CLI para cambiar la configuración o realizar cambios en la configuración del entorno local. Una vez completadas las actualizaciones, puede implementar los cambios en los entornos de ensayo y producción.

Se recomiendan los siguientes cambios y comprobaciones:

- [Prueba de correo electrónico saliente completada](../project/outgoing-emails.md)

- [Configuración segura para credenciales de administrador y URL de administrador base](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/security/security-admin)

- [Optimización de todas las imágenes para la web](../cdn/fastly-image-optimization.md)

- [Compruebe la configuración de minificación para HTML, JavaScript y CSS](../deploy/static-content.md)

## Verificar almacenamiento en caché rápido

- Pruebe y compruebe que el almacenamiento en caché de Fastly funciona correctamente en el sitio de producción. Para ver pruebas y comprobaciones detalladas, consulte [Pruebas rápidas](../test/staging-and-production.md#check-fastly-caching).

- [Asegúrese de que la última versión del módulo CDN de Fastly para Commerce esté instalada en el entorno de producción](../cdn/fastly-configuration.md#upgrade-the-fastly-module)

- [Asegúrese de que se ha cargado la versión más actual del código VCL de Fastly](../cdn/fastly-configuration.md#upload-vcl-to-fastly)

## Pruebas de rendimiento

Le recomendamos que revise las opciones de [Performance Toolkit](https://github.com/magento/magento2/tree/2.4/setup/performance-toolkit) como parte de su proceso de preparación previo al lanzamiento.

También puede realizar pruebas con las siguientes opciones de terceros:

- [Asedio](https://www.joedog.org/siege-home/): Software de formación y prueba de tráfico para llevar tu tienda al límite. Visite el sitio con un número configurable de clientes simulados. Siege admite autenticación básica, cookies, protocolos HTTP, HTTPS y FTP.

- [Jmeter](https://jmeter.apache.org/): Excelente prueba de carga para ayudar a medir el rendimiento para el tráfico con picos, como para las ventas flash. Cree pruebas personalizadas para ejecutar en el sitio.

- [New Relic](https://support.newrelic.com/s/) (proporcionado): ayuda a localizar procesos y áreas del sitio, lo que provoca un rendimiento lento con un tiempo de seguimiento empleado por acción, como la transmisión de datos, consultas, Redis y mucho más.

- [WebPageTest](https://www.webpagetest.org/) y [PKingdom](https://www.pingdom.com/): análisis en tiempo real del tiempo de carga de las páginas del sitio con diferentes ubicaciones de origen. El reino puede costar una tarifa. WebPageTest es una herramienta gratuita.

## Configuración de seguridad

- [Configurar el análisis de seguridad](overview.md#set-up-the-security-scan-tool)

- [Configuración segura para el usuario administrador](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/security/security-admin)

- [Configuración segura para la URL de administración](https://experienceleague.adobe.com/en/docs/commerce-admin/stores-sales/site-store/store-urls#use-a-custom-admin-url)

- [Elimine los usuarios que ya no estén en el proyecto de infraestructura de Adobe Commerce en la nube](../project/user-access.md)

- [Configurar autenticación de doble factor](https://developer.adobe.com/commerce/testing/functional-testing-framework/two-factor-authentication/)

## Monitorización del rendimiento

Puede utilizar los servicios de New Relic para la monitorización del rendimiento en entornos Pro y Starter. En las cuentas de plan Pro, proporcionamos las alertas administradas para la directiva de alertas de Adobe Commerce a fin de supervisar el rendimiento de las aplicaciones y la infraestructura mediante los agentes de infraestructura y APM de New Relic. Para obtener detalles sobre el uso de estos servicios, consulte [Supervisar el rendimiento con alertas administradas](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts).

### Siguiente paso

[Pasos del inicio](steps.md)
