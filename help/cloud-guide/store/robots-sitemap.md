---
title: Añadir robots de mapa del sitio y de motor de búsqueda
description: Aprenda a añadir robots de mapa del sitio y de motores de búsqueda a Adobe Commerce en la infraestructura en la nube.
feature: Cloud, Configuration, Search, Site Navigation
exl-id: 060dc1f5-0e44-494e-9ade-00cd274e84bc
source-git-commit: 1d52481fb6874f3a9ba14b0ff4fe39dc7d564938
workflow-type: tm+mt
source-wordcount: '574'
ht-degree: 0%

---

# Añadir robots de mapa del sitio y de motor de búsqueda

Si intenta generar y escribir el archivo `sitemap.xml` en el directorio raíz, se producirá el siguiente error:

```
Please make sure that "/" is writable by the web-server.
```

Con Adobe Commerce en la infraestructura en la nube, solo puede escribir en directorios específicos, como `var`, `pub/media`, `pub/static` o `app/etc`. Cuando genere el archivo `sitemap.xml` mediante el panel de administración, debe especificar la ruta de acceso `/media/`.

No tiene que generar un archivo `robots.txt` porque genera el contenido `robots.txt` bajo demanda y lo almacena en la base de datos. Puede ver el contenido en su explorador con el vínculo `<domain.your.project>/robots.txt` o `<domain.your.project>/robots`.

Esto requiere ECE-Tools versión 2002.0.12 y posterior con un archivo `.magento.app.yaml` actualizado. Vea un ejemplo de estas reglas en el [repositorio de magento en la nube](https://github.com/magento/magento-cloud/blob/master/.magento.app.yaml#L43-L49).

**Para generar un archivo de `sitemap.xml` en la versión 2.2 y posterior**:

1. Acceda al administrador.
1. En el menú _Marketing_, haga clic en **Mapa del sitio** en la sección _SEO y búsqueda_.
1. En la vista _Mapa del sitio_, haga clic en **Agregar mapa del sitio**.
1. En la vista _Nuevo mapa del sitio_, escriba los siguientes valores:

   - **Nombre de archivo**:`sitemap.xml`
   - **Ruta**:`/media/`

1. Haga clic en **Guardar y generar**. El nuevo mapa del sitio estará disponible en la cuadrícula _Mapa del sitio_.
1. Haga clic en la ruta de acceso de la columna _Vínculo para Google_.

**Para agregar contenido al archivo `robots.txt`**:

1. Acceda al administrador.
1. En el menú _Contenido_, haga clic en **Configuración** en la sección _Diseño_.
1. En la vista _Configuración de diseño_, haga clic en **Editar** para el sitio web en la columna _Acción_.
1. En la vista _Sitio web principal_, haga clic en **Robots de motores de búsqueda**.
1. Actualice el campo **Editar instrucción personalizada de robots.txt**.
1. Haga clic en **Guardar configuración**.
1. Compruebe el archivo `<domain.your.project>/robots.txt` o la dirección URL `<domain.your.project>/robots` en el explorador.

>[!NOTE]
>
>Si el archivo `<domain.your.project>/robots.txt` genera un `404 error`, [envíe un vale de soporte técnico de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=es#submit-ticket) para quitar la redirección de `/robots.txt` a `/media/robots.txt`.

## Reescribir utilizando el fragmento de VCL de Fastly

Si tiene dominios diferentes y necesita mapas de sitio independientes, puede crear una VCL para enrutar al mapa del sitio adecuado. Genere el archivo `sitemap.xml` en el panel de administración como se ha descrito anteriormente y, a continuación, cree un fragmento de VCL personalizado de Fastly para administrar la redirección. Ver [fragmentos personalizados de VCL de Fastly](../cdn/fastly-vcl-custom-snippets.md).

>[!NOTE]
>
> Puede cargar fragmentos de VCL personalizados desde la IU de administración o mediante la API de Fastly. Vea [ejemplos y tutoriales de fragmentos de VCL personalizados](../cdn/fastly-vcl-custom-snippets.md#example-vcl-snippet-code).

### Usar un fragmento de VCL de Fastly para redireccionar

Cree un fragmento de VCL personalizado para reescribir la ruta de acceso de `sitemap.xml` a `/media/sitemap.xml` mediante los pares clave-valor `type` y `content`.

```json
{
  "name": "sitemapxml_rewrite",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path ~ \"^/?sitemap.xml$\" ) { set req.url = \"/media/sitemap.xml\"; }"
}
```

El siguiente ejemplo muestra cómo reescribir la ruta de acceso de `robots.txt` y `sitemap.xml` en `/media/robots.txt` y `/media/sitemap.xml`

```json
{
  "name": "sitemaprobots_rewrite",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path ~ \"^/?sitemap.xml$\" ) { set req.url = \"/media/sitemap.xml\"; } else if (req.url.path ~ \"^/?robots.txt$\") { set req.url = \"/media/robots.txt\";}"
}
```

**Para usar un fragmento de VCL de Fastly para una redirección de dominio en particular**:

Cree un archivo de `pub/media/domain_robots.txt`, donde el dominio es `domain.com`, y use el siguiente fragmento de VCL:

```json
{
  "name": "domain_robots",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path == \"/robots.txt\" ) { if ( req.http.host ~ \"(domain).com$\" ) { set req.url = \"/media/\" re.group.1 \"_robots.txt\"; }}"
}
```

El fragmento VCL enruta `http://domain.com/robots.txt` y presenta el archivo `pub/media/domain_robots.txt`.

Para configurar una redirección para `robots.txt` y `sitemap.xml` en un solo fragmento, cree `pub/media/domain_robots.txt` y `pub/media/domain_sitemap.xml` archivos, donde el dominio es `domain.com` y use el siguiente fragmento de VCL:

```json
{
  "name": "domain_sitemaprobots",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path == \"/robots.txt\" ) { if ( req.http.host ~ \"(domain).com$\" ) { set req.url = \"/media/\" re.group.1 \"_robots.txt\"; }} else if ( req.url.path == \"/sitemap.xml\" ) { if ( req.http.host ~ \"(domain).com$\" ) {  set req.url = \"/media/\" re.group.1 \"_sitemap.xml\"; }}"
}
```

En la configuración de administración de `sitemap`, debe especificar la ubicación del archivo usando `pub/media/` en lugar de `/`.

### Configurar la indexación por motor de búsqueda

Para activar las personalizaciones de `robots.txt` en Producción, habilite la indexación por motores de búsqueda para la opción `<environment-name>` en la configuración del proyecto en Cloud Console:

- Consola de nube heredada: la dirección URL sigue el patrón `https://<region-id>.magento.cloud/projects/<project_id>`

  Cambie la configuración [!UICONTROL Indexing by search engines] (consola heredada) [!UICONTROL Hide from search engines] (consola Adobe) a **Activado**.

  ![Use [!DNL Cloud Console] para administrar entornos](../../assets/robots-indexing-by-search-engine.png)

- Consola de Adobe Cloud: la dirección URL sigue el patrón ``https://console.adobecommerce.com/<username>/<project_id>``

  Desmarque la configuración [!UICONTROL Hide from search engines].

- También puede utilizar la CLI de Magento en la nube para actualizar esta configuración:

  ```bash
  magento-cloud environment:info -p <project_id> -e production restrict_robots false
  ```

>[!NOTE]
>
>- La indexación por motores de búsqueda solo se puede habilitar en Producción, pero no en ninguno de los entornos inferiores.
>
>- Si usa PWA Studio y no puede obtener acceso al archivo `robots.txt` configurado, agregue `robots.txt` a la [Lista de permitidos de Front Name](https://github.com/magento/magento2-upward-connector#front-name-allowlist) en **Tiendas** > Configuración > **General** > **Web** > Configuración de UPWARD PWA.

