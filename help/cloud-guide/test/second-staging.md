---
title: Segundo entorno de ensayo
description: Conozca las ventajas y los usos de tener un segundo entorno de ensayo para pruebas paralelas, aislamiento de problemas, control de versiones y más.
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '716'
ht-degree: 0%

---

# Segundo entorno de ensayo

En las infraestructuras de Adobe Commerce Cloud, el entorno de ensayo garantiza pruebas y validaciones completas en un entorno que puede reflejar el entorno de producción. Este entorno le permite identificar y resolver errores de forma segura, a la vez que se asegura de que las nuevas funciones o cambios se integren sin problemas antes de que afecten a su sitio activo.

Algunos proyectos requieren un flujo de trabajo de desarrollo más sofisticado. Para satisfacer esta necesidad, Adobe ofrece un entorno de ensayo adicional como opción complementaria a su infraestructura en la nube. Contar con dos entornos de ensayo resulta ventajoso para proyectos complejos y para la colaboración simultánea entre equipos.

Tener un entorno de ensayo secundario ofrece las siguientes ventajas:

- **Pruebas paralelas**: con dos entornos de ensayo, los equipos pueden probar simultáneamente nuevas características y actualizaciones críticas sin interferir en el desarrollo de los demás. Por ejemplo, puede dedicar un entorno a las pruebas de funciones, mientras que reserva el otro entorno para las pruebas de rendimiento y de estrés.

- **Aislamiento de problemas**: cuando surjan errores o problemas en el entorno de ensayo principal, puede replicarlos y diagnosticarlos en el entorno secundario sin detener el progreso de las pruebas. Esto garantiza que la resolución de problemas no interfiera con las pruebas en curso.

- **Control de versiones**: administre las distintas versiones de la aplicación de forma más eficaz. El entorno de ensayo principal puede ejecutar la última compilación estable, mientras que el secundario prueba las funciones experimentales. Esto le ayuda a administrar mejor los ciclos de lanzamiento y garantiza que los cambios experimentales no interrumpan las compilaciones estables.

- **Planificación de despliegue mejorada**: puede simular diferentes escenarios de implementación. El entorno de ensayo principal puede imitar la configuración de producción actual, mientras que el entorno secundario se puede configurar para probar próximas versiones.

- **Pruebas de aceptación del usuario (UAT)**: mientras utiliza el entorno principal para el desarrollo en curso, puede dedicar el entorno secundario a UAT. Esto permite a los usuarios o clientes probar y proporcionar comentarios sobre las nuevas funciones en una configuración controlada, sin afectar a las pruebas.

- **Pruebas de regresión**: puede utilizar un entorno para pruebas de cara al futuro y otro para pruebas de regresión, asegurándose de que los nuevos cambios no rompan la funcionalidad existente.

- **Formación y demostraciones**: utilice un entorno para formar a los nuevos integrantes del equipo o mostrar nuevas características a las partes interesadas. Esto garantiza que las actividades de formación no interrumpan las pruebas.

- **Configuración de vínculo privado**: los entornos principal e de integración no admiten la configuración de vínculo privado. Si el flujo de trabajo de desarrollo requiere entornos adicionales conectados mediante Private Link para fines de desarrollo, prueba o cualquier otro fin, considere la posibilidad de agregar un entorno de ensayo adicional.

Tener dos entornos de ensayo puede mejorar considerablemente la eficacia del flujo de trabajo, la gestión de riesgos y la calidad general de la aplicación. Estos entornos proporcionan seguridad y flexibilidad que un entorno de ensayo no sería capaz de ofrecer.

>[!NOTE]
>
>Esta configuración es un complemento. Los clientes que deseen un entorno secundario deben ponerse en contacto con su representante de ventas para solicitar uno. El representante de ventas puede proporcionar información sobre los precios y el proceso de aprovisionamiento.

Si el proyecto ya tiene un entorno de ensayo adicional o está considerando actualizar el proyecto actual, considere las siguientes responsabilidades y plazos de aprovisionamiento:

- El proceso de aprovisionamiento puede tardar hasta dos semanas después de confirmar la solicitud con el representante de ventas de Adobe.

- Los nuevos entornos de ensayo solo están disponibles en la misma región que los entornos de ensayo y producción actuales.

- Debe actualizar el entorno principal o de integración y especificar en cuál de los entornos se basará el entorno de ensayo secundario. El entorno de ensayo secundario no se puede copiar de los entornos de ensayo o producción.

- Después de recibir el nuevo entorno de ensayo, puede incluirlo en su flujo de desarrollo. Al igual que con otros entornos, las actualizaciones de código y base de datos son su responsabilidad.

## Solicitar el entorno

Para facilitar la solicitud, siga los pasos:

1. Actualice la rama.

   Actualice la rama principal o de integración con el código y la base de datos que desee utilizar para el nuevo entorno.

1. Póngase en contacto con su representante de ventas.

   Póngase en contacto con el representante de ventas de Adobe y proporcióneles la rama específica que desee utilizar como base para el nuevo entorno de ensayo (integración o principal).

1. Confirme los detalles.

   Después de confirmar los detalles con su representante de ventas, espere a que confirme que la solicitud de aprovisionamiento se ha recibido y se está procesando. Una vez completado el proceso de aprovisionamiento, el equipo de Adobe le enviará una confirmación.

1. Implemente en su nuevo entorno.

   Siga los [pasos de implementación estándar](../deploy/staging-production.md) para implementar su código y base de datos en el nuevo entorno de ensayo.
