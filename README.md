---
source-git-commit: 8cbda8ca194c5e5865073c9eb08e061cfecb5ace
workflow-type: tm+mt
source-wordcount: '831'
ht-degree: 1%

---
# Adobe Commerce en infraestructura en la nube

Este sitio contiene la documentación para desarrolladores más reciente de Commerce en la infraestructura en la nube.

- [Guía de infraestructura de Commerce en la nube](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/user-guide/overview)
- [Introducción a Commerce](https://experienceleague.adobe.com/es/docs/commerce-on-cloud/start/overview) en la infraestructura en la nube

## Código de conducta de Adobe Open Source

Este proyecto ha adoptado el [Código de conducta de Adobe Open Source](code-of-conduct.md). Para obtener más información, consulte el artículo [Colaboración](contributing.md).

## Acerca de sus contribuciones al contenido de Adobe

Consulte la [Guía del colaborador de Adobe Docs](https://experienceleague.adobe.com/es/docs/contributor/contributor-guide/introduction).

La forma en que contribuya depende de quién sea y del tipo de cambios con los que desee contribuir:

### Cambios menores

Si va a contribuir con actualizaciones menores, visite el artículo y haga clic en el área de comentarios que aparece en la parte inferior del artículo, haga clic en **Opciones de comentarios detalladas** y, a continuación, haga clic en **Sugerir una edición** para ir al archivo de código fuente Markdown en GitHub. Utilice la interfaz de usuario de GitHub para realizar las actualizaciones.

Las correcciones o aclaraciones menores que envíe para la documentación y los ejemplos de código en este repositorio están sujetos a las condiciones de uso de Adobe.

### Cambios importantes o nuevos artículos de los miembros de la comunidad

Si forma parte de la comunidad de Adobe y desea crear un nuevo artículo o enviar cambios importantes, utilice la pestaña Problemas del repositorio Git para enviar un problema e iniciar una conversación con el equipo de documentación. Una vez que haya acordado un plan, deberá trabajar con un empleado para ayudar a incorporar ese nuevo contenido a través de una combinación de trabajo en los repositorios públicos y privados.

### Cambios importantes de los empleados de Adobe

Si es redactor técnico, administrador de programa o desarrollador del equipo de producto para una solución de Adobe Experience Cloud y debe contribuir o crear artículos técnicos, debe utilizar el repositorio privado en `https://git.corp.adobe.com/AdobeDocs`.

## Herramientas y configuración

Los colaboradores de la comunidad pueden utilizar la interfaz de usuario de GitHub para la edición básica o bifurcar el repositorio para realizar contribuciones importantes.

Consulte la [Guía para colaboradores de Adobe Docs](https://experienceleague.adobe.com/es/docs/contributor/contributor-guide/introduction) para obtener más información.

## Utilizar Markdown para dar formato al tema

Todos los artículos de este repositorio utilizan GitHub Flavored Markdown. Si no está familiarizado con el uso de markdown, consulte:

- [Conceptos básicos de Markdown](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
- [Hoja de trucos de markdown imprimible](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)

## Plantillas

Para algunos temas, utilizamos archivos de datos y plantillas para generar contenido publicado. Los casos de uso de este enfoque incluyen:

- Publicación de grandes conjuntos de contenido generado mediante programación
- Proporciona una única fuente fiable para los clientes de varios sistemas que requieren formatos de archivo legibles por el equipo, como YAML, para la integración (por ejemplo, herramientas CLI de la nube y configuraciones de servicio)

Algunos ejemplos de contenido con plantillas son, entre otros, los siguientes:

- [Referencia de CLI de Cloud](help/templated/cloud-cli-ref.md)
- [Paquetes de nube](help/templated/cloud-packages.md)
- [Referencia de herramientas ECE](help/templated/ece-tools.md)
- [Extensiones de PHP para la nube](help/templated/php-extensions-cloud.md)

### Generación de contenido con plantilla

En general, la mayoría de los redactores solo necesitan añadir una versión de lanzamiento a las tablas de disponibilidad del producto y requisitos del sistema. El mantenimiento del resto del contenido con plantillas se automatiza o se administra mediante un miembro del equipo dedicado. Estas instrucciones están destinadas a la mayoría de los escritores.

>**NOTA:**
>
>- La generación de contenido con plantillas requiere trabajar en la línea de comandos de un terminal.
>- Debe tener instalado Ruby para ejecutar el script de procesamiento. Consulte [_jekyll/.ruby-version] (_jekyll/.ruby-version) para obtener la versión requerida.

Consulte lo siguiente para obtener una descripción de la estructura de archivos del contenido con plantillas:

- `_jekyll`: contiene temas con plantillas y recursos necesarios
- `_jekyll/_data`: contiene los formatos de archivo legibles por el equipo utilizados para procesar plantillas
- `_jekyll/templated`: contiene archivos de plantilla basados en HTML que utilizan el lenguaje de plantilla Liquid
- `help/_includes/templated`: contiene la salida generada para el contenido con plantilla en formato de archivo `.md` para que se pueda publicar en temas de Experience League; el script de procesamiento escribe automáticamente la salida generada en este directorio

Para actualizar el contenido con plantillas:

1. En el editor de texto, abra un archivo de datos en el directorio `_jekyll/_data`. Por ejemplo:

   - [Referencia de CLI de nube](help/templated/cloud-cli-ref.md): `_jekyll/_data/cloud-cli-ref.yaml`
   - [Paquetes de nube](help/templated/cloud-packages.md): `_jekyll/_data/cloud-packages.yaml`
   - [Referencia de herramientas ECE](help/templated/ece-tools.md): `_jekyll/_data/ece-tools.yaml`

2. Utilice la estructura YAML existente para crear entradas.

3. Vaya al directorio `_jekyll`.

   ```bash
   cd _jekyll
   ```

4. Genere contenido con plantilla y escriba el resultado en el directorio `help/_includes/templated`.

   ```bash
   bundle exec rake render
   ```

   >**NOTA:** Debe ejecutar el script desde el directorio `_jekyll`. Si es la primera vez que ejecuta el script, debe instalar primero las dependencias de Ruby con el comando `bundle install`. Las tareas y dependencias de rastrillo principales (Jekyll, Rake, optimización de imágenes) las proporciona la joya `adobe-comdox-exl-rake-tasks` para mejorar la capacidad de mantenimiento en los repositorios de documentación de Adobe Commerce. Las tareas personalizadas específicas de este repositorio se implementan en `Rakefile`.

5. Vuelva al directorio `root`.

   ```bash
   cd ..
   ```

6. Compruebe que se hayan modificado los `help/_includes/templated` archivos esperados.

   ```bash
   git status
   ```

   Debería ver un resultado similar al siguiente:

   ```bash
   modified:   _data/cloud-cli-ref.yaml
   modified:   help/_includes/templated/cloud-cli-ref.md
   ```

7. Presione los cambios.

   ```bash
   git add .
   git commit -m "descriptive message of the intended commit"
   git push
   ```

Consulte la documentación de Jekyll para obtener más información sobre [Archivos de datos](https://jekyllrb.com/docs/datafiles), [Filtros líquidos](https://jekyllrb.com/docs/liquid/filters/) y otras características.

## Tareas de rastrillo disponibles

Este repositorio usa las tareas de rastrillado proporcionadas por la joya `adobe-comdox-exl-rake-tasks`. Para ver todas las tareas disponibles, ejecute:

```bash
cd _jekyll
bundle exec rake --tasks
```

## Enlaces previos a la confirmación para la optimización de imágenes

Este repositorio incluye enlaces automatizados previos a la confirmación que optimizan las imágenes antes de la confirmación. **Todos los colaboradores deben habilitar estos vínculos** para garantizar una optimización de imagen coherente y una reducción del tamaño del repositorio.

### Configuración rápida

Después de clonar el repositorio, ejecute:

```bash
.githooks/setup-hooks.sh
```

### Qué hacen los ganchos

- Detectar automáticamente archivos de imagen clasificados (PNG, JPG, JPEG, GIF, SVG)
- Ejecutar `image_optim` para comprimir y optimizar imágenes
- Volver a almacenar automáticamente las imágenes optimizadas
- Asegúrese de que todas las imágenes confirmadas estén optimizadas correctamente

### Ventajas

- Tamaño de repositorio reducido
- Cargas de página más rápidas para la documentación
- Calidad de imagen coherente en todos los colaboradores
- No se requiere optimización manual

Para obtener instrucciones de instalación, solución de problemas y configuración detalladas, consulte [`.githooks/README.md`](.githooks/README.md).
