---
title: Niveles variables y opciones
description: Obtenga información acerca de los diferentes niveles de variables y opciones que se utilizan para personalizar el entorno de tiempo de ejecución del proyecto de infraestructura en la nube de Adobe Commerce.
feature: Cloud, Configuration, Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 0%

---

# Niveles variables

Las variables de proyecto se aplican a todos los entornos dentro del proyecto. Las variables de entorno se aplican a un entorno o rama específicos. Un entorno _hereda_ definiciones de variables del entorno principal.

Puede anular un valor heredado definiendo la variable específicamente para el entorno. Por ejemplo, para establecer variables para el desarrollo, defina los valores de las variables en el archivo `.magento.env.yaml` en el entorno de integración. Todos los entornos que se ramifican desde el entorno de integración heredan esos valores. Consulte [Configuración de implementación](configure-env-yaml.md) para obtener detalles acerca de cómo configurar su entorno mediante el archivo `.magento.env.yaml`.

>[!BEGINTABS]

>[!TAB CLI]

**Para establecer variables mediante la CLI de nube**:

- **Variables específicas del proyecto**: para establecer el mismo valor para _todos_ los entornos de su proyecto. Estas variables están disponibles durante la compilación y el tiempo de ejecución en todos los entornos.

  ```bash
  magento-cloud variable:create --level project --name <variable-name> --value <variable-value>
  ```

- **Variables específicas del entorno**: para establecer un valor único para un entorno _específico_. Estas variables están disponibles durante la ejecución y las heredan los entornos secundarios. Especifique el entorno en el comando mediante la opción `-e`.

  ```bash
  magento-cloud variable:create --level environment --name <variable-name> --value <variable-value>
  ```

Después de establecer las variables específicas del proyecto, debe volver a implementar manualmente el entorno remoto para que el cambio surta efecto. Insertar los nuevos compromisos para almacenar en déclencheur una reimplementación.

>[!TAB Consola]

**Para establecer variables mediante[!DNL Cloud Console]**:

1. En _[!DNL Cloud Console]_, haga clic en el icono de configuración en el lado derecho de la navegación del proyecto.

   ![Configurar proyecto](../../assets/icon-configure.png){width="36"}

1. Para establecer una variable de nivel de proyecto, en _Configuración del proyecto_, haga clic en **Variables**.

   ![Variables de proyecto](../../assets/ui-project-variables.png)

1. Para establecer una variable de nivel de entorno, en la lista _Entornos_, seleccione un entorno y haga clic en la ficha **[!UICONTROL Variables]**.

   ![Pestaña Variables de entorno](../../assets/ui-environment-variables.png)

1. Haga clic en **[!UICONTROL Create variable]**.

1. Proporcione un nombre y un valor para la variable. Elija entre las siguientes opciones:

   - Disponible durante el tiempo de ejecución
   - Disponible durante la compilación
   - Valor JSON
   - Variable confidencial (valor oculto en la consola y respuestas CLI)
   - Hacer heredable (los entornos secundarios pueden heredar variables de nivel de entorno)

1. Haga clic en **[!UICONTROL Create variable]**.

>[!CAUTION]
>
>Al establecer variables específicas del entorno en [!DNL Cloud Console], se vuelve a implementar automáticamente el entorno.

>[!ENDTABS]

## Visibilidad

Puede limitar la visibilidad de una variable durante la generación o el tiempo de ejecución mediante el comando `--visible-<build|runtime>`. Además, hay opciones para establecer la herencia y la confidencialidad.

Utilice las siguientes opciones para evitar que se vea o herede una variable:

- `--inheritable false`: deshabilita la herencia para entornos secundarios. Esto resulta útil para establecer valores solo de producción en la rama `master` y permitir que todos los demás entornos utilicen una variable de nivel de proyecto del mismo nombre.
- `--sensitive true`: marca la variable como _no legible_ en [!DNL Cloud Console]. No puede ver la variable en la interfaz de usuario; sin embargo, puede verla desde el contenedor de aplicaciones, como cualquier otra variable.

A continuación se muestra un caso específico para evitar que se vea o herede una variable. Solo puede especificar estas opciones en la CLI. Este caso no se aplica a todas las variables de entorno disponibles.

```bash
magento-cloud variable:create --name <variable-name> --value <variable-value> --inheritable false --sensitive true
```

## Comprobar niveles y valores de variables

Puede ver una lista de las variables existentes mediante la CLI de nube.

```bash
magento-cloud variables
```

```
Variables on the project Project-Name (<project-id>), environment <environment-name>:
+----------------------------+-------------+-------------------------------------------+
| Name                       | Level       | Value                                     |
+----------------------------+-------------+-------------------------------------------+
| env:COMPOSER_AUTH          | project     | {                                         |
|                            |             |    "http-basic": {                        |
|                            |             |       "repo.magento.com": {               |
|                            |             |       "username":                         |
|                            |             | "<public-key>",                           |
|                            |             |       "password":                         |
|                            |             | "<private-key>"                           |
|                            |             |     }                                     |
|                            |             |   }                                       |
|                            |             | }                                         |
| ADMIN_EMAIL                | project     | admin@123.com                             |
| ADMIN_EMAIL                | environment | admin@123.com                             |
| ADMIN_PASSWORD             | environment | password                                  |
| ADMIN_URL                  | environment | admin123                                  |
| ADMIN_USERNAME             | environment | admin                                     |
| php:newrelic.license       | environment | xxxx71fb030366182117f955a22e4baf8exxxxxx  |
+----------------------------+-------------+-------------------------------------------+
```
