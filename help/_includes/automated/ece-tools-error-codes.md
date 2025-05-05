---
source-git-commit: 350cfea06f036f0787b330e6e40c5af46a30f5ad
workflow-type: tm+mt
source-wordcount: '2554'
ht-degree: 4%

---
# Códigos de error ECE-Herramientas

<!--Note: The error code tables in this file are auto-generated from source code. To request changes to error code descriptions or suggestions, submit a GitHub issue to the magento/ece-tools repository.-->

## Errores críticos

Los errores críticos indican un problema con la configuración de Commerce on infraestructura en la nube proyecto que causa un error implementación, por ejemplo, configuración incorrecta, no compatible o faltante para los ajustes requeridos. Antes de poder implementar, debe actualizar la configuración para resolver estos errores.

### Generar fase

| Error código | Paso de compilación | Descripción del error (título) | Acción sugerida |
| - | - | - | - |
| 2 |  | No se puede escribir en el archivo `./app/etc/env.php` | El script de implementación no puede realizar los cambios necesarios en el archivo `/app/etc/env.php`. Compruebe los permisos del sistema de archivos. |
| 3 |  | La configuración no está definida en el `schema.yaml` archivo | La configuración no está definida en el `./vendor/magento/ece-tools/config/schema.yaml` archivo. Compruebe que el nombre del variable configuración sea correcto y esté definido. |
| 4 |  | No se pudo analizar el archivo `.magento.env.yaml` | La `./.magento.env.yaml` formato del archivo está no válido. Utilice un analizador YAML para comprobar la sintaxis y corregir cualquier error. |
| 5 |  | No se puede leer el `.magento.env.yaml` archivo | No se puede leer el `./.magento.env.yaml` archivo. Compruebe los permisos del archivo. |
| 6 |  | No se puede leer el `.schema.yaml` archivo | No se puede leer el `./vendor/magento/ece-tools/config/magento.env.yaml` archivo. Compruebe los permisos de los archivos y vuelva a implementar (`magento-cloud environment:redeploy`). |
| 7 | refresh-modules | No se puede escribir en el archivo `./app/etc/config.php` | El script de implementación no puede realizar los cambios necesarios en el archivo `/app/etc/config.php`. Compruebe los permisos del sistema de archivos. |
| 8 | validate-config | No se puede leer el archivo `composer.json` | No se puede leer el archivo `./composer.json`. Compruebe los permisos del archivo. |
| 9 | validate-config | Falta la sección de carga automática necesaria en el archivo `composer.json` | Falta la sección `autoload` necesaria en el archivo `composer.json`. Compare la sección de carga automática con el archivo `composer.json` en la plantilla de Cloud y agregue la configuración que falta. |
| 10 | validate-config | El `.magento.env.yaml` archivo contiene una opción que no está declarada en el esquema o una opción configurada con un valor no válido o fase | El `./.magento.env.yaml` archivo contiene no válido configuración. Consulte el registro de errores para obtener información detallada. |
| 11 | refresh-modules | Error del comando: `/bin/magento module:enable --all` | Intente ejecutar `composer update` localmente. A continuación, confirme y envíe el archivo `composer.lock` actualizado. Además, compruebe `cloud.log` para obtener más información. Para obtener resultados de comandos más detallados, agregue la opción `VERBOSE_COMMANDS: '-vvv'` al archivo `.magento.env.yaml`. |
| 12 | apply-patch | No se ha podido aplicar el parche |  |
| 13 | set-report-dir-nesting-level | No se puede escribir en el archivo `/pub/errors/local.xml` |  |
| 14 | copiar datos de muestra | Error al copiar archivos de datos de muestra |  |
| 15 | compile-id | Comando falló: `/bin/magento setup:di:compile` | Compruebe `cloud.log` para obtener más información. `VERBOSE_COMMANDS: '-vvv'` añadir en `.magento.env.yaml` para obtener una salida de comandos más detallada. |
| 16 | volcado-carga automática | Comando falló: `composer dump-autoload` | Error en el `composer dump-autoload` comando. Compruebe `cloud.log` para obtener más información. |
| 17 | empacador de carreras | Error en el comando para ejecutar `Baler` para el agrupamiento de JavaScript | Compruebe la variable de entorno `SCD_USE_BALER` para comprobar que el módulo Baler está configurado y habilitado para el agrupamiento JS. Si no necesita el módulo Baler, establezca `SCD_USE_BALER: false`. |
| 18 | compress-static-contenido | No se encontró la utilidad requerida (tiempo de espera, bash) |  |
| 19 | deploy-static-content | Error del comando `/bin/magento setup:static-content:deploy` | Consulte el `cloud.log` para obtener más información. Para obtener una salida de comandos más detallada, agregue la `VERBOSE_COMMANDS: '-vvv'` opción al `.magento.env.yaml` archivo. |
| 20 | compress-static-contenido | Error al comprimir contenido estática | Compruebe `cloud.log` para obtener más información. |
| 21 | copia de seguridad-data: static-contenido | Error al copiar el contenido estático en el `init` directorio | Consulte el `cloud.log` para obtener más información. |
| 22 | backup-data: writable-dirs | No se pudieron copiar algunos directorios editables en el directorio `init` | No se pudieron copiar los directorios editables en la carpeta `./init`. Compruebe los permisos del sistema de archivos. |
| 23 |  | No se puede crear un objeto de registro |  |
| 24 | backup-data: static-content | No se pudo limpiar el directorio `./init/pub/static/` | No se pudo limpiar la carpeta `./init/pub/static`. Compruebe los permisos del sistema de archivos. |
| 25 |  | No se encuentra el paquete Composer | Si ha instalado la versión de la aplicación Adobe Commerce directamente desde el repositorio de GitHub, compruebe que la variable de entorno `DEPLOYED_MAGENTO_VERSION_FROM_GIT` esté configurada. |
| 26 | validate-config | Elimine la configuración del módulo Magento Braintree que ya no es compatible con Adobe Commerce y Magento Open Source 2.4 y versiones posteriores. | La compatibilidad con el módulo Braintree ya no se incluye en Magento 2.4.0 y posteriores. Quitar el variable de CONFIG__STORES__DEFAULT__PAYMENT__BRAINTREE__CHANNEL desde la sección de variables del `.magento.app.yaml` archivo. Para Braintree soporte de pago, utilice una extensión oficial de la Commerce Marketplace en su lugar. |

### Implementar fase

| Código de error | Paso Implementar | Descripción del error (título) | Acción sugerida |
| - | - | - | - |
| 101 | Pre-implementar: caché | Configuración de caché incorrecta (falta puerto o host) | Falta la configuración de caché o `port`los parámetros `server` necesarios. Consulte el `cloud.log` para obtener más información. |
| 102 |  | No se puede escribir en el `./app/etc/env.php` archivo | El script de implementación no puede realizar los cambios necesarios en el `/app/etc/env.php` archivo. Compruebe los permisos del sistema de archivos. |
| 103 |  | La configuración no está definida en el `schema.yaml` archivo | La configuración no está definida en el archivo `./vendor/magento/ece-tools/config/schema.yaml`. Compruebe que el nombre de la variable de configuración es correcto y que está definido. |
| 104 |  | Error al analizar el `.magento.env.yaml` archivo | La configuración no está definida en el `./vendor/magento/ece-tools/config/schema.yaml` archivo. Compruebe que el nombre del variable configuración sea correcto y que esté definido. |
| 105 |  | No se puede leer el archivo `.magento.env.yaml` | No se puede leer el archivo `./.magento.env.yaml`. Compruebe los permisos del archivo. |
| 106 |  | No se puede leer el archivo `.schema.yaml` |  |
| 107 | Pre-implementar: clean-redis-cache | Error al limpiar la caché de Redis | Error al limpiar la caché de Redis. Compruebe que la configuración de caché de Redis es correcta y que el servicio de Redis está disponible. Consulte [Configuración del servicio Redis](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/configure/service/redis). |
| 140 | pre-implementar: clean-valkey-cache | Error al limpiar la caché de Valkey | Error al limpiar la caché de Valkey. Compruebe que la configuración de la caché de Valkey es correcta y que el servicio Valkey está disponible. Consulte [Configuración del servicio](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/configure/service/valkey) Valkey. |
| 108 | implementación previa: establecer modo de producción | `/bin/magento maintenance:enable` Error Comando | Consulte el `cloud.log` para obtener más información. Para obtener una salida de comandos más detallada, agregue la `VERBOSE_COMMANDS: '-vvv'` opción al `.magento.env.yaml` archivo. |
| 109 | validate-config | Configuración incorrecta de la base de datos | Compruebe que la `DATABASE_CONFIGURATION` variable entorno esté configurada correctamente. |
| 110 | validate-config | Configuración de sesión incorrecta | Compruebe que la `SESSION_CONFIGURATION` variable entorno esté configurada correctamente. La configuración debe contener al menos el `save` parámetro. |
| 111 | validate-config | Configuración de búsqueda incorrecta | Compruebe que la variable de entorno `SEARCH_CONFIGURATION` esté configurada correctamente. La configuración debe contener al menos el parámetro `engine`. |
| 112 | validate-config | Configuración de recurso incorrecta | Compruebe que la variable de entorno `RESOURCE_CONFIGURATION` esté configurada correctamente. La configuración debe contener al menos `connection` parámetro. |
| 113 | validate-config:elasticsuite-integration | ElasticSuite está instalado, pero el servicio ElasticSearch no está disponible | Compruebe que la variable de entorno `SEARCH_CONFIGURATION` esté configurada correctamente y que el servicio Elasticsearch esté disponible. |
| 114 | validate-config:elasticsuite-integration | ElasticSuite está instalado, pero se utiliza otro motor de búsqueda | ElasticSuite está instalado, pero se configura otro motor de búsqueda. Actualice el variable entorno para habilitar Elasticsearch `SEARCH_CONFIGURATION` y comprobar la configuración del servicio de Elasticsearch en el `services.yaml` archivo. |
| 115 |  | Error en la ejecución del consulta de la base de datos |  |
| 116 | install-update: programa de instalación | `/bin/magento setup:install` Error Comando | Consulte el `cloud.log` y `install_upgrade.log` para obtener más información. Para obtener una salida de comandos más detallada, agregue la `VERBOSE_COMMANDS: '-vvv'` opción al `.magento.env.yaml` archivo. |
| 117 | install-update: config-import | `app:config:import` Error Comando | Compruebe `cloud.log` para obtener más información. Para obtener resultados de comandos más detallados, agregue la opción `VERBOSE_COMMANDS: '-vvv'` al archivo `.magento.env.yaml`. |
| 118 |  | No se encontró la utilidad requerida (tiempo de espera, bash) |  |
| 119 | install-update: implementar-static-contenido | Error del comando `/bin/magento setup:static-content:deploy` | Compruebe `cloud.log` para obtener más información. Para obtener resultados de comandos más detallados, agregue la opción `VERBOSE_COMMANDS: '-vvv'` al archivo `.magento.env.yaml`. |
| 120 | compress-static-content | Error de compresión de contenido estático | Consulte el `cloud.log` para obtener más información. |
| 121 | implementar-static-contenido:generate | No se puede actualizar la versión implementada | No se puede actualizar el `./pub/static/deployed_version.txt` archivo. Compruebe los permisos del sistema de archivos. |
| 122 | clean-static-contenido | No se han podido limpiar los archivos de contenido estáticos |  |
| 123 | install-update: split-db | `/bin/magento setup:db-schema:split` Error Comando | Consulte el `cloud.log` para obtener más información. Para obtener una salida de comandos más detallada, agregue la `VERBOSE_COMMANDS: '-vvv'` opción al `.magento.env.yaml` archivo. |
| 124 | limpiar vista preprocesado | Error al limpiar la `var/view_preprocessed` carpeta | No se puede limpiar la `./var/view_preprocessed` carpeta. Compruebe los permisos del sistema de archivos. |
| 125 | install-update: reset-contraseña | Error al actualizar el `/var/credentials_email.txt` archivo | Error al actualizar el `/var/credentials_email.txt` archivo. Compruebe los permisos del sistema de archivos. |
| 126 | install-update: update | Error del comando `/bin/magento setup:upgrade` | Compruebe `cloud.log` y `install_upgrade.log` para obtener más información. Para obtener resultados de comandos más detallados, agregue la opción `VERBOSE_COMMANDS: '-vvv'` al archivo `.magento.env.yaml`. |
| 127 | clean-cache | Error del comando `/bin/magento cache:flush` | Compruebe `cloud.log` para obtener más información. Para obtener resultados de comandos más detallados, agregue la opción `VERBOSE_COMMANDS: '-vvv'` al archivo `.magento.env.yaml`. |
| 128 | disable-maintenance-mode | `/bin/magento maintenance:disable` Error Comando | Compruebe `cloud.log` para obtener más información. Agregar `VERBOSE_COMMANDS: '-vvv'` a `.magento.env.yaml` para obtener un resultado de comando más detallado. |
| 129 | install-update: reset-password | No se puede leer la plantilla para restablecer la contraseña |  |
| 130 | install-update: cache_type | Comando falló: `php ./bin/magento cache:enable` | `php ./bin/magento cache:enable` Comando ejecuta solo cuando se instaló Adobe Systems Commerce, pero `./app/etc/env.php` el archivo estaba ausente o vacío al principio del implementación. Consulte el `cloud.log` para obtener más información. `VERBOSE_COMMANDS: '-vvv'` añadir en `.magento.env.yaml` para obtener una salida de comandos más detallada. |
| 131 | install-update | El `crypt/key`  valor clave no existe en el `./app/etc/env.php` archivo ni el `CRYPT_KEY` nube entorno variable | Este error se produce si el `./app/etc/env.php` archivo no está presente cuando comienza Adobe Systems implementación Commerce o si el `crypt/key` valor no está definido. Si migró la base de datos desde otra entorno, recupere el valor de la clave de cripta de esa entorno. A continuación, agregue el valor al [CRYPT_KEY](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/configure/env/stage/variables-deploy#crypt_key) nube entorno variable de su entorno actual. Consulte [Adobe Systems Clave](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/develop/overview#gather-credentials) de cifrado de Commerce. Si eliminó accidentalmente el `./app/etc/env.php` archivo, use el siguiente comando para restaurarlo desde los archivos de copia de seguridad creados a partir de un comando implementación anterior: `./vendor/bin/ece-tools backup:restore` CLI. |
| 132 |  | No se puede conectar con el servicio de Elasticsearch | Compruebe que las credenciales de Elasticsearch son válidas y compruebe que el servicio se está ejecutando. |
| 137 |  | No se puede conectar al servicio OpenSearch | Compruebe que las credenciales válidas de OpenSearch y compruebe que el servicio se está ejecutando. |
| 133 | validate-config | Elimine la configuración del módulo Magento Braintree que ya no sea compatible con Adobe Commerce o Magento Open Source 2.4 y versiones posteriores. | La compatibilidad con el módulo Braintree ya no se incluye con Adobe Commerce o Magento Open Source 2.4.0 y posteriores. Quite la variable CONFIG__STORES__DEFAULT__PAYMENT__BRAINTREE__CHANNEL de la sección de variables del archivo `.magento.app.yaml`. Para obtener asistencia de Braintree, utilice una extensión oficial de Braintree Payments de Commerce Marketplace. |
| 134 | validate-config | Adobe Systems Commerce y Magento Open Source 2.4.0 requieren que se instale Elasticsearch servicio | Instalación Elasticsearch servicio |
| 138 | validate-config | Adobe Systems Commerce and Magento Open Source 2.4.4 requiere que se instale OpenSearch o Elasticsearch service | Instalar el servicio OpenSearch |
| 135 | validate-config | El motor de búsqueda debe establecerse en Elasticsearch para Adobe Systems Commerce y Magento Open Source >= 2.4.0 | Marque la variable SEARCH_CONFIGURATION de la `engine` opción. Si está configurado, elimina la opción o establece el valor en &quot;elasticsearch&quot;. |
| 136 | validate-config | La base de datos dividida se eliminó a partir de Adobe Systems Commerce y Magento Open Source 2.5.0. | Si utiliza una base de datos dividida, debe volver o migrar a una sola base de datos o utilizar un enfoque alternativo. |
| 139 | validate-config | motor de búsqueda incorrecto | Esta versión Adobe Systems Commerce o Magento Open Source no admite OpenSearch. Utilizar las versiones 2.3.7-p3, 2.4.3-p2 o posterior |

### Post-implementar fase

| Código de error | Paso posterior a la implementación | Descripción del error (título) | Acción sugerida |
| - | - | - | - |
| 201 | is-deploy-failed | Error de implementación de fase |  |
| 202 |  | No se puede escribir en el archivo `./app/etc/env.php` | El script de implementación no puede realizar los cambios necesarios en el archivo `/app/etc/env.php`. Compruebe los permisos del sistema de archivos. |
| 203 |  | La configuración no está definida en el `schema.yaml` archivo | La configuración no está definida en el archivo `./vendor/magento/ece-tools/config/schema.yaml`. Compruebe que el nombre de la variable de configuración es correcto y que está definido. |
| 204 |  | No se pudo analizar el archivo `.magento.env.yaml` | El formato de archivo `./.magento.env.yaml` no es válido. Utilice un analizador YAML para comprobar la sintaxis y corregir cualquier error. |
| 205 |  | No se puede leer el `.magento.env.yaml` archivo | Compruebe los permisos del archivo. |
| 206 |  | No se puede leer el archivo `.schema.yaml` |  |
| 207 | calentamiento | Error al cargar previamente algunas páginas de calentamiento |  |
| 208 | tiempo hasta el primer byte | Error al probar el tiempo hasta el primer byte (TTFB) |  |
| 227 | Limpiar caché | `/bin/magento cache:flush` Error Comando | Consulte el `cloud.log` para obtener más información. `VERBOSE_COMMANDS: '-vvv'` añadir en `.magento.env.yaml` para obtener una salida de comandos más detallada. |

### General

| Error código | Paso general | Descripción del error (título) | Acción sugerida |
| - | - | - | - |
| 243 |  | La configuración no está definida en el archivo `schema.yaml` | Compruebe que el nombre del variable configuración sea correcto y que se haya definido. |
| 244 |  | Error al analizar el `.magento.env.yaml` archivo | La `./.magento.env.yaml` formato del archivo está no válido. Utilice un analizador YAML para comprobar la sintaxis y corregir cualquier error. |
| 245 |  | No se puede leer el archivo `.magento.env.yaml` | No se puede leer el archivo `./.magento.env.yaml`. Compruebe los permisos de los archivos. |
| 246 |  | No se puede leer el `.schema.yaml` archivo |  |
| 247 |  | No se puede generar un módulo para los eventos | Consulte el `cloud.log` para obtener más información. |
| 248 |  | No se puede habilitar un módulo para eventos | Consulte el `cloud.log` para obtener más información. |
| 249 |  | Error al generar la módulo AdobeCommerceWebhookPlugins | Consulte el `cloud.log` para obtener más información. |
| 250 |  | Error al habilitar el módulo AdobeCommerceWebhookPlugins | Compruebe `cloud.log` para obtener más información. |

## Errores de advertencia

Los errores de advertencia indican un problema con la configuración del proyecto de Commerce en la infraestructura en la nube, como ajustes de configuración incorrectos, obsoletos, no compatibles o faltantes para funciones opcionales que pueden afectar al funcionamiento del sitio. Aunque una advertencia no provoca un error de implementación, debe revisar los mensajes de advertencia y actualizar la configuración para resolverlos.

### Fase de compilación

| Código de error | Paso Generar | Descripción del error (título) | Acción sugerida |
| - | - | - | - |
| 1001 | validate-config | Archivo aplicación/etc/config.php no existe |  |
| 1002 | validate-config | El.El archivo /versión_options.ini ya no es compatible |  |
| 1003 | validate-config | Falta la sección de módulos del archivo de configuración compartido |  |
| 1004 | validate-config | La configuración no es compatible con esta versión de Magento |  |
| 1005 | validate-config | Opciones de SCD ignoradas |  |
| 1006 | validate-config | El estado configurado no es ideal |  |
| 1007 | empacador de carreras | No se puede utilizar el empaquetado de Baler JS |  |

### Implementar fase

| Error código | Paso de implementación | Descripción Error (Título) | Acción sugerida |
| - | - | - | - |
| 2001 | implementación previa:caché | La caché está configurada para un servicio Redis que no está disponible. Se ignora la configuración. |  |
| 2032 | pre-implementar:caché | La caché está configurada para un servicio Valkey que no está disponible. Se ignora la configuración. |  |
| 2002 | validate-config | El estado configurado no es ideal |  |
| 2003 | validate-config | No se ha configurado el valor del nivel de anidación de directorios para el error sistema de informes |  |
| 2004 | validate-config | Configuración no válida en el ./pub/errors/local.xml archivo. |  |
| 2005 | validate-config | Los datos de administrador se utilizan para crear un usuario de administrador únicamente durante la instalación inicial. Los cambios realizados en los datos de administración se omiten durante el proceso de actualización. | Después de la instalación inicial, puede eliminar los datos de administración de la configuración. |
| 2006 | validate-config | No se creó el usuario de administración como administrador correo electrónico no se configuró | Después de la instalación, puede crear un usuario de administrador manualmente: Use ssh para conectarse a su entorno. A continuación, ejecute el `bin/magento admin:user:create` comando. |
| 2007 | validate-config | Actualizar la versión php a la versión recomendada |  |
| 2008 | validate-config | El soporte de Solr ha quedado obsoleto en Adobe Systems Commerce y Magento Open Source 2.1. |  |
| 2009 | validate-config | Solr ya no es compatible con Adobe Systems Commerce y Magento Open Source 2.2 o posterior. |  |
| 2010 | validate-config | El servicio Elasticsearch está instalado en el nivel de infraestructura, pero no se utiliza como motor de búsqueda. | Considere la posibilidad de eliminar el servicio de Elasticsearch de la capa de infraestructura para optimizar el uso de los recursos. |
| 2011 | validate-config | Elasticsearch versión de servicio en la capa de infraestructura no es compatible con la versión actual de la módulo Elasticsearch/Elasticsearch, utilizada por su aplicación de comercio de Adobe Systems. |  |
| 2012 | validate-config | La configuración actual no es compatible con esta versión de Adobe Commerce |  |
| 2013 | validate-config | Opciones de SCD ignoradas porque el proceso de implementación no se ejecutó en la fase de compilación |  |
| 2014 | validate-config | La configuración contiene variables o valores obsoletos |  |
| 2015 | validate-config | La configuración del entorno no es válida |  |
| 2016 | validate-config | La configuración de tipo JSON no se puede decodificar |  |
| 2017 | validate-config | La configuración actual no es compatible con esta versión de Adobe Commerce |  |
| 2018 | validate-config | Algunos servicios han superado el límite de vida |  |
| 2019 | validate-config | La opción de configuración de búsqueda MySQL está obsoleta | Utilice Elasticsearch en su lugar. |
| 2029 | validate-config | La base de datos dividida quedó obsoleta en Adobe Systems Commerce y Magento Open Source 2.4.2 y se eliminará en 2.5. | Si utiliza una base de datos dividida, debe empezar a planificar la reversión a una base de datos única o la migración a ella, o bien utilizar un método alternativo. |
| 2020 | install-update | Adobe Systems instalación de Commerce se había completado, pero el archivo de `app/etc/env.php` configuración faltaba o estaba vacío. | Los datos necesarios se restauran desde entorno configuraciones y desde el archivo .magento.env.yaml. |
| 2021 | install-update:conexión-db | Para bases de datos divididas utilizadas conexiones personalizadas |  |
| 2022 | install-update:conexión-db | Ha cambiado a una configuración de base de datos que no es compatible con la conexión esclava. |  |
| 2023 | install-update:split-db | Se omite la habilitación de una base de datos dividida. |  |
| 2024 | install-update:split-db | En la variable SPLIT_DB falta la configuración para los tipos de conexión dividida. |  |
| 2025 | install-update:split-db | No se ha establecido la conexión esclava. |  |
| 2026 | pre-implementar:restore-writable-dirs | No se han podido restaurar algunos datos generados durante la fase de versión en los directorios montados | Consulte el `cloud.log` para obtener más información. |
| 2027 | validate-config:image-mode-variable | No se admite el valor de modo para MAGE_MODE entorno variable | Quitar el MAGE_MODE entorno variable, o cambie su valor a &quot;producción&quot;. Adobe Commerce en la infraestructura en la nube solo admite el modo &quot;producción&quot;. |
| 2028 | almacenamiento remota | No se pudo habilitar el almacenamiento remoto. | Compruebe las credenciales de almacenamiento remoto. |
| 2030 | validate-config | Los servicios Elasticsearch y OpenSearch se instalan en el nivel de infraestructura. Adobe Commerce y Magento Open Source 2.4.4 y posteriores utilizan OpenSearch de forma predeterminada | Considere la posibilidad de eliminar el servicio Elasticsearch u OpenSearch de la capa de infraestructura para optimizar el uso de los recursos. |

### Fase posterior a la implementación

| Código de error | Paso posterior a la implementación | Descripción del error (título) | Acción sugerida |
| - | - | - | - |
| 3001 | validate-config | El registro de depuración está habilitado en Adobe Commerce | Para ahorrar espacio en disco, no habilite el registro de depuración para los entornos de producción. |
| 3002 | calentamiento | No se pueden recuperar las URL de tienda |  |
| 3003 | calentamiento | No se puede obtener la URL del almacén |  |
| 3004 | reserva | No se pueden crear archivos de copia de seguridad |  |

### General

| Error código | Paso general | Descripción Error (Título) | Acción sugerida |
| - | - | - | - |
| 4001 |  | No se puede obtener el número de procesadores del sistema: |  |
