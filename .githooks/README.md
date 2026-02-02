---
source-git-commit: ee2ed3a78939af36572f0baa6672b2e9f9a2557f
workflow-type: tm+mt
source-wordcount: '417'
ht-degree: 0%

---
# Enlaces previos a la confirmación para la optimización de imágenes

Este directorio contiene enlaces previos a la confirmación que optimizan automáticamente las imágenes antes de enviarlas al repositorio.

## Qué hacen los ganchos

- **Detectar automáticamente** archivos de imagen clasificados (PNG, JPG, JPEG, GIF)
- **Ejecutar`image_optim`** para comprimir y optimizar imágenes
- **Volver a almacenar en zona intermedia las imágenes optimizadas** automáticamente
- **Asegúrese de que todas las imágenes confirmadas** estén optimizadas correctamente

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
Optimizing: path/to/your/image.png
Re-staged optimized image: path/to/your/image.png
Image optimization complete!
```

## Directrices de imagen

- **PNG**: se usará para capturas de pantalla y elementos de la interfaz de usuario (se optimizará automáticamente)
- **JPEG**: se usará para fotografías (se optimizará automáticamente)
- **GIF**: úselo para animaciones (se optimizará automáticamente)
- **SVG**: se usa para iconos y gráficos simples (no procesados por enlaces, confirmar tal cual)

Los enlaces previos a la confirmación optimizan automáticamente las imágenes PNG, JPEG y GIF en la confirmación.

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
- **SVG**: no procesado (excluido de la detección para conservar gráficos vectoriales y animaciones)

## Resolución de problemas

### El enlace no se está ejecutando

- Comprobar configuración de gancho: `git config core.hooksPath`
- Asegúrese de que el archivo de enlace es ejecutable: `chmod +x .githooks/pre-commit`
- Compruebe que está en el repositorio correcto con el directorio `_jekyll`

### Errores de optimización

- Verificar que `bundle install` se haya ejecutado en el directorio `_jekyll`
- Compruebe que la joya `adobe-comdox-exl-rake-tasks` esté instalada (proporciona `image_optim`)
- Revisar el archivo de configuración `.image_optim.yml`

### Problemas de rendimiento

- Ajustar el número de subprocesos en `_jekyll/.image_optim.yml`
- Establecer la variable de entorno `DEBUG=1` para obtener información detallada sobre el error

## Cómo funciona

1. **déclencheur previo a la confirmación**: Cuando ejecuta `git commit`, el vínculo se ejecuta automáticamente
2. **Detección de imágenes**: analiza los archivos clasificados en busca de extensiones de imagen
3. **Optimización**: Ejecuta `image_optim` en cada imagen preconfigurada
4. **Reensayo**: vuelve a agregar automáticamente las imágenes optimizadas al área de ensayo
5. **Procedimientos de confirmación**: Si la optimización se realiza correctamente, la confirmación continúa de forma normal

## Formatos de imagen compatibles

- **PNG** (`.png`): compresión sin pérdidas y con pérdidas
- **JPEG** (`.jpg`, `.jpeg`): compresión con pérdidas con limpieza de metadatos
- **GIF** (`.gif`): animación y optimización estática
- **SVG** (`.svg`): no procesado por vínculos (confirmar tal cual para conservar la calidad)

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
