---
title: Pruebas de ensayo y producción
description: Aprenda a realizar pruebas en los entornos de ensayo y producción.
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1329'
ht-degree: 0%

---

# Pruebas de ensayo y producción

Después de una migración correcta del código, los archivos y los datos a Ensayo o Producción, utilice las direcciones URL del entorno para probar los sitios y almacenes. A continuación se proporciona información sobre la verificación de registros, la prueba de configuraciones de Fastly, las pruebas de aceptación de usuarios (UAT) y más.

{{second-staging}}

## Archivos de registro

Si se producen errores en la implementación u otros problemas durante la prueba, compruebe los archivos de registro. Los archivos de registro se encuentran en el directorio `var/log`.

El registro de implementación está en `/var/log/platform/<prodject-ID>/deploy.log`. El valor de `<project-ID>` depende del identificador de proyecto y de si el entorno es de ensayo o de producción. Por ejemplo, con un id. de proyecto de `yw1unoukjcawe`, el usuario de ensayo es `yw1unoukjcawe_stg` y el usuario de producción es `yw1unoukjcawe`.

Al acceder a los registros en entornos de producción o ensayo, utilice SSH para iniciar sesión en cada uno de los tres nodos y localizar los registros. O bien, puede usar [administración de registros de New Relic](../monitor/log-management.md) para ver y consultar los datos de registro agregados de todos los nodos. Ver [Ver registros](log-locations.md#application-logs).

## Compruebe el código base

Compruebe que la base de código se ha implementado correctamente en los entornos de ensayo y producción. Los entornos deben tener bases de código idénticas.

## Comprobar configuración

Compruebe los ajustes de configuración a través del panel de administración, incluida la URL básica, la URL básica de administración, la configuración de varios sitios y mucho más. Si debe realizar algún cambio adicional, complete las ediciones en la rama Git local e inserte en la rama `master` en Integración, ensayo y producción.

## Comprobar almacenamiento en caché rápido

[Configurar Fastly](../cdn/fastly-configuration.md) requiere una atención cuidadosa a los detalles: usar el ID de servicio Fastly y las credenciales de token de API Fastly correctos, cargar el código VCL de Fastly, actualizar la configuración de DNS y aplicar los certificados SSL/TLS a sus entornos. Después de completar estas tareas de configuración, puede verificar el almacenamiento en caché de Fastly en los entornos de ensayo y producción.

**Para comprobar la configuración del servicio de Fastly**:

1. Inicie sesión en el administrador de ensayo y producción mediante la dirección URL con `/admin` o la [dirección URL del administrador actualizada](../environment/variables-admin.md#admin-url).

1. Vaya a **Tiendas** > **Configuración** > **Configuración** > **Avanzada** > **Sistema**. Desplácese y haga clic en **Caché de página completa**.

1. Asegúrese de que el valor **Aplicación de almacenamiento en caché** esté establecido en _Fastly CDN_

1. Pruebe las credenciales de Fastly.

   - Haga clic en **Configuración rápida**.

   - Compruebe que los valores del ID del servicio de Fastly y las credenciales del token de la API de Fastly. Ver [Obtener credenciales de Fastly](/help/cloud-guide/cdn/fastly-configuration.md#get-fastly-credentials).

   - Haga clic en **Probar credenciales**.

   >[!WARNING]
   >
   >Asegúrese de haber introducido el ID de servicio rápido y el token de API correctos en los entornos de ensayo y producción. Las credenciales de Fastly se crean y asignan por entorno de servicio. Si introduce credenciales de ensayo en el entorno de producción, no podrá cargar los fragmentos de VCL, el almacenamiento en caché no funciona correctamente y la configuración del almacenamiento en caché apunta al servidor y a los almacenes incorrectos.

**Para comprobar el comportamiento del almacenamiento en caché de Fastly**:

1. Busque encabezados mediante la utilidad de línea de comandos `dig` para obtener información acerca de la configuración del sitio.

   Puede utilizar cualquier dirección URL con el comando `dig`. Los siguientes ejemplos utilizan direcciones URL de Pro:

   - Ensayo: `dig https://mcstaging.<your-domain>.com`
   - Producción: `dig https://mcprod.<your-domain>.com`

   Para obtener más pruebas de `dig`, vea Prueba [de Fastly antes de cambiar el DNS](https://docs.fastly.com/en/guides/working-with-domains).

1. Use `cURL` para comprobar la información del encabezado de respuesta.

   ```bash
   curl https://mcstaging.<your-domain>.com -H "host: mcstaging.<your-domain.com>" -k -vo /dev/null -H Fastly-Debug:1
   ```

   Consulte [Comprobar encabezados de respuesta](../cdn/fastly-troubleshooting.md#check-cache-hit-and-miss-response-headers) para obtener más información sobre cómo comprobar los encabezados.

1. Una vez que esté activo, use `cURL` para revisar su sitio activo.

   ```bash
   curl https://<your-domain> -k -vo /dev/null -H Fastly-Debug:1
   ```

## Prueba UAT completa

Prueba de aceptación del usuario (UAT) completa en ensayo y producción. Las siguientes pruebas son una lista rápida de las posibles tareas y áreas que se pueden probar como comerciante y cliente. La lista puede ser más larga e incluir pruebas adicionales para módulos personalizados, extensiones e integraciones de terceros. Al realizar pruebas, utilice sobremesas, portátiles y dispositivos móviles.

Si tiene problemas, guarde los pasos de reproducción, los mensajes de error, las capturas de pantalla extrañas y los vínculos. Utilice esta información para investigar y corregir los problemas en el código y las configuraciones del entorno de integración o la configuración del entorno.

<table>
<tr>
<td style="width:150px">Administración de usuarios</td>
<td>
<ul>
<li>Crear y editar cuentas de cliente, comprobar correos electrónicos</li>
<li>Creación de funciones de administrador para comerciantes</li>
<li>Crear cuentas de comerciante con funciones específicas</li>
<li>Probar el acceso a la cuenta de comerciante por función</li>
</ul>
</td>
</tr>
<tr>
<td>Catálogos y productos</td>
<td>
<ul>
<li>Creación de un catálogo con productos asociados</li>
<li>Cree productos para su tienda, incluidos todos los tipos de productos: simples, configurables y agrupados</li>
<li>Añadir imágenes de productos, muestras, vídeos y otras opciones de medios</li>
<li>Configurar precios, descuentos y reglas de precios </li>
<li>Configurar funciones avanzadas, incluidos rangos de precios, productos destacados y fechas de disponibilidad</li>
<li>Modifique el inventario y verifique que se muestren y cambien los valores correctos por aumento y compra completada</li>
</ul>
</td>
</tr>
<tr>
<td>Carros de compras y cierres</td>
<td>
<ul>
<li>Busque productos y seleccione las opciones de filtrado</li>
<li>Añadir productos al carro de compras desde los resultados de búsqueda, las páginas de categorías o las páginas de productos</li>
<li>Probar todos los tipos de productos</li>
<li>Ver el carro de compras y modificar su contenido eliminando o cambiando cantidades </li>
<li>Realice el cierre de compra para verificar las cantidades del pedido con la información del carro de compras y del producto</li>
<li>Verifique que los impuestos se calculen correctamente para el carro de compras</li>
<li>Completar una compra con diferentes opciones: añadir un cupón, seleccionar el envío, introducir la información de envío y facturación y la información de pago</li>
<li>Verificar las puertas de pago y las opciones durante el pago</li>
<li>Compruebe las notificaciones en pantalla, los pedidos enumerados en la cuenta del cliente y las notificaciones por correo electrónico</li>
<li>Comprobar el cierre de compra de invitados y clientes</li>
</ul>
</td>
</tr>
<tr>
<td>Order Management</td>
<td>
<ul>
<li>Creación de un pedido para un cliente</li>
<li>Buscar y ver pedidos</li>
<li>Modificar un pedido añadiendo y eliminando productos, cambiando importes, modificando la información de envío y facturación</li>
<li>Gestionar un reembolso</li>
<li>Cancelar un pedido</li>
<li>Aplicar códigos de cupones y descuentos</li>
</ul>
</td>
</tr>
<tr>
<td>Contenido del sitio</td>
<td>
<ul>
<li>Compruebe que todas las temáticas y recursos se cargan correctamente</li>
<li>Verificar que CSS se muestra correctamente, incluidos los tamaños de medios adaptables</li>
<li>Consulta los Términos y condiciones, la política de reembolsos y otra información sobre la política</li>
<li>Compruebe la información de contacto, los vínculos y mucho más acerca de su compañía</li>
<li>Buscar productos y contenido, comprobar el filtrado de resultados</li>
<li>Comprobar el bloque de pie de página y los bloques de navegación superiores</li>
<li>Pruebe las páginas 404 y de mantenimiento</li>
</ul>
</td>
</tr>
<tr>
<td>Extensiones</td>
<td>
<ul>
<li>Compruebe todas las configuraciones de extensión, especialmente para cualquier módulo de impuestos, envíos y pagos (por ejemplo, pedidos enviados al almacén y al sistema de gestión financiera)</li>
<li>Prueba de todas las interacciones de módulos personalizados y extensiones instaladas</li>
<li>Compruebe los datos de cualquier interacción que se deba completar (pagos, pedidos, notificaciones por correo electrónico)</li>
<li>Compruebe las configuraciones por entorno de las extensiones</li>
<li>Verificar las dependencias entre módulos y extensiones funcionan</li>
<li>Comprobar todas las acciones como comerciante y cliente</li>
</ul>
</td>
</tr>
<tr>
<td>Integraciones de terceros</td>
<td>
<ul>
<li>Compruebe que los datos se guardan correctamente en Adobe Commerce y que el servicio de terceros puede exportar, insertar o acceder a ellos (por ejemplo, los pedidos se muestran en el sistema de administración de pedidos de terceros)</li>
<li>Verifique cualquier configuración e interacción por integración</li>
<li>Realice pruebas de ida y vuelta originadas en Adobe Commerce y el servicio de terceros</li>
<li>Verificar que la autenticación se complete</li>
<li>Compruebe si hay problemas registrados para actualizar las integraciones de código o mensajes de error en los paneles de control</li>
</ul>
</td>
</tr>
<tr>
<td>Pruebas back-end</td>
<td>
<ul>
<li>Prueba y borrado de caché </li>
<li>Realización de reíndices y verificación de resultados</li>
<li>Compruebe los trabajos cron, compruebe si hay errores cron_schedule</li>
<li>Verificar y comprobar si hay algún problema con el script de shell</li>
<li>Compruebe los problemas registrados: registros de aplicación, registros de PHP, registros de MySQL, registros de correo electrónico</li>
</ul>
</td>
</tr>
</table>

## Pruebas de carga y esfuerzo

Antes de iniciar, es mejor realizar pruebas exhaustivas de tráfico y rendimiento en los entornos de ensayo y producción. Considere la posibilidad de realizar pruebas de rendimiento para los procesos de front-end y back-end.

Antes de comenzar la prueba, escriba un ticket con asistencia técnica que indique los entornos que está probando, las herramientas que está utilizando y el lapso de tiempo. Actualice el ticket con resultados e información para rastrear el rendimiento. Cuando termine la prueba, añada los resultados actualizados y anote que la prueba de ticket se ha completado con una marca de fecha y hora.

Revise las opciones de [Performance Toolkit](https://github.com/magento/magento2/tree/2.4/setup/performance-toolkit) como parte de su proceso de preparación previo al lanzamiento.

Para obtener los mejores resultados, utilice las siguientes herramientas:

- [Prueba de rendimiento de la aplicación](../environment/variables-post-deploy.md#ttfb_tested_pages): pruebe el rendimiento de la aplicación configurando la variable de entorno `TTFB_TESTED_PAGES` para probar el tiempo de respuesta del sitio.
- [Asedio](https://www.joedog.org/siege-home/): software de pruebas y configuración de tráfico para llevar tu tienda al límite. Visite el sitio con un número configurable de clientes simulados. Siege admite autenticación básica, cookies, protocolos HTTP, HTTPS y FTP.
- [Jmeter](https://jmeter.apache.org): Excelentes pruebas de carga para medir el rendimiento del tráfico con picos, como las ventas flash. Cree pruebas personalizadas para ejecutar en el sitio.
- [New Relic](../monitor/new-relic-service.md) (proporcionado): ayuda a localizar procesos y áreas del sitio que causan un rendimiento lento con un tiempo de seguimiento empleado por acción, como la transmisión de datos, consultas, Redis y más.
- [WebPageTest](https://www.webpagetest.org) y [PKingdom](https://www.pingdom.com): análisis en tiempo real del tiempo de carga de las páginas del sitio con diferentes ubicaciones de origen. El reino puede requerir una tarifa. WebPageTest es una herramienta gratuita.

## Pruebas funcionales

Puede utilizar el Marco de prueba funcional de Magento (MFTF) para completar las pruebas funcionales de Adobe Commerce desde el entorno de Cloud Docker. Consulte [Pruebas de aplicaciones](https://developer.adobe.com/commerce/cloud-tools/docker/test/application-testing/) en la guía de _Cloud Docker para Commerce_.

## Configurar el escáner de seguridad

Hay una herramienta gratuita de exploración de seguridad para sus sitios. Para agregar sus sitios y ejecutar la herramienta, consulte [Herramienta de exploración de seguridad](../launch/overview.md#set-up-the-security-scan-tool).
