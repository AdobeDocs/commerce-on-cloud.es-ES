---
source-git-commit: 7f2934af84c947046fed3a32c3b6e2937aed418a
workflow-type: tm+mt
source-wordcount: '2554'
ht-degree: 4%

---
# Códigos de error ECE-Tools

<!--Note: The error code tables in this file are auto-generated from source code. To request changes to error code descriptions or suggestions, submit a GitHub issue to the magento/ece-tools repository.-->

## Errores críticos

Los errores críticos indican un problema con la configuración del proyecto de Commerce en la infraestructura en la nube que provoca un error de implementación, por ejemplo, una configuración incorrecta, no admitida o faltante para la configuración requerida. Antes de poder implementar, debe actualizar la configuración para resolver estos errores.

### Fase de compilación

| Código de error | Paso Generar | Descripción del error (título) | Acción sugerida |
| - | - | - | - |
| 2 |  | No se puede escribir en el archivo `./app/etc/env.php` | El script de implementación no puede realizar los cambios necesarios en el archivo `/app/etc/env.php`. Compruebe los permisos del sistema de archivos. |
| 3 |  | La configuración no está definida en el archivo `schema.yaml` | La configuración no está definida en el archivo `./vendor/magento/ece-tools/config/schema.yaml`. Compruebe que el nombre de la variable de configuración sea correcto y esté definido. |
| 4 |  | No se pudo analizar el archivo `.magento.env.yaml` | El formato de archivo `./.magento.env.yaml` no es válido. Utilice un analizador de YAML para comprobar la sintaxis y corregir los errores. |
| 5 |  | No se puede leer el archivo `.magento.env.yaml` | No se puede leer el archivo `./.magento.env.yaml`. Compruebe los permisos del archivo. |
| 6 |  | No se puede leer el archivo `.schema.yaml` | No se puede leer el archivo `./vendor/magento/ece-tools/config/magento.env.yaml`. Compruebe los permisos de archivo y vuelva a implementar (`magento-cloud environment:redeploy`). |
| 7 | refresh-modules | No se puede escribir en el archivo `./app/etc/config.php` | El script de implementación no puede realizar los cambios necesarios en el archivo `/app/etc/config.php`. Compruebe los permisos del sistema de archivos. |
| 8 | validate-config | No se puede leer el archivo `composer.json` | No se puede leer el archivo `./composer.json`. Compruebe los permisos del archivo. |
| 9 | validate-config | Falta la sección de carga automática necesaria en el archivo `composer.json` | Falta la sección `autoload` necesaria en el archivo `composer.json`. Compare la sección de carga automática con el archivo `composer.json` en la plantilla de Cloud y agregue la configuración que falta. |
| 10 | validate-config | El archivo `.magento.env.yaml` contiene una opción que no está declarada en el esquema o una opción configurada con un valor o fase no válidos | El archivo `./.magento.env.yaml` contiene una configuración no válida. Consulte el registro de errores para obtener información detallada. |
| 11 | refresh-modules | Error del comando: `/bin/magento module:enable --all` | Intente ejecutar `composer update` localmente. A continuación, confirme y envíe el archivo `composer.lock` actualizado. Además, compruebe `cloud.log` para obtener más información. Para obtener resultados de comandos más detallados, agregue la opción `VERBOSE_COMMANDS: '-vvv'` al archivo `.magento.env.yaml`. |
| 12 | apply-patch | Error al aplicar el parche |  |
| 13 | set-report-dir-nesting-level | No se puede escribir en el archivo `/pub/errors/local.xml` |  |
| 14 | copy-sample-data | Error al copiar los archivos de datos de ejemplo |  |
| 15 | compile-id | Error del comando: `/bin/magento setup:di:compile` | Compruebe `cloud.log` para obtener más información. Agregar `VERBOSE_COMMANDS: '-vvv'` a `.magento.env.yaml` para obtener un resultado de comando más detallado. |
| 16 | dump-autoload | Error del comando: `composer dump-autoload` | Error en el comando `composer dump-autoload`. Compruebe `cloud.log` para obtener más información. |
| 17 | empacador de carreras | Error en el comando para ejecutar `Baler` para el agrupamiento de JavaScript | Compruebe la variable de entorno `SCD_USE_BALER` para comprobar que el módulo Baler está configurado y habilitado para el agrupamiento JS. Si no necesita el módulo Baler, establezca `SCD_USE_BALER: false`. |
| 18 | compress-static-content | No se ha encontrado la utilidad requerida (timeout, bash) |  |
| 19 | deploy-static-content | Error del comando `/bin/magento setup:static-content:deploy` | Compruebe `cloud.log` para obtener más información. Para obtener resultados de comandos más detallados, agregue la opción `VERBOSE_COMMANDS: '-vvv'` al archivo `.magento.env.yaml`. |
| 20 | compress-static-content | Error de compresión de contenido estático | Compruebe `cloud.log` para obtener más información. |
| 21 | backup-data: static-content | No se pudo copiar contenido estático en el directorio `init` | Compruebe `cloud.log` para obtener más información. |
| 22 | backup-data: writable-dirs | No se pudieron copiar algunos directorios editables en el directorio `init` | No se pudieron copiar los directorios editables en la carpeta `./init`. Compruebe los permisos del sistema de archivos. |
| 23 |  | No se puede crear un objeto de registro |  |
| 24 | backup-data: static-content | No se pudo limpiar el directorio `./init/pub/static/` | No se pudo limpiar la carpeta `./init/pub/static`. Compruebe los permisos del sistema de archivos. |
| 25 |  | No se encuentra el paquete Composer | Si ha instalado la versión de la aplicación Adobe Commerce directamente desde el repositorio de GitHub, compruebe que la variable de entorno `DEPLOYED_MAGENTO_VERSION_FROM_GIT` esté configurada. |
| 26 | validate-config | Elimine la configuración del módulo Magento Braintree que ya no es compatible con Adobe Commerce y Magento Open Source 2.4 y versiones posteriores. | La compatibilidad con el módulo Braintree ya no se incluye en Magento 2.4.0 y posteriores. Quite la variable CONFIG__STORES__DEFAULT__PAYMENT__BRAINTREE__CHANNEL de la sección de variables del archivo `.magento.app.yaml`. Para la compatibilidad con pagos de Braintree, utilice una extensión oficial de Commerce Marketplace en su lugar. |

### Implementar fase

| Código de error | Paso Implementar | Descripción del error (título) | Acción sugerida |
| - | - | - | - |
| 101 | implementación previa: caché | Configuración de caché incorrecta (falta puerto o host) | En la configuración de caché faltan los parámetros necesarios `server` o `port`. Compruebe `cloud.log` para obtener más información. |
| 102 |  | No se puede escribir en el archivo `./app/etc/env.php` | El script de implementación no puede realizar los cambios necesarios en el archivo `/app/etc/env.php`. Compruebe los permisos del sistema de archivos. |
| 103 |  | La configuración no está definida en el archivo `schema.yaml` | La configuración no está definida en el archivo `./vendor/magento/ece-tools/config/schema.yaml`. Compruebe que el nombre de la variable de configuración es correcto y que está definido. |
| 104 |  | No se pudo analizar el archivo `.magento.env.yaml` | La configuración no está definida en el archivo `./vendor/magento/ece-tools/config/schema.yaml`. Compruebe que el nombre de la variable de configuración es correcto y que está definido. |
| 105 |  | No se puede leer el archivo `.magento.env.yaml` | No se puede leer el archivo `./.magento.env.yaml`. Compruebe los permisos del archivo. |
| 106 |  | No se puede leer el archivo `.schema.yaml` |  |
| 107 | implementación previa: clean-redis-cache | Error al limpiar la caché de Redis | Error al limpiar la caché de Redis. Compruebe que la configuración de la caché de Redis sea correcta y que el servicio Redis esté disponible. Consulte [Configuración del servicio Redis](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/service/redis.html). |
| 140 | implementación previa: clean-valkey-cache | Error al limpiar la caché de Valkey | Error al limpiar la caché de Valkey. Compruebe que la configuración de la caché de Valkey sea correcta y que el servicio Valkey esté disponible. Consulte [Configuración del servicio Valkey](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/service/valkey.html). |
| 108 | implementación previa: establecer modo de producción | Error del comando `/bin/magento maintenance:enable` | Compruebe `cloud.log` para obtener más información. Para obtener resultados de comandos más detallados, agregue la opción `VERBOSE_COMMANDS: '-vvv'` al archivo `.magento.env.yaml`. |
| 109 | validate-config | Configuración de base de datos incorrecta | Compruebe que la variable de entorno `DATABASE_CONFIGURATION` esté configurada correctamente. |
| 110 | validate-config | Configuración de sesión incorrecta | Compruebe que la variable de entorno `SESSION_CONFIGURATION` esté configurada correctamente. La configuración debe contener al menos el parámetro `save`. |
| 111 | validate-config | Configuración de búsqueda incorrecta | Compruebe que la variable de entorno `SEARCH_CONFIGURATION` esté configurada correctamente. La configuración debe contener al menos el parámetro `engine`. |
| 112 | validate-config | Configuración de recurso incorrecta | Compruebe que la variable de entorno `RESOURCE_CONFIGURATION` esté configurada correctamente. La configuración debe contener al menos `connection` parámetro. |
| 113 | validate-config:elasticsuite-integration | ElasticSuite está instalado, pero el servicio de Elasticsearch no está disponible | Compruebe que la variable de entorno `SEARCH_CONFIGURATION` esté configurada correctamente y que el servicio Elasticsearch esté disponible. |
| 114 | validate-config:elasticsuite-integration | ElasticSuite está instalado, pero se utiliza otro motor de búsqueda | ElasticSuite está instalado, pero hay otro motor de búsqueda configurado. Actualice la variable de entorno `SEARCH_CONFIGURATION` para habilitar Elasticsearch y compruebe la configuración del servicio Elasticsearch en el archivo `services.yaml`. |
| 115 |  | Error de ejecución de consulta de base de datos |  |
| 116 | install-update: setup | Error del comando `/bin/magento setup:install` | Compruebe `cloud.log` y `install_upgrade.log` para obtener más información. Para obtener resultados de comandos más detallados, agregue la opción `VERBOSE_COMMANDS: '-vvv'` al archivo `.magento.env.yaml`. |
| 117 | install-update: config-import | Error del comando `app:config:import` | Compruebe `cloud.log` para obtener más información. Para obtener resultados de comandos más detallados, agregue la opción `VERBOSE_COMMANDS: '-vvv'` al archivo `.magento.env.yaml`. |
| 118 |  | No se ha encontrado la utilidad requerida (timeout, bash) |  |
| 119 | install-update: deploy-static-content | Error del comando `/bin/magento setup:static-content:deploy` | Compruebe `cloud.log` para obtener más información. Para obtener resultados de comandos más detallados, agregue la opción `VERBOSE_COMMANDS: '-vvv'` al archivo `.magento.env.yaml`. |
| 120 | compress-static-content | Error de compresión de contenido estático | Compruebe `cloud.log` para obtener más información. |
| 121 | deploy-static-content:generate | No se puede actualizar la versión implementada | No se puede actualizar el archivo `./pub/static/deployed_version.txt`. Compruebe los permisos del sistema de archivos. |
| 122 | clean-static-content | Error al limpiar los archivos de contenido estático |  |
| 123 | install-update: split-db | Error del comando `/bin/magento setup:db-schema:split` | Compruebe `cloud.log` para obtener más información. Para obtener resultados de comandos más detallados, agregue la opción `VERBOSE_COMMANDS: '-vvv'` al archivo `.magento.env.yaml`. |
| 124 | clean-view-preprocessed | No se pudo limpiar la carpeta `var/view_preprocessed` | No se puede limpiar la carpeta `./var/view_preprocessed`. Compruebe los permisos del sistema de archivos. |
| 125 | install-update: reset-password | No se pudo actualizar el archivo `/var/credentials_email.txt` | No se pudo actualizar el archivo `/var/credentials_email.txt`. Compruebe los permisos del sistema de archivos. |
| 126 | install-update: update | Error del comando `/bin/magento setup:upgrade` | Compruebe `cloud.log` y `install_upgrade.log` para obtener más información. Para obtener resultados de comandos más detallados, agregue la opción `VERBOSE_COMMANDS: '-vvv'` al archivo `.magento.env.yaml`. |
| 127 | clean-cache | Error del comando `/bin/magento cache:flush` | Compruebe `cloud.log` para obtener más información. Para obtener resultados de comandos más detallados, agregue la opción `VERBOSE_COMMANDS: '-vvv'` al archivo `.magento.env.yaml`. |
| 128 | disable-maintenance-mode | Error del comando `/bin/magento maintenance:disable` | Compruebe `cloud.log` para obtener más información. Agregar `VERBOSE_COMMANDS: '-vvv'` a `.magento.env.yaml` para obtener un resultado de comando más detallado. |
| 129 | install-update: reset-password | No se puede leer la plantilla para restablecer la contraseña |  |
| 130 | install-update: cache_type | Error del comando: `php ./bin/magento cache:enable` | El comando `php ./bin/magento cache:enable` se ejecuta solamente cuando se instaló Adobe Commerce, pero el archivo `./app/etc/env.php` estaba ausente o vacío al principio de la implementación. Compruebe `cloud.log` para obtener más información. Agregar `VERBOSE_COMMANDS: '-vvv'` a `.magento.env.yaml` para obtener un resultado de comando más detallado. |
| 131 | install-update | El valor de clave `crypt/key` no existe en el archivo `./app/etc/env.php` ni en la variable de entorno de nube `CRYPT_KEY` | Este error se produce si el archivo `./app/etc/env.php` no está presente cuando comienza la implementación de Adobe Commerce o si el valor `crypt/key` no está definido. Si migró la base de datos desde otro entorno, recupere el valor de la clave de cifrado de ese entorno. A continuación, agregue el valor a la variable de entorno de nube [CRYPT_KEY](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-deploy.html#crypt_key) de su entorno actual. Consulte [Clave de cifrado de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/develop/overview.html#gather-credentials). Si quitó accidentalmente el archivo `./app/etc/env.php`, use el siguiente comando para restaurarlo a partir de los archivos de copia de seguridad creados a partir de una implementación anterior: `./vendor/bin/ece-tools backup:restore` comando CLI.&quot; |
| 132 |  | No se puede conectar al servicio Elasticsearch | Compruebe si hay credenciales de Elasticsearch válidas y que el servicio se esté ejecutando |
| 137 |  | No se puede conectar al servicio OpenSearch | Compruebe si hay credenciales de OpenSearch válidas y que el servicio se esté ejecutando |
| 133 | validate-config | Elimine la configuración del módulo Magento Braintree que ya no sea compatible con Adobe Commerce o Magento Open Source 2.4 y versiones posteriores. | La compatibilidad con el módulo Braintree ya no se incluye con Adobe Commerce o Magento Open Source 2.4.0 y posteriores. Quite la variable CONFIG__STORES__DEFAULT__PAYMENT__BRAINTREE__CHANNEL de la sección de variables del archivo `.magento.app.yaml`. Para obtener asistencia de Braintree, utilice una extensión oficial de Braintree Payments de Commerce Marketplace. |
| 134 | validate-config | Adobe Commerce y Magento Open Source 2.4.0 requieren que esté instalado el servicio Elasticsearch | Instalar el servicio Elasticsearch |
| 138 | validate-config | Adobe Commerce y Magento Open Source 2.4.4 requieren que esté instalado el servicio OpenSearch o Elasticsearch | Instalar el servicio OpenSearch |
| 135 | validate-config | El motor de búsqueda debe configurarse como Elasticsearch para Adobe Commerce y Magento Open Source >= 2.4.0 | Compruebe la variable SEARCH_CONFIGURATION para la opción `engine`. Si está configurada, elimine la opción o establezca el valor en &quot;elasticsearch&quot;. |
| 136 | validate-config | La base de datos dividida se eliminó a partir de Adobe Commerce y Magento Open Source 2.5.0. | Si utiliza una base de datos dividida, debe revertirla a una única base de datos o migrarla a ella, o utilizar un método alternativo. |
| 139 | validate-config | Motor de búsqueda incorrecto | Esta versión de Adobe Commerce o Magento Open Source no admite OpenSearch. Utilice las versiones 2.3.7-p3, 2.4.3-p2 o posterior |

### Fase posterior a la implementación

| Código de error | Paso posterior a la implementación | Descripción del error (título) | Acción sugerida |
| - | - | - | - |
| 201 | is-deploy-failed | Error de implementación de fase |  |
| 202 |  | No se puede escribir en el archivo `./app/etc/env.php` | El script de implementación no puede realizar los cambios necesarios en el archivo `/app/etc/env.php`. Compruebe los permisos del sistema de archivos. |
| 203 |  | La configuración no está definida en el archivo `schema.yaml` | La configuración no está definida en el archivo `./vendor/magento/ece-tools/config/schema.yaml`. Compruebe que el nombre de la variable de configuración es correcto y que está definido. |
| 204 |  | No se pudo analizar el archivo `.magento.env.yaml` | El formato de archivo `./.magento.env.yaml` no es válido. Utilice un analizador de YAML para comprobar la sintaxis y corregir los errores. |
| 205 |  | No se puede leer el archivo `.magento.env.yaml` | Compruebe los permisos del archivo. |
| 206 |  | No se puede leer el archivo `.schema.yaml` |  |
| 207 | calentamiento | Error al cargar previamente algunas páginas de calentamiento |  |
| 208 | tiempo hasta el primer byte | Error al probar el tiempo hasta el primer byte (TTFB) |  |
| 227 | clean-cache | Error del comando `/bin/magento cache:flush` | Compruebe `cloud.log` para obtener más información. Agregar `VERBOSE_COMMANDS: '-vvv'` a `.magento.env.yaml` para obtener un resultado de comando más detallado. |

### General

| Código de error | Paso general | Descripción del error (título) | Acción sugerida |
| - | - | - | - |
| 243 |  | La configuración no está definida en el archivo `schema.yaml` | Compruebe que el nombre de la variable de configuración es correcto y que está definido. |
| 244 |  | No se pudo analizar el archivo `.magento.env.yaml` | El formato de archivo `./.magento.env.yaml` no es válido. Utilice un analizador de YAML para comprobar la sintaxis y corregir los errores. |
| 245 |  | No se puede leer el archivo `.magento.env.yaml` | No se puede leer el archivo `./.magento.env.yaml`. Compruebe los permisos del archivo. |
| 246 |  | No se puede leer el archivo `.schema.yaml` |  |
| 247 |  | No se puede generar un módulo para eventos | Compruebe `cloud.log` para obtener más información. |
| 248 |  | No se puede habilitar un módulo para eventos | Compruebe `cloud.log` para obtener más información. |
| 249 |  | Error al generar el módulo AdobeCommerceWebhookPlugins | Compruebe `cloud.log` para obtener más información. |
| 250 |  | Error al habilitar el módulo AdobeCommerceWebhookPlugins | Compruebe `cloud.log` para obtener más información. |

## Errores de advertencia

Los errores de advertencia indican un problema con la configuración del proyecto de Commerce en la infraestructura en la nube, como ajustes de configuración incorrectos, obsoletos, no compatibles o faltantes para funciones opcionales que pueden afectar al funcionamiento del sitio. Aunque una advertencia no provoca un error de implementación, debe revisar los mensajes de advertencia y actualizar la configuración para resolverlos.

### Fase de compilación

| Código de error | Paso Generar | Descripción del error (título) | Acción sugerida |
| - | - | - | - |
| 1001 | validate-config | El archivo app/etc/config.php no existe |  |
| 1002 | validate-config | El .El archivo /build_options.ini ya no es compatible |  |
| 1003 | validate-config | Falta la sección de módulos en el archivo de configuración compartido |  |
| 1004 | validate-config | La configuración no es compatible con esta versión de Magento |  |
| 1005 | validate-config | Opciones de SCD ignoradas |  |
| 1006 | validate-config | El estado configurado no es ideal |  |
| 1007 | empacador de carreras | No se puede utilizar el paquete JS de Baler |  |

### Implementar fase

| Código de error | Paso Implementar | Descripción del error (título) | Acción sugerida |
| - | - | - | - |
| 2001 | implementación previa:caché | La caché está configurada para un servicio Redis que no está disponible. Se ignora la configuración. |  |
| 2032 | implementación previa:caché | La caché está configurada para un servicio de Valkey que no está disponible. Se ignora la configuración. |  |
| 2002 | validate-config | El estado configurado no es ideal |  |
| 2003 | validate-config | No se ha configurado el valor de nivel de anidamiento de directorio para los informes de errores |  |
| 2004 | validate-config | Configuración no válida en .Archivo /pub/errors/local.xml. |  |
| 2005 | validate-config | Los datos de administración se utilizan para crear un usuario administrador solo durante la instalación inicial. Los cambios realizados en los datos de administración se omiten durante el proceso de actualización. | Después de la instalación inicial, puede quitar los datos de administración de la configuración. |
| 2006 | validate-config | No se ha creado el usuario administrador porque no se ha definido un correo electrónico de administrador | Después de la instalación, puede crear un usuario administrador manualmente: utilice ssh para conectarse a su entorno. A continuación, ejecute el comando `bin/magento admin:user:create`. |
| 2007 | validate-config | Actualizar versión de php a la versión recomendada |  |
| 2008 | validate-config | La compatibilidad con Solr ha quedado obsoleta en Adobe Commerce y Magento Open Source 2.1. |  |
| 2009 | validate-config | Adobe Commerce y Magento Open Source 2.2 o posterior ya no admiten Solr. |  |
| 2010 | validate-config | El servicio Elasticsearch está instalado en el nivel de infraestructura, pero no se utiliza como motor de búsqueda. | Considere la posibilidad de eliminar el servicio de Elasticsearch de la capa de infraestructura para optimizar el uso de los recursos. |
| 2011 | validate-config | La versión del servicio de Elasticsearch en el nivel de infraestructura no es compatible con la versión actual del módulo elasticsearch/elasticsearch, que utiliza la aplicación de Adobe Commerce. |  |
| 2012 | validate-config | La configuración actual no es compatible con esta versión de Adobe Commerce |  |
| 2013 | validate-config | Opciones de SCD ignoradas porque el proceso de implementación no se ejecutó en la fase de compilación |  |
| 2014 | validate-config | La configuración contiene variables o valores obsoletos |  |
| 2015 | validate-config | La configuración del entorno no es válida |  |
| 2016 | validate-config | No se puede descodificar la configuración de tipo JSON |  |
| 2017 | validate-config | La configuración actual no es compatible con esta versión de Adobe Commerce |  |
| 2018 | validate-config | Algunos servicios han superado el límite de vida |  |
| 2019 | validate-config | La opción de configuración de búsqueda MySQL está obsoleta | Utilice Elasticsearch en su lugar. |
| 2029 | validate-config | La base de datos dividida estaba obsoleta en Adobe Commerce y Magento Open Source 2.4.2 y se eliminará en 2.5. | Si utiliza una base de datos dividida, debe empezar a planificar la reversión a una base de datos única o la migración a ella, o bien utilizar un método alternativo. |
| 2020 | install-update | Se completó la instalación de Adobe Commerce, pero faltaba el archivo de configuración `app/etc/env.php` o estaba vacío. | Los datos necesarios se restauran desde las configuraciones de entorno y desde el archivo .magento.env.yaml. |
| 2021 | install-update:conexión-db | Para bases de datos divididas que utilizan conexiones personalizadas |  |
| 2022 | install-update:conexión-db | Ha cambiado a una configuración de base de datos que no es compatible con la conexión esclava. |  |
| 2023 | install-update:split-db | Se omite la activación de una base de datos dividida. |  |
| 2024 | install-update:split-db | Falta la configuración de la variable SPLIT_DB para los tipos de conexión dividida. |  |
| 2025 | install-update:split-db | Conexión esclava no establecida. |  |
| 2026 | implementación previa:restore-writable-dirs | No se pudieron restaurar algunos datos generados durante la fase de compilación en los directorios montados | Compruebe `cloud.log` para obtener más información. |
| 2027 | validate-config:image-mode-variable | No se admite el valor de modo para la variable de entorno MAGE_MODE | Elimine la variable de entorno MAGE_MODE o cambie su valor a &quot;producción&quot;. Adobe Commerce en la infraestructura en la nube solo admite el modo &quot;producción&quot;. |
| 2028 | almacenamiento remoto | No se pudo habilitar el almacenamiento remoto. | Compruebe las credenciales de almacenamiento remoto. |
| 2030 | validate-config | Los servicios Elasticsearch y OpenSearch se instalan en el nivel de infraestructura. Adobe Commerce y Magento Open Source 2.4.4 y posteriores utilizan OpenSearch de forma predeterminada | Considere la posibilidad de eliminar el servicio Elasticsearch u OpenSearch de la capa de infraestructura para optimizar el uso de los recursos. |

### Fase posterior a la implementación

| Código de error | Paso posterior a la implementación | Descripción del error (título) | Acción sugerida |
| - | - | - | - |
| 3001 | validate-config | El registro de depuración está habilitado en Adobe Commerce | Para ahorrar espacio en disco, no habilite el registro de depuración para los entornos de producción. |
| 3002 | calentamiento | No se pueden recuperar las URL del almacén |  |
| 3003 | calentamiento | No se puede obtener la URL del almacén |  |
| 3004 | reserva | No se pueden crear archivos de copia de seguridad |  |

### General

| Código de error | Paso general | Descripción del error (título) | Acción sugerida |
| - | - | - | - |
| 4001 |  | No se puede obtener el recuento del procesador del sistema: |  |
