---
source-git-commit: f75dbbc9726ff21a648b1bb3d8f06cb45a5d6b08
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 0%

---
# Rastrear archivos de biblioteca

Este directorio contiene definiciones de tareas de rastrillo organizadas por funcionalidad. Rake carga automáticamente todos los archivos de `.rake` en este directorio.

## Organización de archivos

### `adobe-docs-tasks.rake`

Contiene requisitos comunes, funcionalidad compartida y tareas sin espacio de nombres para Adobe Commerce en las tareas de rastrillo del repositorio de documentación de Experience League:

- `whatsnew` - Generar datos para compendio de noticias (predeterminado: desde la última actualización)
- `render`: procesar archivos con plantilla y mantener las inclusiones

### `includes.rake`

Contiene tareas de administración de inclusión organizadas en el espacio de nombres `:includes`:

- `includes:maintain_relationships`: detectar y mantener relaciones de inclusión en archivos Markdown
- `includes:maintain_timestamps` - Agregar/actualizar marcas de tiempo basadas en cambios de archivo de inclusión
- `includes:maintain_all` - Ejecutar ambas operaciones en secuencia
- `includes:unused` - Buscar archivos de inclusión no utilizados

### `images.rake`

Contiene tareas de administración de imágenes organizadas en el espacio de nombres `:images`:

- `images:optimize` - Optimizar imágenes en archivos no confirmados modificados
- `images:unused` - Buscar imágenes no utilizadas en el proyecto

## Cómo funciona

Rake detecta y carga automáticamente cualquier archivo de `.rake` en el directorio `rakelib/`. Esto nos permite:

1. **Organizar tareas por funcionalidad** - Agrupar tareas relacionadas
2. **Mantener limpio el archivo de clasificación principal**: Céntrese en las tareas principales del proyecto
3. **Agregue fácilmente nuevos grupos de tareas** - Simplemente cree nuevos `.rake` archivos
4. **Mantener separación de preocupaciones** - Cada archivo administra un dominio específico

## Adición de nuevos grupos de tareas

Para agregar un nuevo grupo de tareas relacionadas:

1. Crear un nuevo archivo de `.rake` en este directorio
2. Utilice un área de nombres descriptiva (por ejemplo, `namespace :images` para tareas relacionadas con imágenes)
3. Definir las tareas dentro del área de nombres
4. Rake las cargará automáticamente

## Estructura de ejemplo

```ruby
namespace :your_namespace do
  desc 'Description of what this task does'
  task :task_name do
    # Task implementation
  end
end
```

## Uso

Las tareas están disponibles inmediatamente después de la creación:

```bash
rake your_namespace:task_name
```

## Ventajas

- **Modularidad**: cada archivo se centra en un área específica
- **Mantenimiento**: más fácil de encontrar y modificar grupos de tareas específicos
- **Escalabilidad**: puede agregar muchos grupos de tareas sin necesidad de agrupar el archivo de clasificación principal
- **Desarrollo de equipo**: distintos desarrolladores pueden trabajar en diferentes grupos de tareas
