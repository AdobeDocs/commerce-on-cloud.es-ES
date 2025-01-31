---
title: Controladores de registro
description: Obtenga información sobre cómo configurar los controladores de registro para Adobe Commerce en la infraestructura en la nube.
feature: Cloud, Logs, Configuration
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 0%

---

# Controladores de registro

Puede configurar los controladores de registro para enviar mensajes a un servidor de registro remoto. Un controlador de registro inserta registros de compilación e implementación en otros sistemas, de forma similar a la forma de insertar registros en Slack y correo electrónico. Puede habilitar un controlador _syslog_, que es ideal para registrar mensajes relacionados con el hardware, o un controlador de Formato de registro extendido (GELF) de Graylog, que es ideal para registrar mensajes de aplicaciones de software.

El ejemplo siguiente configura ambos controladores agregando la configuración al archivo `.magento.env.yaml`. Para ver los valores mínimos del nivel de registro (`min_level`), vea [Niveles de registro](#log-levels).

```yaml
log:
  syslog:
    ident: "<syslog-ident>"
    facility: 8 # https://php.net/manual/en/network.constants.php
    min_level: "info"
    logopts: <syslog-logopts>

  syslog_udp:
    host: "<syslog-host>"
    port: <syslog-port>
    facility: 8  # https://php.net/manual/en/network.constants.php
    ident: "<syslog-ident>"
    min_level: "info"

  gelf:
    min_level: "info"
    use_default_formatter: true
    additional: # Some additional information for each log message
      project: "<some-project-id>"
      app_id: "<some-app-id>"
    transport:
      http:
        host: "<http-host>"
        port: <http-port>
        path: "<http-path>"
        connection_timeout: 60
      tcp:
        host: "<tcp-host>"
        port: <tcp-port>
        connection_timeout: 60
      udp:
        host: "<udp-host>"
        port: <udp-port>
        chunk_size: 1024
```

## Niveles de registro

Los niveles de registro determinan el nivel de detalle de los mensajes de notificación. Las siguientes categorías de nivel de registro incluyen todos los niveles de registro por debajo de él. Por ejemplo, un nivel de `debug` incluye el registro de todos los niveles, mientras que un nivel de `alert` solo muestra alertas y emergencias.

- **debug**: información de depuración detallada
- **info**: eventos interesantes, como un inicio de sesión de usuario o un registro SQL
- **aviso**: eventos normales pero significativos
- **advertencia**: ocurrencias excepcionales que no son errores, como el uso de una API obsoleta o el uso incorrecto de una API
- **error**: errores en tiempo de ejecución que no requieren una acción inmediata
- **crítico**: condiciones críticas, como un componente de aplicación no disponible o una excepción inesperada
- **alerta** (se requiere una acción inmediata, como un sitio web inactivo o una base de datos no disponible) que déclencheur una alerta SMS
- **emergencia**—el sistema no se puede utilizar
