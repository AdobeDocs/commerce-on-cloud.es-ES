---
title: Trabajadores
description: Obtenga información acerca de cómo configurar la propiedad de trabajadores en el archivo de configuración de la aplicación  [!DNL Commerce] .
feature: Cloud, Configuration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '339'
ht-degree: 0%

---

# Propiedad Workers

Puede definir un trabajador para que se ejecute independientemente de la instancia web sin una instancia de Nginx en ejecución; sin embargo, el trabajador utiliza el mismo almacenamiento de red utilizado por la aplicación [!DNL Commerce]. No es necesario configurar un servidor web en la instancia de trabajo (mediante Node.js o Go) porque el enrutador no puede dirigir solicitudes públicas al trabajador. Esto hace que la instancia de trabajo sea ideal para tareas en segundo plano o para ejecutar continuamente tareas que corren el riesgo de bloquear una implementación.

## Configuración de un trabajador

Los trabajadores solo están disponibles para su uso con los entornos de ensayo y producción de Pro. Los entornos de integración profesional y de inicio pueden optar por usar la variable [CRON_CONSUMERS_RUNNER](../environment/variables-deploy.md#cron_consumers_runner).

Para configurar un trabajador en Ensayo o Producción profesional, [envíe un vale de soporte técnico de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=es#submit-ticket) e incluya la siguiente información:

- Identificador de proyecto
- ID de entorno
- Nombre del trabajador
- Comandos de inicio

Puede configurar un proceso por trabajador. Una configuración de trabajo básica y común en el archivo `.magento.app.yaml` podría tener el siguiente aspecto:

```yaml
workers:
    queue:
        commands:
            start: |
                php ./bin/magento queue:consumers:start commerce.eventing.event.publish
```

En este ejemplo se define un único trabajador denominado `queue`, con un nivel de asignación de recursos pequeño (tamaño S), y se ejecuta el comando `php ./bin/magento` al inicio. A continuación, el trabajador `queue` se ejecuta en cada nodo como un proceso de trabajo. Si el comando se cierra, se reinicia automáticamente.

## Comandos y anulaciones

La clave `commands.start` es necesaria para iniciar comandos con la aplicación de trabajo. Puede utilizar cualquier comando shell válido, aunque es ideal utilizar el lenguaje de su aplicación. Si el comando especificado por la clave `start` termina, se reinicia automáticamente.

>[!IMPORTANT]
>
>Los vínculos `deploy` y `post_deploy` y los comandos `crons` solo se ejecutan en el contenedor web, no en las instancias de trabajo.

### Herencia

Un trabajador hereda las definiciones de las propiedades `size`, `relationships`, `access`, `disk` y `mount`, y `variables`, a menos que se anulen explícitamente.

Las siguientes propiedades son las más utilizadas para anular [la configuración de nivel superior](properties.md):

- `size`: asigne menos recursos a un único proceso en segundo plano
- `variables`: indicar a la aplicación que se ejecute de forma diferente

### Intervalos y colas

Aunque cada trabajador se pone en cola detrás de otro, la siguiente configuración produce una separación consistente de dos segundos entre las marcas de tiempo del archivo `var/time.txt`, independientemente del tiempo de espera de ocho segundos del código PHP:

```yaml
workers:
    time1:
        commands:
            start: 'php -r "sleep(8); echo time() . PHP_EOL;" >> var/time.txt& sleep 2'
```
