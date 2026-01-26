---
title: Paquete [!DNL ECE-Tools]
description: Obtenga información acerca del paquete  [!DNL ECE-Tools] y cómo ayuda a administrar e implementar Adobe Commerce.
exl-id: 15d762ef-bca7-480b-b719-caf131dc9180
source-git-commit: db34528be490f92cc61c609ca143c01ef3284157
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 0%

---

# Paquete de herramientas ECE

El paquete [!DNL ECE-Tools] es un conjunto de scripts y herramientas diseñadas para administrar e implementar la aplicación [!DNL Commerce]. El paquete `ece-tools` simplifica muchos procesos, como la administración de trabajos cron, la verificación de la configuración del proyecto y la aplicación de revisiones y revisiones de Adobe. Puede ver y contribuir al [repositorio de código abierto [!DNL ECE-Tools] en GitHub](https://github.com/magento/ece-tools).

{{ece-tools-package}}

El paquete `ece-tools` es compatible con Adobe Commerce (a partir de la versión 2.1.4) y contiene scripts y comandos de Adobe Commerce en la infraestructura de la nube diseñados para administrar el código y generar e implementar automáticamente sus proyectos.

A continuación se enumeran los comandos `ece-tools` disponibles:

```bash
php ./vendor/bin/ece-tools list
```

## Creación e implementación

El paquete `ece-tools` contiene comandos para realizar operaciones en las fases de compilación, implementación y posterior implementación del inicio de Adobe Commerce en la aplicación de infraestructura en la nube. Por ejemplo, el comando `php ./vendor/bin/ece-tools build` inicia el proceso de generación de la aplicación.

De manera predeterminada, estos comandos de `ece-tools` se encuentran en la propiedad [hooks](../application/hooks-property.md) del archivo de configuración `.magento.app.yaml`.

## Generador de configuración de Docker

El paquete `ece-tools` incluye una dependencia para el paquete [magento/magento-cloud-docker](https://github.com/magento/magento-cloud-docker), que proporciona archivos de funcionalidad y configuración para que las imágenes de Docker inicien un entorno de desarrollo de Docker para Adobe Commerce en la infraestructura en la nube. También puede ejecutar Cloud Docker para Commerce como paquete independiente. Consulte [Desarrollo de Docker](../dev-tools/cloud-docker.md).

## Servicios, rutas y variables

Puede usar el paquete `ece-tools` para mostrar información detallada sobre las [variables de nube](../environment/variables-cloud.md) con codificación Base64 que se usan en cualquier entorno de nube. El siguiente comando muestra todos los servicios, rutas y variables.

```bash
php ./vendor/bin/ece-tools env:config:show
```

Para mostrar un conjunto específico de información, utilice el siguiente formato:

```bash
php ./vendor/bin/ece-tools env:config:show <option>
```

- `services`: muestra los datos de relación de la variable de entorno `MAGENTO_CLOUD_RELATIONSHIPS`, definida en el archivo `services.yaml`.
- `routes`: muestra las rutas configuradas para el proyecto mediante la variable de entorno `MAGENTO_CLOUD_ROUTES`.
- `variables`: muestra las variables configuradas para el proyecto mediante la variable de entorno `MAGENTO_CLOUD_VARIABLES`.

Salida de ejemplo para la opción `services`:

```
Magento Cloud Services:
+-----------------------------------+----------------------------------+
| Service Configuration             | Value                            |
+-----------------------------------+----------------------------------+
| database:                                                            |
+-----------------------------------+----------------------------------+
| host                              | 127.0.0.1                        |
| password                          | <password>                       |
| port                              | 3306                             |
+-----------------------------------+----------------------------------+
| opensearch:                                                          |
+-----------------------------------+----------------------------------+
| host                              | 127.0.0.1                        |
| port                              | 9200                             |
...
```

## Verificar configuración del entorno

Hay un conjunto de comandos de verificación disponibles para ayudar a evaluar la configuración del proyecto. Consulte [Asistentes inteligentes](../deploy/smart-wizards.md) en la sección _Optimizar implementación_ para obtener una descripción detallada de cada comando del asistente. El comando `wizard:ideal-state` se ejecuta automáticamente durante la fase de compilación. Para verificar el estado ideal del proyecto:

```bash
php ./vendor/bin/ece-tools wizard:ideal-state
```

>[!NOTE]
>
>Debe ejecutar el comando `wizard:ideal-state` en el entorno remoto de la nube. El comando siempre devuelve el error `The configured state is not ideal` cuando se ejecuta en el entorno de desarrollo local.

Salida de ejemplo:

```
Ideal state is configured
```

Ver [Notas de la versión de ece-tools](../release-notes/cloud-tools-suite.md).

## Parches de Adobe y parches personalizados

El paquete `ece-tools` incluye una dependencia para el paquete [magento/magento-cloud-patch](https://github.com/magento/magento-cloud-patches), que ofrece parches y correcciones rápidas de Adobe que mejoran la integración de todas las versiones de Adobe Commerce con entornos en la nube y admiten la entrega rápida de correcciones críticas. &quot;también ofrece parches personalizados que añade a su proyecto de infraestructura en la nube de Adobe Commerce. Ver [Aplicar parches](../development/apply-patches.md).

