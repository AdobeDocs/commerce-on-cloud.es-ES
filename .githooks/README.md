---
source-git-commit: 3d3df68ab5e1781974348190b30372c0b8169d1d
workflow-type: tm+mt
source-wordcount: '689'
ht-degree: 0%

---
# Enlaces previos a la confirmación para la optimización de imágenes

Este directorio contiene enlaces previos a la confirmación que optimizan automáticamente las imágenes antes de enviarlas al repositorio.

## Qué hacen los ganchos

- **Detectar automáticamente** archivos de imagen clasificados (`.png`, `.jpeg`, `.jpg`, `.gif`, `.svg`)
- **Ejecute`image_optim`** para comprimir y optimizar imágenes rasterizadas (`.png`, `.jpeg`, `.jpg`, `.gif`)
- **Volver a almacenar en zona intermedia las imágenes optimizadas** automáticamente
- **Asegúrese de que todas las imágenes rasterizadas confirmadas** estén optimizadas correctamente
- **Compruebe los SVG clasificados** con un límite de tamaño y anule la confirmación si se hace referencia a un SVG sobredimensionado desde cualquier archivo en `help/` (de lo contrario, solo advierta)

## Ventajas

- Tamaño de repositorio reducido
- Cargas de página más rápidas para la documentación
- Calidad de imagen coherente en todos los colaboradores
- No se requiere optimización manual

## Requisitos previos

- Ruby 3.0 o superior
- Paquete
- Git

## Configurar

### Configuración automática (recomendada)

```bash
.githooks/setup-hooks.sh
```

### Configuración manual

```bash
git config core.hooksPath .githooks
chmod +x .githooks/*
```

### Configuración completa del proyecto

1. Clone el repositorio:

   ```bash
   git clone <repository-url>
   cd commerce-admin.en
   ```

2. Habilitar vínculos previos a la confirmación:

   ```bash
   .githooks/setup-hooks.sh
   ```

3. Instale las dependencias de Jekyll:

   ```bash
   cd _jekyll
   bundle install
   ```

## Comprobación de los ganchos

1. Añadir un archivo de imagen al repositorio
2. Escenario: `git add <image-file>`
3. Intentar confirmar: `git commit -m 'test'`
4. El gancho debe optimizar automáticamente la imagen

### Resultado esperado

```bash
Found 1 staged image(s). Running optimization...

Checking images ...
path/to/your/image.png    100.00%
Pre-commit image checks complete!
```

### Pruebas unitarias

La lógica de detección de vínculos de SVG del gancho (que decide si se hace referencia a un SVG de gran tamaño desde `help/`) está cubierta por pruebas unitarias que solo necesitan el paquete de Ruby `minitest` — sin gemas o configuración de `_jekyll`:

```bash
ruby .githooks/test/svg_link_checker_test.rb
```

## Directrices de imagen

- **PNG**: se usará para capturas de pantalla y elementos de la interfaz de usuario (se optimizará automáticamente)
- **JPEG**: se usará para fotografías (se optimizará automáticamente)
- **GIF**: úselo para animaciones (se optimizará automáticamente)
- **SVG**: se usa para iconos y gráficos simples (no optimizados, pero contrastados con un límite de tamaño; la confirmación falla solo si el SVG sobredimensionado está vinculado desde `help/`)

Los vínculos previos a la confirmación optimizarán automáticamente las imágenes de `.png`, `.jpeg`/`.jpg` y `.gif` en la confirmación, y comprobarán los SVG clasificados con un límite de tamaño (140 KB).

Si una SVG preconfigurada supera el límite y se hace referencia a ella desde un archivo de `help/`, se anula la confirmación. Si no se hace referencia al SVG de gran tamaño en `help/`, el vínculo solo imprime una advertencia y la confirmación continúa. Convierta SVG de gran tamaño a PNG en su lugar:

```bash
cd _jekyll
bundle exec rake images:svg_to_png path=../help/assets/image.svg
```

La ruta de acceso es relativa a `_jekyll`, por lo que se hace referencia a las imágenes de menos de `help/` como `../help/...`.

## Optimización manual

Para la optimización manual de imágenes:

```bash
cd _jekyll
bundle exec rake images:optimize path=../path/to/images
```

## Configuración

Los vínculos utilizan el archivo de configuración `_jekyll/.image_optim.yml` para personalizar la configuración de optimización:

- **PNG**: Utiliza `advpng`, `optipng` y `pngquant`
- **JPEG**: Utiliza `jhead`, `jpegoptim` y `jpegtran`
- **GIF**: Utiliza `gifsicle`
- **SVG**: no optimizado (excluido de `image_optim` para conservar gráficos vectoriales y animaciones), pero comparado con un límite de tamaño de 140 KB

## Resolución de problemas

### El enlace no se está ejecutando

- Comprobar configuración de gancho: `git config core.hooksPath`
- Asegúrese de que el archivo de enlace es ejecutable: `chmod +x .githooks/pre-commit`
- Compruebe que está en el repositorio correcto con el directorio `_jekyll`

### Errores de optimización

- Verificar que `bundle install` se haya ejecutado en el directorio `_jekyll`
- Compruebe que la joya `adobe-comdox-exl-rake-tasks` esté instalada (proporciona las tareas de rastrillo `images:optimize`, `images:check_size` y `images:svg_to_png` en las que se ejecuta el enlace)
- Revisar el archivo de configuración `.image_optim.yml`

### SVG supera el límite de tamaño

- La confirmación se anula si un SVG de ensayo excede los 140 KB y se hace referencia a él desde un archivo de `help/` (de lo contrario, el vínculo solo advierte y la confirmación continúa)
- Convertir el SVG a PNG: `cd _jekyll && bundle exec rake images:svg_to_png path=../help/assets/image.svg` (la ruta de acceso es relativa a `_jekyll`, por lo que las imágenes de menos de `help/` son referenciadas como `../help/...`)
- A continuación, coloque en zona intermedia el PNG en lugar del SVG y confirme de nuevo

### Problemas de rendimiento

- Ajustar el número de subprocesos en `_jekyll/.image_optim.yml`
- Establecer la variable de entorno `DEBUG=1` para obtener información detallada sobre el error

## Cómo funciona

1. **déclencheur previo a la confirmación**: Cuando ejecuta `git commit`, el vínculo se ejecuta automáticamente
2. **Detección de imágenes**: analiza los archivos clasificados en busca de extensiones de imagen
3. **Optimización**: Ejecuta `image_optim` en cada PNG, JPEG o GIF clasificados
4. **Reensayo**: vuelve a agregar automáticamente las imágenes optimizadas al área de ensayo
5. **Comprobación de tamaño de SVG**: comprueba cada SVG ensayado con respecto al límite de tamaño de 140 KB
6. **Confirmar ganancias**: Si la optimización se realiza correctamente y no se hace referencia a un SVG sobredimensionado desde `help/`, la confirmación continúa normalmente; de lo contrario, se anula la confirmación (un SVG sobredimensionado al que no se hace referencia desde `help/` solo genera un déclencheur de advertencia)

## Formatos de imagen compatibles

- **PNG** (`.png`): compresión sin pérdidas y con pérdidas
- **JPEG** (`.jpg`, `.jpeg`): compresión con pérdidas con limpieza de metadatos
- **GIF** (`.gif`): animación y optimización estática
- **SVG** (`.svg`) - No optimizado (confirmar tal cual para conservar la calidad), pero comparado con un límite de tamaño de 140 KB; la confirmación se anula si se supera el límite y se hace referencia a SVG desde `help/` (de lo contrario, el vínculo solo lo advierte)

## Prácticas recomendadas

1. **Probar el vínculo**: Intente confirmar primero una imagen pequeña para asegurarse de que funciona
2. **Revisar cambios**: compruebe la diferencia de Git para ver los resultados de la optimización
3. **Rendimiento del monitor**: Las imágenes grandes pueden tardar un tiempo en optimizarse
4. **Control de versiones**: Los vínculos se almacenan en este directorio `.githooks/`

## Asistencia

Para problemas con los vínculos previos a la confirmación:

1. Compruebe la salida del gancho para ver si hay mensajes de error
2. Verifique que la configuración de `image_optim` funcione
3. Pruebe primero las tareas de rastrillado manual
4. Revise los registros de enlace y la configuración
5. Compruebe la configuración del vínculo: `git config core.hooksPath`
