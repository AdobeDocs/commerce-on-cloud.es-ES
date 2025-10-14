---
title: Configurar el servicio ActiveMQ
description: Obtenga información sobre cómo habilitar el servicio ActiveMQ Artemis para administrar las colas de mensajes de Adobe Commerce en la infraestructura en la nube.
feature: Cloud, Services
source-git-commit: ef22de6873b49f0fb9adfa9fc343a8d738a543e9
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 0%

---

# Configurar el servicio [!DNL ActiveMQ]

[Message Queue Framework (MQF)](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/message-queues/message-queue-framework.html) es un sistema de Adobe Commerce que permite que un [módulo](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/glossary#module) publique mensajes en colas. También define los consumidores que reciben los mensajes de forma asincrónica.

El MQF puede usar [ActiveMQ Artemis](https://activemq.apache.org/components/artemis/) como agente de mensajería, que proporciona una plataforma escalable para enviar y recibir mensajes. También incluye un mecanismo para almacenar mensajes no enviados. [!DNL ActiveMQ Artemis] admite el protocolo STOMP (Protocolo de mensajería orientada a texto de transmisión) para mensajes.

[!DNL ActiveMQ Artemis] está disponible como alternativa a RabbitMQ para administrar colas de mensajes. Resulta especialmente útil cuando necesita características específicas de ActiveMQ o desea utilizar el protocolo STOMP.

>[!IMPORTANT]
>
>Si prefiere usar un servicio de Agente de mensajes existente, como [!DNL ActiveMQ], en lugar de depender de Adobe Commerce en la infraestructura de la nube para crearlo, use la variable de entorno [`QUEUE_CONFIGURATION`](../environment/variables-deploy.md#queue_configuration) para conectarlo a su sitio.

{{service-instruction}}

**Para habilitar ActiveMQ Artemis**:

1. Agregue el nombre, tipo y valor de disco necesarios (en MB) al archivo `.magento/services.yaml` junto con la versión de ActiveMQ instalada.

   ```yaml
   activemq-artemis:
       type: activemq-artemis:<version>
       disk: 1024
   ```

1. Configure las relaciones en el archivo `.magento.app.yaml`.

   ```yaml
   relationships:
       activemq-artemis: "activemq-artemis:activemq-artemis"
   ```

1. Agregue, confirme e inserte los cambios de código.

   ```bash
   git add .magento/services.yaml .magento.app.yaml
   ```

   ```bash
   git commit -m "Enable ActiveMQ Artemis service"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. [Compruebe las relaciones de servicio](services-yaml.md#service-relationships).

{{service-change-tip}}

## Conectar con ActiveMQ para depuración

Para fines de depuración, puede conectarse directamente a una instancia de servicio de una de las siguientes maneras:

- Conéctese desde su entorno de desarrollo local
- Conectar desde la aplicación
- Conéctese desde su aplicación PHP

### Conéctese desde su entorno de desarrollo local

1. Inicie sesión en la CLI de `magento-cloud` y el proyecto:

   ```bash
   magento-cloud login
   ```

1. Compruebe el entorno con ActiveMQ Artemis instalado y configurado.

   ```bash
   magento-cloud environment:checkout <environment-id>
   ```

1. Utilice SSH para conectarse al entorno de la nube:

   ```bash
   magento-cloud ssh
   ```

1. Recupere los detalles de conexión de ActiveMQ y las credenciales de inicio de sesión de la variable [$MAGENTO_CLOUD_RELATIONSHIPS](../application/properties.md#relationships):

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   o

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   En la respuesta, busque la información de ActiveMQ. El nombre de la relación depende de cómo la haya configurado en el archivo `.magento.app.yaml`. Por ejemplo, si utilizó `activemq-artemis` como nombre de relación:

   ```json
   {
      "activemq-artemis" : [
         {
            "password" : "guest",
            "ip" : "246.0.129.2",
            "scheme" : "stomp",
            "port" : 61616,
            "host" : "activemq-artemis.internal",
            "username" : "guest"
         }
      ]
   }
   ```

1. Habilite el reenvío de puertos locales a ActiveMQ (si su proyecto se encuentra en una región diferente, como US-3, EU-5 o AP-3, sustituya ``us-3``/``eu-5``/``ap-3`` por ``us``)

   ```bash
   ssh -L <port-number>:activemq-artemis.internal:<port-number> <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

   Un ejemplo de acceso a la consola web de ActiveMQ Artemis en `http://localhost:8161` es:

   ```bash
   ssh -L 8161:activemq-artemis.internal:8161 <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

   >[!NOTE]
   >
   >ActiveMQ Artemis utiliza el puerto 61616 para la mensajería STOMP y el puerto 8161 para la consola web.

1. Mientras la sesión está abierta, puede acceder a la consola web de ActiveMQ Artemis en `http://localhost:8161` con el nombre de usuario y la contraseña de la variable MAGENTO_CLOUD_RELATIONSHIPS.

### Conectar desde la aplicación

Para conectarse a ActiveMQ que se ejecuta en una aplicación, instale una biblioteca de cliente STOMP en su aplicación PHP.

### Conéctese desde su aplicación PHP

Para conectarse a ActiveMQ usando su aplicación PHP, agregue una biblioteca PHP STOMP al árbol de fuentes. Adobe Commerce utiliza el protocolo STOMP para la comunicación de ActiveMQ y la configuración se configura automáticamente durante la implementación cuando se detecta ActiveMQ Artemis como un servicio configurado.

## Compatibilidad con protocolos

Los elementos de ActiveMQ de Adobe Commerce en la infraestructura en la nube utilizan el protocolo STOMP (Protocolo de mensajería orientada a texto de streaming):

- **STOMP**: protocolo de mensajería utilizado para operaciones en cola (puerto 61616)
- **Consola web**: interfaz de administración accesible a través de HTTP (puerto 8161)

## Diferencias con respecto a RabbitMQ

Aunque [!DNL ActiveMQ Artemis] y [!DNL RabbitMQ] sirven como intermediarios de mensajes para Adobe Commerce, existen algunas diferencias:

- **Protocolo**: ActiveMQ Artemis usa el protocolo STOMP, mientras que RabbitMQ usa AMQP
- **Configuración**: cuando se configura ActiveMQ Artemis, Adobe Commerce utiliza automáticamente el protocolo STOMP
- **Prioridad**: si se configuran ActiveMQ y RabbitMQ, ActiveMQ tiene prioridad para las operaciones basadas en STOMP, mientras que las operaciones AMQP utilizan RabbitMQ
- **Consola web**: ActiveMQ proporciona una consola de administración basada en web para supervisar colas y mensajes

## Configuración de cola

Cuando ActiveMQ Artemis está configurado como servicio, Adobe Commerce configura automáticamente el sistema de cola para utilizar el protocolo STOMP. La configuración se escribe en el archivo `app/etc/env.php` durante la implementación:

```php
'queue' => [
    'stomp' => [
        'host' => 'activemq-artemis.internal',
        'port' => '61616',
        'user' => 'guest',
        'password' => 'guest'
    ],
    'default_connection' => 'stomp'
]
```

Puede anular esta configuración utilizando la variable de entorno [`QUEUE_CONFIGURATION`](../environment/variables-deploy.md#queue_configuration) si es necesario.

