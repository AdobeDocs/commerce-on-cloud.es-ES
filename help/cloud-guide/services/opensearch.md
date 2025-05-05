---
title: Configuración del servicio OpenSearch
description: Obtenga información sobre cómo habilitar el servicio OpenSearch para Adobe Commerce en la infraestructura en la nube.
feature: Cloud, Search, Services
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '651'
ht-degree: 0%

---

# Configuración del servicio OpenSearch

El servicio [OpenSearch](https://www.opensearch.org) es una ramificación de código abierto de Elasticsearch 7.10.2, tras los cambios de licencia de Elasticsearch. Ver el [proyecto de código abierto](https://github.com/opensearch-project) en GitHub.

{{elasticsearch-support}}

OpenSearch le permite tomar datos de cualquier fuente, cualquier formato, y buscarlos y visualizarlos en tiempo real.

- Búsquedas rápidas y avanzadas en productos del catálogo de productos
- Los analizadores OpenSearch admiten varios idiomas
- Admite palabras de detención y sinónimos
- La indexación no afecta a los clientes hasta que se completa la operación de reindexación

{{service-instruction}}

>[!TIP]
>
>Adobe recomienda configurar siempre OpenSearch para su proyecto de Adobe Commerce en la nube, incluso si tiene pensado configurar una herramienta de búsqueda de terceros para su aplicación de Adobe Commerce. La configuración de OpenSearch proporciona una opción de reserva si falla la herramienta de búsqueda de terceros.

**Para habilitar OpenSearch**:

1. Para los entornos de integración de Starter y Pro, agregue el servicio `opensearch` al archivo `.magento/services.yaml` con la versión adecuada y el espacio en disco asignado en MB. En este caso, la versión 2 es apropiada. La versión secundaria no es necesaria porque la infraestructura de nube utiliza la última versión de OpenSearch.

   ```yaml
   opensearch:
       type: opensearch:2
       disk: 1024
   ```

   Para los proyectos Pro, debe [enviar un ticket de soporte de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=es#submit-ticket) para cambiar la versión de OpenSearch en los entornos de ensayo y producción.

1. Establezca o compruebe la propiedad `relationships` en el archivo `.magento.app.yaml`.

   ```yaml
   relationships:
       opensearch: "opensearch:opensearch"
   ```

1. Agregar, confirmar y enviar cambios de código.

   ```bash
   git add .magento/services.yaml .magento.app.yaml
   ```

   ```bash
   git commit -m "Enable OpenSearch"
   ```

   ```bash
   git push origin <branch-name>
   ```

   Para obtener información sobre cómo afectan estos cambios a sus entornos, consulte [Configurar servicios](services-yaml.md).

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

## Compatibilidad del software OpenSearch

Cuando instale o actualice su proyecto de infraestructura en la nube de Adobe Commerce, compruebe siempre la compatibilidad entre la versión del servicio OpenSearch y el cliente [OpenSearch PHP](https://github.com/opensearch-project/opensearch-php) para Adobe Commerce.

- **Configuración por primera vez**: confirme que la versión de OpenSearch especificada en el archivo `services.yaml` es compatible con el cliente PHP OpenSearch configurado para Adobe Commerce.

- **Actualización del proyecto**-Compruebe que el cliente OpenSearch de PHP en la nueva versión de la aplicación es compatible con la versión del servicio OpenSearch instalada en la infraestructura en la nube.

La compatibilidad y la versión del servicio están determinadas por las versiones probadas e implementadas en la infraestructura en la nube, y a veces difieren de las versiones admitidas por las implementaciones locales de Adobe Commerce. Consulte [Requisitos del sistema](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html?lang=es) en la _Guía de instalación_ para obtener una lista de las versiones compatibles.

**Para comprobar la compatibilidad del software OpenSearch**:

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. Mostrar los detalles de OpenSearch para el entorno activo.

   ```bash
   magento-cloud relationships --property=opensearch
   ```

1. Como alternativa, puede utilizar SSH para iniciar sesión en el entorno remoto.

   ```bash
   magento-cloud ssh
   ```

1. Recupere los detalles de conexión del servicio OpenSearch.

   ```bash
   vendor/bin/ece-tools env:config:show services
   ```

   En la respuesta, busque la dirección IP y el puerto para el extremo del servicio OpenSearch:

   ```
   +------------------------------------------+--------------------------------------------------------+
   | opensearch:                                                                                       |
   +------------------------------------------+--------------------------------------------------------+
   | username                                 | null                                                   |
   | scheme                                   | http                                                   |
   | service                                  | opensearch                                             |
   | fragment                                 | null                                                   |
   | ip                                       | 169.254.220.11                                         |
   | hostname                                 | hostf75wi3sd24l.opensearch.service._.magentosite.cloud |
   | port                                     | 9200                                                   |
   | cluster                                  | projectID-develop-4ranwui                              |
   | host                                     | opensearch.internal                                    |
   | rel                                      | opensearch                                             |
   | path                                     | null                                                   |
   | query                                    |                                                        |
   | password                                 | null                                                   |
   | type                                     | opensearch:2                                           |
   | public                                   | false                                                  |
   | host_mapped                              | false                                                  |
   ```

1. Recupere el servicio OpenSearch `version:number` instalado del extremo del servicio.

   ```bash
   curl -XGET <opensearch-service-endpoint-ip-address>:9200
   ```

   ```json
   {
      "name" : "opensearch.0",
      "cluster_name" : "opensearch",
      "cluster_uuid" : "_yzaae6-ywSEW1MaAF8ZPWyQ",
      "version" : {
        "distribution" : "opensearch",
        "number" : "2.5.0",
        "build_type" : "deb",
        "build_hash" : "aaaaaaa",
        "build_date" : "2023-01-23T12:07:18.760675Z",
        "build_snapshot" : false,
        "lucene_version" : "9.4.2",
        "minimum_wire_compatibility_version" : "7.10.0",
        "minimum_index_compatibility_version" : "7.0.0"
   },
   "tagline" : "The OpenSearch Project: https://opensearch.org/"
   }
   ```

{{pro-update-service}}

## Reinicie el servicio OpenSearch

Si necesita reiniciar el servicio OpenSearch, debe ponerse en contacto con el servicio de asistencia de Adobe Commerce.

## Configuración de búsqueda adicional

- De forma predeterminada, la configuración de búsqueda de los entornos de Cloud se regenera cada vez que realiza la implementación. Puede usar la variable de implementación `SEARCH_CONFIGURATION` para conservar la configuración de búsqueda personalizada entre implementaciones. Consulte [Implementar variables](../environment/variables-deploy.md#search_configuration).

- Después de configurar el servicio OpenSearch para su proyecto, utilice la IU de administración para probar la conexión de OpenSearch y personalizar la configuración de OpenSearch para Adobe Commerce.

### Añadir complementos para OpenSearch

Opcionalmente, puede agregar complementos para OpenSearch agregando la sección `configuration:plugins` al servicio OpenSearch en el archivo `.magento/services.yaml`. Por ejemplo, el siguiente código habilita los complementos de análisis de ICU y análisis fonético.

```yaml
opensearch:
    type: opensearch:2
    disk: 1024
    configuration:
        plugins:
            - analysis-icu
            - analysis-phonetic
```

Consulte [Proyecto OpenSearch](https://github.com/opensearch-project) para obtener más información sobre los complementos.

### Eliminar complementos para OpenSearch

Al quitar las entradas del complemento de la sección `opensearch:` del archivo `.magento/services.yaml`, **no** desinstala o deshabilita el servicio. Para deshabilitar completamente el servicio, debe reindexar los datos de OpenSearch después de quitar los complementos del archivo `.magento/services.yaml`. Este diseño evita la posible pérdida o corrupción de datos que dependen de estos complementos.

**Para quitar los complementos de OpenSearch**:

1. Quite las entradas del complemento OpenSearch del archivo `.magento/services.yaml`.
1. Agregue, confirme e inserte los cambios de código.

   ```bash
   git add .magento/services.yaml
   ```

   ```bash
   git commit -m "Remove OpenSearch plugin"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. Confirme los `.magento/services.yaml` cambios en su repositorio de la nube.
1. Reindexe el índice de búsqueda del catálogo.

   ```bash
   bin/magento indexer:reindex catalogsearch_fulltext
   ```

1. Limpie la caché.

   ```bash
   bin/magento cache:clean
   ```
