---
title: Lanzamiento del sitio
description: Obtenga información sobre cómo comenzar la preparación del lanzamiento del sitio.
exl-id: 95abc7aa-ed4d-44f7-96aa-517c646bc00d
source-git-commit: 38ac38d4edd0f317155d0d4537021a29a21d5761
workflow-type: tm+mt
source-wordcount: '924'
ht-degree: 0%

---

# Lanzamiento del sitio

Cuando haya completado la implementación y las pruebas en los entornos de integración y ensayo, puede comenzar la preparación del lanzamiento del sitio. En primer lugar, debe completar todo el desarrollo y las pruebas antes de trabajar en el entorno de producción. ¿Se siente listo para el lanzamiento? Revise las listas de comprobación, las prácticas recomendadas y los pasos finales para iniciar el sitio.

Si ha comprobado esta información antes de implementar y probar en Ensayo, considere la posibilidad de revisar los beneficios de probar en Ensayo primero en la siguiente sección. El ensayo es un entorno casi de producción que se ejecuta en hardware, configuraciones, arquitectura y servicios similares. Puede reducir el tiempo de inactividad y hacer que la extensión, el servicio, las configuraciones personalizadas y las pruebas de aceptación de usuarios comerciales sean componentes vitales para el lanzamiento de sus sitios y tiendas.

## ¿Por qué realizar pruebas completas en integración, ensayo y producción?

Recomendamos encarecidamente realizar pruebas en los entornos de integración, ensayo y producción debido a la complejidad de garantizar que el código personalizado, los temas, las extensiones y las integraciones de terceros trabajen juntos para hacer funcionar sus tiendas. Los siguientes son problemas comunes que pueden descubrirse y resolverse al completar las pruebas en los entornos de integración y ensayo antes de actualizar el entorno de producción:

- El ensayo es compatible con todos los servicios de producción, funciones, datos de base de datos, pila de tecnología, arquitectura y mucho más. Refleja Producción, lo que significa que si se producen errores en Ensayo, tiene una advertencia antes de que se produzcan en Producción.

- Es posible que el código que funciona en su entorno de integración local no funcione en los entornos de ensayo y producción.

- Los entornos de integración no son compatibles con algunos servicios disponibles en Ensayo y Producción, como Fastly y New Relic.

- [Pruebe por completo](../test/guidance.md) su sitio con varias herramientas en Ensayo para comprobar la carga, el estrés, el rendimiento y los recursos del sitio.

- Dado que es posible que los entornos de integración solo tengan bases de datos rellenadas con datos de prueba que no coincidan con un entorno de tipo producción, es posible que encuentre errores adicionales o un comportamiento inesperado al realizar pruebas en entornos de ensayo o producción.

## Requisitos previos para el lanzamiento del sitio

Necesita la siguiente información y recursos para prepararse para el lanzamiento del sitio:

- Información de registro CNAME para configurar el DNS

- Lista de todos los dominios de tienda que se agregarán al certificado

- Certificado SSL/TLS

Como parte de la suscripción a la infraestructura en la nube de Adobe Commerce, Adobe proporciona un certificado SSL/TLS validado por el dominio y emitido por Let&#39;s Encrypt. Cada entorno de Pro Production, Staging y Starter Production (`master`) tiene un certificado único que cubre todos los dominios y subdominios de ese entorno. Estos certificados se aprovisionan y cargan en el sitio automáticamente después de actualizar la configuración DNS para desarrollo y producción. Consulte [Aprovisionar certificados SSL/TLS](../cdn/fastly-configuration.md#provision-ssltls-certificates).

>[!NOTE]
>
>Si desea implementar su propio certificado SSL de validación extendida para su compañía en lugar de usar el certificado Let&#39;s Encrypt, póngase en contacto con su CTA o [Envíe un ticket de asistencia de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=es#submit-ticket).

## Configurar el escáner de seguridad

>[!NOTE]
>
>La herramienta de análisis de seguridad utiliza las siguientes direcciones IP públicas:
>
>```text
>52.87.98.44
>34.196.167.176
>3.218.25.102
>```
>
>Añada estas direcciones IP a una lista de permitidos de las reglas del cortafuegos de la red para permitir que la herramienta analice el sitio. La herramienta envía solicitudes únicamente a los puertos 80 y 443.

La herramienta de análisis de seguridad le permite supervisar periódicamente los sitios web de sus tiendas y recibir actualizaciones para detectar riesgos de seguridad conocidos, malware y software obsoleto. Esta herramienta es un servicio gratuito disponible para todas las implementaciones y versiones de Adobe Commerce en la infraestructura en la nube. Puede acceder a la herramienta a través de su [cuenta de Commerce Marketplace](https://account.magento.com/customer/account/login).

- Monitorizar el estado de seguridad de los sitios y las actualizaciones de seguridad aplicadas

- Recibir actualizaciones de seguridad y notificaciones específicas del sitio

>[!NOTE]
>
>Adobe recomienda utilizar Security Scan Tool sobre otras herramientas de terceros para garantizar la mejor calidad de servicio durante la investigación de los resultados.

Consulte la [Guía del usuario](https://experienceleague.adobe.com/es/docs/commerce-admin/systems/security/security-scan) para obtener información sobre cómo configurar y utilizar la herramienta de análisis de seguridad. Normalmente, empezará a utilizar esta herramienta cuando comience la prueba de aceptación de usuarios (UAT).

Cada sitio que se analiza debe registrarse a través de la ficha Security Scan. Durante el proceso de registro, debe aceptar el descargo de responsabilidad antes de empezar a escanear. Controla la programación y autoriza al usuario a recibir notificaciones cuando se completa cada análisis. Puede programar análisis para una fecha y hora específica y recurrente, o ejecutar un análisis bajo demanda según sea necesario.

Security Scan Tool utiliza varias cadenas de agente de usuario para simular la actividad de malware en la vida real. Puede ver los siguientes agentes de usuario en los registros de acceso o análisis:

```text
Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:57.0) Gecko/20100101 Firefox/57.0
GuzzleHttp/6.3.3 curl/7.29.0 PHP/7.1.18
Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/37.0.2062.120 Safari/537.36
Visbot/2.0 (+http://www.visvo.com/en/webmasters.jsp;bot@visvo.com)
```

## Analizar el sitio

1. Acceda a su [cuenta de Commerce Marketplace](https://account.magento.com/customer/account/login).

1. Haga clic en la ficha Examen de seguridad y seleccione **Ir al examen de seguridad**.

1. En la columna _Acciones_ del sitio, seleccione **Ejecutar análisis**. El estado de notificación muestra el análisis programado.

### Para revisar el informe:

1. Cuando el informe se completa, aparece una notificación.

1. En la fila del sitio, seleccione el informe que desee ver en la columna **Informes**. El pedido es del más reciente al más antiguo.

El informe enumera los problemas, incluidos los análisis erróneos, los resultados no identificados y los análisis correctos. Cada entrada proporciona información detallada para el análisis, una lista de problemas que se deben investigar y las acciones que se deben realizar. Algunas de estas acciones pueden requerir la descarga e instalación de parches de seguridad. Añada los parches necesarios a una rama de desarrollo de la estación de trabajo local antes de añadirlos a la rama de producción.

Los resultados del análisis incluyen una etiqueta que describe el estado de aprobación o fallo del análisis con información detallada sobre las comprobaciones realizadas:

- &quot;Error&quot; indica que el sitio web contiene una vulnerabilidad grave.

- &quot;No identificado&quot; sugiere que su equipo o proveedor de alojamiento requiere una revisión más profunda para determinar si se requieren más acciones.

Los resultados del análisis también proporcionan pasos de corrección sugeridos para cada prueba de seguridad fallida. Los resultados del análisis de seguridad están protegidos y solo los puede ver el usuario registrado. Solo los usuarios designados en el proceso de registro del sitio reciben notificaciones de finalización de análisis.

## Listo para iniciar el sitio

Cuando esté listo para iniciar el proceso de lanzamiento del sitio, consulte lo siguiente:

- [Iniciar lista de comprobación](checklist.md)

- [Pasos del inicio](steps.md)
