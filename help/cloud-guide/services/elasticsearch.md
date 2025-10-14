---
title: Configuración del servicio de Elasticsearch
description: Obtenga información sobre cómo habilitar el servicio de Elasticsearch para Adobe Commerce en la infraestructura en la nube.
feature: Cloud, Search, Services
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '788'
ht-degree: 0%

---

# Configuración del servicio de Elasticsearch

[Elasticsearch](https://www.elastic.co) es un producto de código abierto que le permite tomar datos de cualquier fuente, cualquier formato, y buscarlos y visualizarlos en tiempo real.

{{elasticsearch-support}}

Para la versión 2.4.4 y posterior de Adobe Commerce, consulte [Configuración del servicio OpenSearch](opensearch.md).

- Elasticsearch realiza búsquedas rápidas y avanzadas en los productos del catálogo de productos
- Los analizadores de Elasticsearch admiten varios idiomas
- Admite palabras de detención y sinónimos
- La indexación no afecta a los clientes hasta que se completa la operación de reindexación

>[!TIP]
>
>Adobe recomienda configurar siempre Elasticsearch para su proyecto de infraestructura en la nube de Adobe Commerce, incluso si planea configurar una herramienta de búsqueda de terceros para su aplicación de Adobe Commerce. La configuración del Elasticsearch proporciona una opción de reserva en caso de que falle la herramienta de búsqueda de terceros.

{{service-instruction}}

**Para habilitar el Elasticsearch**:

1. En Proyectos iniciales, agregue el servicio `elasticsearch` al archivo `.magento/services.yaml` con la versión del Elasticsearch y el espacio en disco asignado en MB.

   ```yaml
   elasticsearch:
       type: elasticsearch:<version>
       disk: 1024
   ```

   En los proyectos Pro, debe enviar un ticket de asistencia de Adobe Commerce para cambiar la versión del Elasticsearch en los entornos de ensayo y producción.

1. Establezca la propiedad `relationships` en el archivo `.magento.app.yaml`.

   ```yaml
   relationships:
       elasticsearch: "elasticsearch:elasticsearch"
   ```

1. Agregar, confirmar y enviar cambios de código.

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable Elasticsearch" && git push origin <branch-name>
   ```

   Para obtener información sobre cómo afectan estos cambios a sus entornos, consulte [Servicios](services-yaml.md).

1. Una vez completado el proceso de implementación, utilice SSH para iniciar sesión en el entorno remoto.

   ```bash
   magento-cloud ssh
   ```

1. Reindexe el índice de búsqueda del catálogo.

   ```bash
   bin/magento indexer:reindex catalogsearch_fulltext
   ```

1. Limpie la caché.

   ```bash
   bin/magento cache:clean
   ```

{{service-change-tip}}

## Compatibilidad de software de Elasticsearch

Cuando instale o actualice su proyecto de infraestructura de Adobe Commerce en la nube, compruebe siempre la compatibilidad entre la versión del servicio de Elasticsearch y el cliente de [Elasticsearch PHP](https://github.com/elastic/elasticsearch-php) para Adobe Commerce.

- **Configuración por primera vez**: confirme que la versión del Elasticsearch especificada en el archivo `services.yaml` es compatible con el cliente PHP del Elasticsearch configurado para Adobe Commerce.

- **Actualización del proyecto**: compruebe que el cliente PHP Elasticsearch de la nueva versión de la aplicación sea compatible con la versión del servicio Elasticsearch instalada en la infraestructura de la nube.

La compatibilidad y la versión del servicio para Adobe Commerce en la infraestructura en la nube están determinadas por las versiones implementadas en la infraestructura en la nube y, a veces, difieren de las versiones admitidas por las implementaciones locales de Adobe Commerce. Consulte [Versiones de servicio](services-yaml.md#service-versions).

**Para comprobar la compatibilidad del software del Elasticsearch**:

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. Mostrar los detalles del Elasticsearch para el entorno activo.

   ```bash
   magento-cloud relationships --property=elasticsearch
   ```

1. Como alternativa, puede utilizar SSH para iniciar sesión en el entorno remoto.

   ```bash
   magento-cloud ssh
   ```

1. Compruebe la versión del paquete Composer de `elasticsearch/elasticsearch`.

   ```bash
   composer show elasticsearch/elasticsearch
   ```

   En la respuesta, compruebe la versión instalada en la propiedad `versions`.

   ```
   name     : elasticsearch/elasticsearch
   descrip. : PHP Client for Elasticsearch
   keywords : client, elasticsearch, search
   versions : * v7.17.1
   type     : library
   license  : Apache License 2.0 (Apache-2.0) (OSI approved) https://spdx.org/licenses/Apache-2.0.html#licenseText
   license  : GNU Lesser General Public License v2.1 only (LGPL-2.1-only) (OSI approved) https://spdx.org/licenses/LGPL-2.1-only.html#licenseText
   homepage :
   source   : [git] git@github.com:elastic/elasticsearch-php.git f1b8918f411b837ce5f6325e829a73518fd50367
   dist     : [zip] https://api.github.com/repos/elastic/elasticsearch-php/zipball/f1b8918f411b837ce5f6325e829a73518fd50367 f1b8918f411b837ce5f6325e829a73518fd50367
   path     : ~/vendor/elasticsearch/elasticsearch
   names    : elasticsearch/elasticsearch
   ```

   Además, puede encontrar la versión del cliente PHP del Elasticsearch en el archivo `composer.lock` en el directorio raíz del entorno.

1. Desde la línea de comandos, recupere los detalles de conexión del servicio Elasticsearch.

   ```bash
   vendor/bin/ece-tools env:config:show services
   ```

   En la respuesta, busque la dirección IP del extremo del servicio Elasticsearch:

   ```
   | elasticsearch:                                                                                                  |
   +------------------------------------------+----------------------------------------------------------------------+
   | username                                 | null                                                                 |
   | scheme                                   | http                                                                 |
   | service                                  | elasticsearch                                                        |
   | fragment                                 | null                                                                 |
   | ip                                       | 169.254.220.11                                                       |
   | hostname                                 | dzggu33f75wi3sd24lgwtoupxm.elasticsearch.service._.magentosite.cloud |
   | public                                   | false                                                                |
   | cluster                                  | fo3qdoxtla4j4-master-7rqtwti                                         |
   | host                                     | elasticsearch.internal                                               |
   | rel                                      | elasticsearch                                                        |
   | query                                    |                                                                      |
   | path                                     | null                                                                 |
   | password                                 | null                                                                 |
   | type                                     | elasticsearch:6.5                                                    |
   | port                                     | 9200                                                                 |
   +------------------------------------------+----------------------------------------------------------------------+
   ```

1. Recupere el servicio de Elasticsearch instalado `version:number` del extremo del servicio.

   ```bash
   curl -XGET <elasticsearch-service-endpoint-ip-address>:9200/
   ```

   ```json
   {
      "name" : "-AqGi9D",
      "cluster_name" : "elasticsearch",
      "cluster_uuid" : "_yze6-ywSEW1MaAF8ZPWyQ",
      "version" : {
        "number" : "6.5.4",
        "build_flavor" : "default",
        "build_type" : "deb",
        "build_hash" : "82a8aa7",
        "build_date" : "2019-01-23T12:07:18.760675Z",
        "build_snapshot" : false,
        "lucene_version" : "7.5.0",
        "minimum_wire_compatibility_version" : "5.6.0",
        "minimum_index_compatibility_version" : "5.0.0"
   },
   "  tagline" : "You Know, for Search"
   }
   ```

1. Comprobar la compatibilidad de versiones entre el servicio Elasticsearch y el cliente PHP.

   Si las versiones son incompatibles, realice una de las siguientes actualizaciones en la configuración de su entorno:

   - Cambie el cliente PHP del Elasticsearch a una versión compatible con la versión del servicio del Elasticsearch.

     ```bash
     composer require "elasticsearch/elasticsearch:~<version>"
     ```

   - Cambie la versión del servicio Elasticsearch en el archivo `services.yaml` a una versión compatible con el cliente PHP Elasticsearch.

     {{pro-update-service}}

## Reinicie el servicio de Elasticsearch

Si necesita reiniciar el servicio [Elasticsearch](https://www.elastic.co), debe ponerse en contacto con el soporte técnico de Adobe Commerce.

## Configuración de búsqueda adicional

- De forma predeterminada, la configuración de búsqueda de los entornos de Cloud se regenera cada vez que realiza la implementación. Puede usar la variable de implementación `SEARCH_CONFIGURATION` para conservar la configuración de búsqueda personalizada entre implementaciones. Consulte [Implementar variables](../environment/variables-deploy.md#search_configuration).

- Después de configurar el servicio de Elasticsearch para el proyecto, utilice la IU de administración para probar la conexión del Elasticsearch y personalizar la configuración del Elasticsearch para Adobe Commerce.

### Añadir complementos para el Elasticsearch

Opcionalmente, puede agregar complementos para Elasticsearch agregando la sección `configuration:plugins` al servicio Elasticsearch en el archivo `.magento/services.yaml`. Por ejemplo, el siguiente código habilita los complementos de análisis de ICU y análisis fonético.

```yaml
elasticsearch:
    type: elasticsearch:<service-version>
    disk: 1024
    configuration:
        plugins:
            - analysis-icu
            - analysis-phonetic
```

Si usa el complemento de terceros Elastic Suite, debe [actualizar el paquete `ece-tools`](../dev-tools/update-package.md) a la versión 2002.0.19 o posterior.
Al configurar Elastic Suite, agregue los ajustes de configuración a la variable de implementación `ELASTICSUITE_CONFIGURATION`. Esta configuración guarda la configuración en todas las implementaciones.

### Eliminar complementos de Elasticsearch

Al eliminar las entradas del complemento de `elasticsearch:` en `.magento/services.yaml`, no se desinstalan ni se deshabilitan, como cabría esperar. Debe reindexar los datos de Elasticsearch. Este comportamiento es intencional para evitar la posible pérdida o corrupción de datos que dependen de estos complementos.

**Para quitar los complementos de Elasticsearch**:

1. Quite las entradas del complemento Elasticsearch del archivo `.magento/services.yaml`.
1. Agregue, confirme e inserte los cambios de código.

   ```bash
   git add .magento/services.yaml
   ```

   ```bash
   git commit -m "Remove Elasticsearch plugin"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. Confirme los cambios de `.magento/services.yaml` en su repositorio de Cloud.
1. Reindexe el índice de búsqueda del catálogo.

   ```bash
   bin/magento indexer:reindex catalogsearch_fulltext
   ```

1. Limpie la caché.

   ```bash
   bin/magento cache:clean
   ```

>[!TIP]
>
>Para obtener más información sobre cómo usar o solucionar problemas del complemento Elastic Suite con Adobe Commerce, consulte la [documentación de Elastic Suite](https://github.com/Smile-SA/elasticsuite).

## Resolución de problemas

Consulte los siguientes artículos de soporte de Adobe Commerce para obtener ayuda sobre la resolución de problemas del Elasticsearch:

- El Elasticsearch [5 está configurado, pero la página de búsqueda no se carga con el error &quot;Los datos de campo están deshabilitados...&quot; &#x200B;](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/elasticsearch/elasticsearch-5-is-configured-but-search-page-does-not-load-with-fielddata-is-disabled...-error.html?lang=es)
- [Elasticsearch en el solucionador de problemas de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/elasticsearch/elasticsearch-in-magento-troubleshooter.html)
- [El estado del índice de Elasticsearch es `yellow` o `red`](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/elasticsearch/elasticsearch-index-status-is-yellow-or-red.html?lang=es)
