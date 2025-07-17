---
source-git-commit: 9166b44ae53e8cfc6b8022730a6b91406ba696c0
workflow-type: tm+mt
source-wordcount: '13341'
ht-degree: 0%

---
# magento-cloud (Adobe Commerce en infraestructura en la nube)

<!-- The template to render with above values -->

**Versión**: 1.46.1

Esta referencia contiene 119 comandos disponibles mediante la herramienta de línea de comandos `magento-cloud`.
La lista inicial se genera automáticamente mediante el comando `magento-cloud list` en Adobe Commerce en la infraestructura en la nube.

## General

Esta referencia se genera a partir del código base de la aplicación. Para cambiar el contenido, puede actualizar el código fuente de la implementación de comandos correspondiente en el repositorio [codebase](https://github.com/magento/magento-cloud-cli) y enviar los cambios para su revisión. Otra forma es _Proporcionarnos comentarios_ (busque el vínculo en la esquina superior derecha). Para ver las directrices de contribución, consulte [Contribuciones de código](https://developer.adobe.com/commerce/contributor/guides/code-contributions/).

### Opciones globales

#### `--help`, `-h`

Mostrar este mensaje de ayuda

- Predeterminado: `false`
- No acepta un valor

#### `--verbose`, `-v|-vv|-vvv`

Aumentar el detalle de los mensajes

- Predeterminado: `false`
- No acepta un valor

#### `--version`, `-V`

Mostrar esta versión de la aplicación

- Predeterminado: `false`
- No acepta un valor

#### `--yes`, `-y`

Responder &quot;sí&quot; a las preguntas de confirmación; aceptar el valor predeterminado para otras preguntas; deshabilitar la interacción

- Predeterminado: `false`
- No acepta un valor

#### `--no-interaction`

No haga ninguna pregunta interactiva; acepte los valores predeterminados. Equivale a utilizar la variable de entorno: MAGENTO_CLOUD_CLI_NO_INTERACTION=1

- Predeterminado: `false`
- No acepta un valor


## `clear-cache`

```bash
magento-cloud cc
```

Borre la caché de CLI

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).


## `decode`

```bash
magento-cloud decode [-P|--property PROPERTY] [--] <value>
```

Descodificar una cadena codificada como MAGENTO_CLOUD_VARIABLES

### Argumentos

#### `value`

Valor de la variable que se va a descodificar

- Requerido

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--property`, `-P`

La propiedad que se verá en la variable

- Requiere un valor


## `docs`

```bash
magento-cloud docs [--browser BROWSER] [--pipe] [--] [<search>]...
```

Abra la documentación en línea

### Argumentos

#### `search`

Término(s) de búsqueda

- Predeterminado: `[]`
- Matriz

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--browser`

Explorador que se utiliza para abrir la dirección URL. Establezca 0 para ninguno.

- Requiere un valor

#### `--pipe`

Envíe la URL a stdout.

- Predeterminado: `false`
- No acepta un valor


## `help`

```bash
magento-cloud help [--format FORMAT] [--raw] [--] [<command_name>]
```

Muestra la Ayuda de un comando

```
The help command displays help for a given command:

  magento-cloud help list

You can also output the help in other formats by using the --format option:

  magento-cloud help --format=json list

To display the list of available commands, please use the list command.
```

### Argumentos

#### `command_name`

El nombre del comando

- Predeterminado: `help`

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--format`

El formato de salida (txt, json o md)

- Predeterminado: `txt`
- Requiere un valor

#### `--raw`

Para generar la ayuda del comando raw

- Predeterminado: `false`
- No acepta un valor


## `list`

```bash
magento-cloud list [--raw] [--format FORMAT] [--all] [--] [<namespace>]
```

Comandos de listas

```
The list command lists all commands:

  magento-cloud list

You can also display the commands for a specific namespace:

  magento-cloud list project

You can also output the information in other formats by using the --format option:

  magento-cloud list --format=xml

It's also possible to get raw list of commands (useful for embedding command runner):

  magento-cloud list --raw
```

### Argumentos

#### `command`

El comando que se va a ejecutar

- Requerido


#### `namespace`

El nombre del área de nombres

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--raw`

Para generar la lista de comandos raw

- Predeterminado: `false`
- No acepta un valor

#### `--format`

El formato de salida (txt, xml, json o md)

- Predeterminado: `txt`
- Requiere un valor

#### `--all`

Mostrar todos los comandos, incluidos los ocultos

- Predeterminado: `false`
- No acepta un valor


## `multi`

```bash
magento-cloud multi [-p|--projects PROJECTS] [--continue] [--sort SORT] [--reverse] [--] <cmd> (<cmd>)...
```

Ejecutar un comando en varios proyectos

### Argumentos

#### `cmd`

El comando que se va a ejecutar

- Predeterminado: `[]`
- Requerido

- Matriz

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--projects`, `-p`

Una lista de ID de proyecto, separados por comas o espacios en blanco

- Requiere un valor

#### `--continue`

Continuar ejecutando comandos incluso si se encuentra una excepción

- Predeterminado: `false`
- No acepta un valor

#### `--sort`

Propiedad por la que se ordena la lista de opciones del proyecto

- Predeterminado: `title`
- Requiere un valor

#### `--reverse`

Invertir el orden de las opciones del proyecto

- Predeterminado: `false`
- No acepta un valor


## `web`

```bash
magento-cloud web [--browser BROWSER] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

Abra el proyecto en la interfaz de usuario web

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--browser`

Explorador que se utiliza para abrir la dirección URL. Establezca 0 para ninguno.

- Requiere un valor

#### `--pipe`

Envíe la URL a stdout.

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor


## `activity:cancel`

```bash
magento-cloud activity:cancel [-t|--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [-a|--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<id>]
```

Cancelar una actividad

### Argumentos

#### `id`

El ID de actividad. El valor predeterminado es la actividad cancelable más reciente.

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--type`, `-t`

Filtre por tipo (al seleccionar una actividad predeterminada). Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco. Los caracteres % o * se pueden usar como comodín para el tipo, p. ej. &#39;%var%&#39; para seleccionar actividades relacionadas con variables.

- Predeterminado: `[]`
- Requiere un valor

#### `--exclude-type`, `-x`

Excluir por tipo (al seleccionar una actividad predeterminada). Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco. Los caracteres % o * se pueden usar como comodín para excluir tipos.

- Predeterminado: `[]`
- Requiere un valor

#### `--all`, `-a`

Compruebe las actividades recientes en todos los entornos (al seleccionar una actividad predeterminada)

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor


## `activity:get`

```bash
magento-cloud activity:get [-P|--property PROPERTY] [-t|--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [--state STATE] [--result RESULT] [-i|--incomplete] [-a|--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [--] [<id>]
```

Ver información detallada sobre una sola actividad

### Argumentos

#### `id`

El ID de actividad. El valor predeterminado es la actividad más reciente.

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--property`, `-P`

La propiedad que se va a ver

- Requiere un valor

#### `--type`, `-t`

Filtre por tipo (al seleccionar una actividad predeterminada). Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco. Los caracteres % o * se pueden usar como comodín para el tipo, p. ej. &#39;%var%&#39; para seleccionar actividades relacionadas con variables.

- Predeterminado: `[]`
- Requiere un valor

#### `--exclude-type`, `-x`

Excluir por tipo (al seleccionar una actividad predeterminada). Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco. Los caracteres % o * se pueden usar como comodín para excluir tipos.

- Predeterminado: `[]`
- Requiere un valor

#### `--state`

Filtrar por estado (al seleccionar una actividad predeterminada): en curso, pendiente, completo o cancelado. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--result`

Filtrar por resultado (al seleccionar una actividad predeterminada): éxito o error

- Requiere un valor

#### `--incomplete`, `-i`

Incluya solo las actividades incompletas (al seleccionar una actividad predeterminada). Es la abreviatura de —state=in_progress,pending

- Predeterminado: `false`
- No acepta un valor

#### `--all`, `-a`

Compruebe las actividades recientes en todos los entornos (al seleccionar una actividad predeterminada)

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--format`

El formato de salida: tabla, csv, tsv o sin formato

- Predeterminado: `table`
- Requiere un valor

#### `--columns`, `-c`

Columnas para mostrar. Los caracteres % o * se pueden usar como comodín. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--no-header`

No generar el encabezado de tabla

- Predeterminado: `false`
- No acepta un valor

#### `--date-fmt`

El formato de fecha (como una cadena de formato de fecha PHP)

- Predeterminado: `c`
- Requiere un valor


## `activity:list`

```bash
magento-cloud activities [-t|--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [--limit LIMIT] [--start START] [--state STATE] [--result RESULT] [-i|--incomplete] [-a|--all] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

Obtener una lista de actividades de un entorno o proyecto

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--type`, `-t`

Filtrar actividades por tipo Los valores se pueden dividir por comas (por ejemplo, &quot;a,b,c&quot;) o espacios en blanco. Se puede omitir la primera parte del nombre de la actividad; por ejemplo, &quot;cron&quot; puede seleccionar actividades &quot;environment.cron&quot;. Los caracteres % o * se pueden usar como comodín, p. ej. &#39;%var%&#39; para seleccionar actividades relacionadas con variables.

- Predeterminado: `[]`
- Requiere un valor

#### `--exclude-type`, `-x`

Excluir actividades por tipo. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco. Se puede omitir la primera parte del nombre de la actividad; por ejemplo, &quot;cron&quot; puede excluir las actividades &quot;environment.cron&quot;. Los caracteres % o * se pueden usar como comodín para excluir tipos.

- Predeterminado: `[]`
- Requiere un valor

#### `--limit`

Limitar el número de resultados mostrados

- Predeterminado: `10`
- Requiere un valor

#### `--start`

Solo se enumerarán las actividades creadas antes de esta fecha

- Requiere un valor

#### `--state`

Filtrar actividades por estado: en curso, pendiente, completo o cancelado. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--result`

Filtrado de actividades por resultado: éxito o fracaso

- Requiere un valor

#### `--incomplete`, `-i`

Enumerar solo las actividades incompletas

- Predeterminado: `false`
- No acepta un valor

#### `--all`, `-a`

Enumerar actividades en todos los entornos

- Predeterminado: `false`
- No acepta un valor

#### `--format`

El formato de salida: tabla, csv, tsv o sin formato

- Predeterminado: `table`
- Requiere un valor

#### `--columns`, `-c`

Columnas para mostrar. Columnas disponibles: id*, created*, description*, progress*, state*, result*, completed, environment, type (* = columnas predeterminadas). El carácter &quot;+&quot; se puede utilizar como marcador de posición para las columnas predeterminadas. Los caracteres % o * se pueden usar como comodín. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--no-header`

No generar el encabezado de tabla

- Predeterminado: `false`
- No acepta un valor

#### `--date-fmt`

El formato de fecha (como una cadena de formato de fecha PHP)

- Predeterminado: `c`
- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor


## `activity:log`

```bash
magento-cloud activity:log [--refresh REFRESH] [-t|--timestamps] [--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [--state STATE] [--result RESULT] [-i|--incomplete] [-a|--all] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<id>]
```

Mostrar el registro de una actividad

### Argumentos

#### `id`

El ID de actividad. El valor predeterminado es la actividad más reciente.

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--refresh`

Intervalo de actualización de la actividad (segundos). Establezca el valor en 0 para deshabilitar la actualización.

- Predeterminado: `3`
- Requiere un valor

#### `--timestamps`, `-t`

Mostrar una marca de tiempo junto a cada mensaje

- Predeterminado: `false`
- No acepta un valor

#### `--type`

Filtre por tipo (al seleccionar una actividad predeterminada). Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco. Los caracteres % o * se pueden usar como comodín para el tipo, p. ej. &#39;%var%&#39; para seleccionar actividades relacionadas con variables.

- Predeterminado: `[]`
- Requiere un valor

#### `--exclude-type`, `-x`

Excluir por tipo (al seleccionar una actividad predeterminada). Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco. Los caracteres % o * se pueden usar como comodín para excluir tipos.

- Predeterminado: `[]`
- Requiere un valor

#### `--state`

Filtrar por estado (al seleccionar una actividad predeterminada): en curso, pendiente, completo o cancelado. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--result`

Filtrar por resultado (al seleccionar una actividad predeterminada): éxito o error

- Requiere un valor

#### `--incomplete`, `-i`

Incluya solo las actividades incompletas (al seleccionar una actividad predeterminada). Es la abreviatura de —state=in_progress,pending

- Predeterminado: `false`
- No acepta un valor

#### `--all`, `-a`

Compruebe las actividades recientes en todos los entornos (al seleccionar una actividad predeterminada)

- Predeterminado: `false`
- No acepta un valor

#### `--date-fmt`

El formato de fecha (como una cadena de formato de fecha PHP)

- Predeterminado: `c`
- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor


## `app:config-get`

```bash
magento-cloud app:config-get [-P|--property PROPERTY] [--refresh] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-i|--identity-file IDENTITY-FILE]
```

Ver la configuración de una aplicación

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--property`, `-P`

La propiedad de configuración que se va a ver

- Requiere un valor

#### `--refresh`

Si se actualiza la caché

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--app`, `-A`

El nombre de la aplicación remota

- Requiere un valor

#### `--identity-file`, `-i`

[Opción obsoleta, ya no se utiliza]

- Requiere un valor


## `app:list`

```bash
magento-cloud apps [--refresh] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

Enumerar aplicaciones en el proyecto

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--refresh`

Si se actualiza la caché

- Predeterminado: `false`
- No acepta un valor

#### `--pipe`

Mostrar solo una lista de nombres de aplicaciones

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--format`

El formato de salida: tabla, csv, tsv o sin formato

- Predeterminado: `table`
- Requiere un valor

#### `--columns`, `-c`

Columnas para mostrar. Columnas disponibles: nombre*, tipo*, disco, ruta, tamaño (* = columnas predeterminadas). El carácter &quot;+&quot; se puede utilizar como marcador de posición para las columnas predeterminadas. Los caracteres % o * se pueden usar como comodín. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--no-header`

No generar el encabezado de tabla

- Predeterminado: `false`
- No acepta un valor


## `auth:api-token-login`

```bash
magento-cloud auth:api-token-login
```

Inicie sesión en Magento Cloud mediante un token de API

```
Use this command to log in to your Magento Cloud account using an API token.

You can create an account at:
    https://business.adobe.com/products/magento/magento-commerce.html

If you have an account, but you do not already have an API token, you can create one here:
    https://accounts.magento.cloud/user/api-tokens

Alternatively, to log in to the CLI with a browser, run:
    magento-cloud auth:browser-login
```

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).


## `auth:browser-login`

```bash
magento-cloud login [-f|--force] [--browser BROWSER] [--pipe]
```

Inicie sesión en Magento Cloud mediante un explorador

```
Use this command to log in to the Magento Cloud CLI using a web browser.

It launches a temporary local website which redirects you to log in if
necessary, and then captures the resulting authorization code.

Your system's default browser will be used. You can override this using the
--browser option.

Alternatively, to log in using an API token (without a browser), run:
magento-cloud auth:api-token-login

To authenticate non-interactively, configure an API token using the
MAGENTO_CLOUD_CLI_TOKEN environment variable.
```

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--force`, `-f`

Inicie sesión de nuevo, incluso si ya ha iniciado sesión

- Predeterminado: `false`
- No acepta un valor

#### `--browser`

Explorador que se utiliza para abrir la dirección URL. Establezca 0 para ninguno.

- Requiere un valor

#### `--pipe`

Envíe la URL a stdout.

- Predeterminado: `false`
- No acepta un valor


## `auth:info`

```bash
magento-cloud auth:info [--no-auto-login] [-P|--property PROPERTY] [--refresh] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--] [<property>]
```

Mostrar la información de la cuenta

### Argumentos

#### `property`

Propiedad de la cuenta que se va a ver

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--no-auto-login`

Omite el inicio de sesión automático. No se generará nada si no se inicia sesión, y el código de salida será 0, suponiendo que no haya otros errores.

- Predeterminado: `false`
- No acepta un valor

#### `--property`, `-P`

La propiedad de cuenta que se va a ver (sintaxis alternativa)

- Requiere un valor

#### `--refresh`

Si se actualiza la caché

- Predeterminado: `false`
- No acepta un valor

#### `--format`

El formato de salida: tabla, csv, tsv o sin formato

- Predeterminado: `table`
- Requiere un valor

#### `--columns`, `-c`

Columnas para mostrar. Los caracteres % o * se pueden usar como comodín. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--no-header`

No generar el encabezado de tabla

- Predeterminado: `false`
- No acepta un valor


## `auth:logout`

```bash
magento-cloud logout [-a|--all] [--other]
```

Cerrar sesión en Magento Cloud

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--all`, `-a`

Cerrar sesión en todas las sesiones locales

- Predeterminado: `false`
- No acepta un valor

#### `--other`

Cerrar sesión en otras sesiones locales

- Predeterminado: `false`
- No acepta un valor


## `blackfire:setup`

```bash
magento-cloud blackfire:setup [--server_id SERVER_ID] [--server_token SERVER_TOKEN] [-p|--project PROJECT] [-W|--no-wait] [--wait]
```

Configure la integración de Blackfire.io para el proyecto

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--server_id`

El ID del servidor

- Requiere un valor

#### `--server_token`

El token del servidor

- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--no-wait`, `-W`

No espere a que se complete la operación

- Predeterminado: `false`
- No acepta un valor

#### `--wait`

Espere a que se complete la operación (valor predeterminado)

- Predeterminado: `false`
- No acepta un valor


## `certificate:add`

```bash
magento-cloud certificate:add [--cert CERT] [--key KEY] [--chain CHAIN] [-p|--project PROJECT] [-W|--no-wait] [--wait]
```

Añadir un certificado SSL al proyecto

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--cert`

La ruta al archivo de certificado

- Requiere un valor

#### `--key`

Ruta al archivo de clave privada del certificado

- Requiere un valor

#### `--chain`

La ruta al archivo de cadena de certificados

- Predeterminado: `[]`
- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--no-wait`, `-W`

No espere a que se complete la operación

- Predeterminado: `false`
- No acepta un valor

#### `--wait`

Espere a que se complete la operación (valor predeterminado)

- Predeterminado: `false`
- No acepta un valor


## `certificate:delete`

```bash
magento-cloud certificate:delete [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] <id>
```

Eliminar un certificado del proyecto

### Argumentos

#### `id`

El ID del certificado (o el inicio)

- Requerido

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--no-wait`, `-W`

No espere a que se complete la operación

- Predeterminado: `false`
- No acepta un valor

#### `--wait`

Espere a que se complete la operación (valor predeterminado)

- Predeterminado: `false`
- No acepta un valor


## `certificate:get`

```bash
magento-cloud certificate:get [-P|--property PROPERTY] [--date-fmt DATE-FMT] [-p|--project PROJECT] [--] <id>
```

Ver un certificado

### Argumentos

#### `id`

El ID del certificado (o el inicio)

- Requerido

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--property`, `-P`

La propiedad de certificado que se va a ver

- Requiere un valor

#### `--date-fmt`

El formato de fecha (como una cadena de formato de fecha PHP)

- Predeterminado: `c`
- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor


## `certificate:list`

```bash
magento-cloud certificates [--domain DOMAIN] [--exclude-domain EXCLUDE-DOMAIN] [--issuer ISSUER] [--only-auto] [--no-auto] [--ignore-expiry] [--only-expired] [--no-expired] [--pipe-domains] [--date-fmt DATE-FMT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT]
```

Enumerar certificados de proyecto

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--domain`

Filtrar por nombre de dominio (búsqueda sin distinción de mayúsculas y minúsculas)

- Requiere un valor

#### `--exclude-domain`

Excluir certificados, coincidencia por nombre de dominio (búsqueda sin distinción de mayúsculas y minúsculas)

- Requiere un valor

#### `--issuer`

Filtrar por emisor

- Requiere un valor

#### `--only-auto`

Mostrar solo los certificados aprovisionados automáticamente

- Predeterminado: `false`
- No acepta un valor

#### `--no-auto`

Mostrar solo los certificados añadidos manualmente

- Predeterminado: `false`
- No acepta un valor

#### `--ignore-expiry`

Mostrar certificados caducados y no caducados

- Predeterminado: `false`
- No acepta un valor

#### `--only-expired`

Mostrar solo los certificados caducados

- Predeterminado: `false`
- No acepta un valor

#### `--no-expired`

Mostrar solo los certificados no caducados (predeterminado)

- Predeterminado: `false`
- No acepta un valor

#### `--pipe-domains`

Devolver solo una lista de nombres de dominio cubiertos por los certificados

- Predeterminado: `false`
- No acepta un valor

#### `--date-fmt`

El formato de fecha (como una cadena de formato de fecha PHP)

- Predeterminado: `c`
- Requiere un valor

#### `--format`

El formato de salida: tabla, csv, tsv o sin formato

- Predeterminado: `table`
- Requiere un valor

#### `--columns`, `-c`

Columnas para mostrar. Columnas disponibles: creado, dominios, caduca, id, emisor. Los caracteres % o * se pueden usar como comodín. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--no-header`

No generar el encabezado de tabla

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor


## `commit:get`

```bash
magento-cloud commit:get [-P|--property PROPERTY] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--date-fmt DATE-FMT] [--] [<commit>]
```

Mostrar detalles de confirmación

### Argumentos

#### `commit`

El SHA de compromiso. Esto también puede aceptar sufijos de &quot;HEAD&quot; y acento circunflejo (^) o tilde (~) para confirmaciones principales.

- Predeterminado: `HEAD`

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--property`, `-P`

La propiedad commit que se va a mostrar.

- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--date-fmt`

El formato de fecha (como una cadena de formato de fecha PHP)

- Predeterminado: `c`
- Requiere un valor


## `commit:list`

```bash
magento-cloud commits [--limit LIMIT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [--] [<commit>]
```

Enumerar confirmaciones

### Argumentos

#### `commit`

El SHA de confirmación de Git de inicio. Esto también puede aceptar sufijos de &quot;HEAD&quot; y acento circunflejo (^) o tilde (~) para confirmaciones principales.

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--limit`

Número de confirmaciones que se van a mostrar.

- Predeterminado: `10`
- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--format`

El formato de salida: tabla, csv, tsv o sin formato

- Predeterminado: `table`
- Requiere un valor

#### `--columns`, `-c`

Columnas para mostrar. Columnas disponibles: autor, fecha, sha, resumen. Los caracteres % o * se pueden usar como comodín. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--no-header`

No generar el encabezado de tabla

- Predeterminado: `false`
- No acepta un valor

#### `--date-fmt`

El formato de fecha (como una cadena de formato de fecha PHP)

- Predeterminado: `c`
- Requiere un valor


## `db:dump`

```bash
magento-cloud db:dump [--schema SCHEMA] [-f|--file FILE] [-d|--directory DIRECTORY] [-z|--gzip] [-t|--timestamp] [-o|--stdout] [--table TABLE] [--exclude-table EXCLUDE-TABLE] [--schema-only] [--charset CHARSET] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE]
```

Crear un volcado local de la base de datos remota

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--schema`

Esquema que se va a volcar. Omita utilizar el esquema predeterminado (normalmente &quot;main&quot;).

- Requiere un valor

#### `--file`, `-f`

Un nombre de archivo personalizado para el volcado

- Requiere un valor

#### `--directory`, `-d`

Un directorio personalizado para el volcado

- Requiere un valor

#### `--gzip`, `-z`

Comprimir el volcado utilizando gzip

- Predeterminado: `false`
- No acepta un valor

#### `--timestamp`, `-t`

Añadir una marca de tiempo al nombre del archivo de volcado

- Predeterminado: `false`
- No acepta un valor

#### `--stdout`, `-o`

Salida a STDOUT en lugar de a un archivo

- Predeterminado: `false`
- No acepta un valor

#### `--table`

Tabla(s) que incluir

- Predeterminado: `[]`
- Requiere un valor

#### `--exclude-table`

Tabla(s) que excluir

- Predeterminado: `[]`
- Requiere un valor

#### `--schema-only`

Volcar solo esquemas, sin datos

- Predeterminado: `false`
- No acepta un valor

#### `--charset`

La codificación del conjunto de caracteres del volcado

- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--app`, `-A`

El nombre de la aplicación remota

- Requiere un valor

#### `--relationship`, `-r`

La relación de servicio que se va a utilizar

- Requiere un valor

#### `--identity-file`, `-i`

Una identidad SSH (clave privada) que utilizar

- Requiere un valor


## `db:size`

```bash
magento-cloud db:size [-B|--bytes] [-C|--cleanup] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-r|--relationship RELATIONSHIP] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-i|--identity-file IDENTITY-FILE]
```

Calcular el uso de disco de una base de datos

```
This is an estimate of the database disk usage. The real size on disk is usually higher because of overhead.

To see more accurate disk usage, run: magento-cloud disk
```

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--bytes`, `-B`

Mostrar tamaños en bytes.

- Predeterminado: `false`
- No acepta un valor

#### `--cleanup`, `-C`

Comprobar si las tablas se pueden limpiar y mostrar recomendaciones (solo InnoDb).

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--app`, `-A`

El nombre de la aplicación remota

- Requiere un valor

#### `--relationship`, `-r`

La relación de servicio que se va a utilizar

- Requiere un valor

#### `--format`

El formato de salida: tabla, csv, tsv o sin formato

- Predeterminado: `table`
- Requiere un valor

#### `--columns`, `-c`

Columnas para mostrar. Columnas disponibles: max, percent_used, used. Los caracteres % o * se pueden usar como comodín. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--no-header`

No generar el encabezado de tabla

- Predeterminado: `false`
- No acepta un valor

#### `--identity-file`, `-i`

Una identidad SSH (clave privada) que utilizar

- Requiere un valor


## `db:sql`

```bash
magento-cloud sql [--raw] [--schema SCHEMA] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [--] [<query>]
```

Ejecutar SQL en la base de datos remota

### Argumentos

#### `query`

Instrucción SQL para ejecutar

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--raw`

Producir salida sin procesar y no tabular

- Predeterminado: `false`
- No acepta un valor

#### `--schema`

Esquema que se va a utilizar. Omita utilizar el esquema predeterminado (normalmente &quot;main&quot;). Pase una cadena vacía para no utilizar ningún esquema.

- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--app`, `-A`

El nombre de la aplicación remota

- Requiere un valor

#### `--relationship`, `-r`

La relación de servicio que se va a utilizar

- Requiere un valor

#### `--identity-file`, `-i`

Una identidad SSH (clave privada) que utilizar

- Requiere un valor


## `domain:add`

```bash
magento-cloud domain:add [--cert CERT] [--key KEY] [--chain CHAIN] [--attach ATTACH] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

Añadir un nuevo dominio al proyecto

### Argumentos

#### `name`

El nombre de dominio

- Requerido

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--cert`

Ruta de acceso al archivo de certificado para este dominio

- Requiere un valor

#### `--key`

Ruta de acceso al archivo de clave privada del certificado proporcionado.

- Requiere un valor

#### `--chain`

Ruta de acceso al archivo o archivos de cadena de certificados para el certificado proporcionado

- Predeterminado: `[]`
- Requiere un valor

#### `--attach`

El dominio de producción al que este sustituye en las rutas del entorno. Necesario para dominios de entorno que no son de producción.

- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--no-wait`, `-W`

No espere a que se complete la operación

- Predeterminado: `false`
- No acepta un valor

#### `--wait`

Espere a que se complete la operación (valor predeterminado)

- Predeterminado: `false`
- No acepta un valor


## `domain:delete`

```bash
magento-cloud domain:delete [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

Eliminar un dominio del proyecto

### Argumentos

#### `name`

El nombre de dominio

- Requerido

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--no-wait`, `-W`

No espere a que se complete la operación

- Predeterminado: `false`
- No acepta un valor

#### `--wait`

Espere a que se complete la operación (valor predeterminado)

- Predeterminado: `false`
- No acepta un valor


## `domain:get`

```bash
magento-cloud domain:get [-P|--property PROPERTY] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<name>]
```

Mostrar información detallada de un dominio

### Argumentos

#### `name`

El nombre de dominio

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--property`, `-P`

Propiedad de dominio que se va a ver

- Requiere un valor

#### `--format`

El formato de salida: tabla, csv, tsv o sin formato

- Predeterminado: `table`
- Requiere un valor

#### `--columns`, `-c`

Columnas para mostrar. Los caracteres % o * se pueden usar como comodín. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--no-header`

No generar el encabezado de tabla

- Predeterminado: `false`
- No acepta un valor

#### `--date-fmt`

El formato de fecha (como una cadena de formato de fecha PHP)

- Predeterminado: `c`
- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor


## `domain:list`

```bash
magento-cloud domains [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

Obtener una lista de todos los dominios

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--format`

El formato de salida: tabla, csv, tsv o sin formato

- Predeterminado: `table`
- Requiere un valor

#### `--columns`, `-c`

Columnas para mostrar. Columnas disponibles: name*, ssl*, created_at*, registered_name, replace_for, type, updated_at (* = columnas predeterminadas). El carácter &quot;+&quot; se puede utilizar como marcador de posición para las columnas predeterminadas. Los caracteres % o * se pueden usar como comodín. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--no-header`

No generar el encabezado de tabla

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor


## `domain:update`

```bash
magento-cloud domain:update [--cert CERT] [--key KEY] [--chain CHAIN] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

Actualización de un dominio

### Argumentos

#### `name`

El nombre de dominio

- Requerido

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--cert`

Ruta de acceso al archivo de certificado para este dominio

- Requiere un valor

#### `--key`

Ruta de acceso al archivo de clave privada del certificado proporcionado.

- Requiere un valor

#### `--chain`

Ruta de acceso al archivo o archivos de cadena de certificados para el certificado proporcionado

- Predeterminado: `[]`
- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--no-wait`, `-W`

No espere a que se complete la operación

- Predeterminado: `false`
- No acepta un valor

#### `--wait`

Espere a que se complete la operación (valor predeterminado)

- Predeterminado: `false`
- No acepta un valor


## `environment:activate`

```bash
magento-cloud environment:activate [--parent PARENT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<environment>]...
```

Activar un entorno

### Argumentos

#### `environment`

Los entornos que se van a activar

- Predeterminado: `[]`
- Matriz

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--parent`

Establezca un nuevo entorno principal antes de activar

- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--no-wait`, `-W`

No espere a que se complete la operación

- Predeterminado: `false`
- No acepta un valor

#### `--wait`

Espere a que se complete la operación (valor predeterminado)

- Predeterminado: `false`
- No acepta un valor


## `environment:branch`

```bash
magento-cloud branch [--title TITLE] [--type TYPE] [--no-clone-parent] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<id>] [<parent>]
```

Rama de un entorno

### Argumentos

#### `id`

El ID (nombre de rama) del nuevo entorno


#### `parent`

El elemento principal del nuevo entorno

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--title`

El título del nuevo entorno

- Requiere un valor

#### `--type`

El tipo de entorno nuevo

- Requiere un valor

#### `--no-clone-parent`

No clonar los datos del entorno principal

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--no-wait`, `-W`

No espere a que se complete la operación

- Predeterminado: `false`
- No acepta un valor

#### `--wait`

Espere a que se complete la operación (valor predeterminado)

- Predeterminado: `false`
- No acepta un valor


## `environment:checkout`

```bash
magento-cloud checkout [-i|--identity-file IDENTITY-FILE] [--] [<id>]
```

Desproteger un entorno

### Argumentos

#### `id`

El ID del entorno que se va a desproteger. Por ejemplo: &quot;sprint2&quot;

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--identity-file`, `-i`

Una identidad SSH (clave privada) que utilizar

- Requiere un valor


## `environment:delete`

```bash
magento-cloud environment:delete [--delete-branch] [--no-delete-branch] [--type TYPE] [-t|--only-type ONLY-TYPE] [--exclude EXCLUDE] [--exclude-type EXCLUDE-TYPE] [--inactive] [--merged] [--allow-delete-parent] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<environment>]...
```

Eliminar uno o más entornos

```
When a Magento Cloud environment is deleted, it will become "inactive": it will
exist only as a Git branch, containing code but no services, databases nor
files.

This command allows you to delete environments as well as their Git branches.
```

### Argumentos

#### `environment`

Los entornos que se van a eliminar. Los caracteres % o * se pueden usar como comodín. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Matriz

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--delete-branch`

Eliminar ramas Git para entornos inactivos, sin confirmación

- Predeterminado: `false`
- No acepta un valor

#### `--no-delete-branch`

No elimine ninguna rama de Git (entornos inactivos)

- Predeterminado: `false`
- No acepta un valor

#### `--type`

Eliminar todos los entornos de un tipo (añadiendo a otros seleccionados) Los valores se pueden dividir por comas (por ejemplo, &quot;a,b,c&quot;) o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--only-type`, `-t`

Solo se pueden eliminar entornos de un tipo específico. Los valores se pueden dividir por comas (por ejemplo, &quot;a,b,c&quot;) o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--exclude`

Entornos que no se van a eliminar. Los caracteres % o * se pueden usar como comodín. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--exclude-type`

Tipos de entorno de los que no se desea eliminar Los valores se pueden dividir por comas (por ejemplo, &quot;a,b,c&quot;) o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--inactive`

Eliminar todos los entornos inactivos (añadir a cualquier otro seleccionado)

- Predeterminado: `false`
- No acepta un valor

#### `--merged`

Eliminar todos los entornos combinados (añadir a cualquier otro seleccionado)

- Predeterminado: `false`
- No acepta un valor

#### `--allow-delete-parent`

Permitir que se eliminen los entornos que tienen tareas secundarias

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--no-wait`, `-W`

No espere a que se complete la operación

- Predeterminado: `false`
- No acepta un valor

#### `--wait`

Espere a que se complete la operación (valor predeterminado)

- Predeterminado: `false`
- No acepta un valor


## `environment:http-access`

```bash
magento-cloud httpaccess [--access ACCESS] [--auth AUTH] [--enabled ENABLED] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait]
```

Actualizar la configuración de acceso HTTP de un entorno

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--access`

Restricción de acceso con el formato &quot;permission:address&quot;. Utilice 0 para borrar todas las direcciones.

- Predeterminado: `[]`
- Requiere un valor

#### `--auth`

Credenciales de autenticación HTTP Basic en el formato &quot;nombre de usuario:contraseña&quot;. Utilice 0 para borrar todas las credenciales.

- Predeterminado: `[]`
- Requiere un valor

#### `--enabled`

Si el control de acceso debe habilitarse: 1 para habilitar, 0 para deshabilitar

- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--no-wait`, `-W`

No espere a que se complete la operación

- Predeterminado: `false`
- No acepta un valor

#### `--wait`

Espere a que se complete la operación (valor predeterminado)

- Predeterminado: `false`
- No acepta un valor


## `environment:info`

```bash
magento-cloud environment:info [--refresh] [--date-fmt DATE-FMT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<property>] [<value>]
```

Leer o establecer propiedades para un entorno

### Argumentos

#### `property`

El nombre de la propiedad


#### `value`

Establezca un nuevo valor para la propiedad

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--refresh`

Si se actualiza la caché

- Predeterminado: `false`
- No acepta un valor

#### `--date-fmt`

El formato de fecha (como una cadena de formato de fecha PHP)

- Predeterminado: `c`
- Requiere un valor

#### `--format`

El formato de salida: tabla, csv, tsv o sin formato

- Predeterminado: `table`
- Requiere un valor

#### `--columns`, `-c`

Columnas para mostrar. Los caracteres % o * se pueden usar como comodín. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--no-header`

No generar el encabezado de tabla

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--no-wait`, `-W`

No espere a que se complete la operación

- Predeterminado: `false`
- No acepta un valor

#### `--wait`

Espere a que se complete la operación (valor predeterminado)

- Predeterminado: `false`
- No acepta un valor


## `environment:init`

```bash
magento-cloud environment:init [--profile PROFILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <url>
```

Inicializar un entorno desde un repositorio Git público

### Argumentos

#### `url`

Una URL a un repositorio Git

- Requerido

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--profile`

El nombre del perfil

- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--no-wait`, `-W`

No espere a que se complete la operación

- Predeterminado: `false`
- No acepta un valor

#### `--wait`

Espere a que se complete la operación (valor predeterminado)

- Predeterminado: `false`
- No acepta un valor


## `environment:list`

```bash
magento-cloud environments [-I|--no-inactive] [--pipe] [--refresh REFRESH] [--sort SORT] [--reverse] [--type TYPE] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT]
```

Obtener una lista de entornos

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--no-inactive`, `-I`

No mostrar entornos inactivos

- Predeterminado: `false`
- No acepta un valor

#### `--pipe`

Envíe una lista sencilla de ID de entorno.

- Predeterminado: `false`
- No acepta un valor

#### `--refresh`

Si se actualiza la lista.

- Predeterminado: `1`
- Requiere un valor

#### `--sort`

Una propiedad por la que ordenar

- Predeterminado: `title`
- Requiere un valor

#### `--reverse`

Ordenar en orden inverso (descendente)

- Predeterminado: `false`
- No acepta un valor

#### `--type`

Filtre la lista por tipo(s) de entorno. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--format`

El formato de salida: tabla, csv, tsv o sin formato

- Predeterminado: `table`
- Requiere un valor

#### `--columns`, `-c`

Columnas para mostrar. Columnas disponibles: id*, title*, status*, type*, created, machine_name, updated (* = columnas predeterminadas). El carácter &quot;+&quot; se puede utilizar como marcador de posición para las columnas predeterminadas. Los caracteres % o * se pueden usar como comodín. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--no-header`

No generar el encabezado de tabla

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor


## `environment:logs`

```bash
magento-cloud log [--lines LINES] [--tail] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [--] [<type>]
```

Leer los registros de un entorno

### Argumentos

#### `type`

El tipo de registro, p. ej. &quot;acceso&quot; o &quot;error&quot;

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--lines`

Número de líneas que se van a mostrar

- Predeterminado: `100`
- Requiere un valor

#### `--tail`

Seguir continuamente el registro

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--app`, `-A`

El nombre de la aplicación remota

- Requiere un valor

#### `--worker`

Un nombre de trabajador

- Requiere un valor

#### `--instance`, `-I`

Un ID de instancia

- Requiere un valor


## `environment:merge`

```bash
magento-cloud merge [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<environment>]
```

Combinar un entorno

```
This command will initiate a Git merge of the specified environment into its parent environment.
```

### Argumentos

#### `environment`

El entorno para combinar

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--no-wait`, `-W`

No espere a que se complete la operación

- Predeterminado: `false`
- No acepta un valor

#### `--wait`

Espere a que se complete la operación (valor predeterminado)

- Predeterminado: `false`
- No acepta un valor


## `environment:pause`

```bash
magento-cloud environment:pause [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait]
```

Pausar un entorno

```
Pausing an environment helps to reduce resource consumption and carbon emissions.

The environment will be unavailable until it is resumed. No data will be lost.
```

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--no-wait`, `-W`

No espere a que se complete la operación

- Predeterminado: `false`
- No acepta un valor

#### `--wait`

Espere a que se complete la operación (valor predeterminado)

- Predeterminado: `false`
- No acepta un valor


## `environment:push`

```bash
magento-cloud push [--target TARGET] [-f|--force] [--force-with-lease] [-u|--set-upstream] [--activate] [--parent PARENT] [--type TYPE] [--no-clone-parent] [-W|--no-wait] [--wait] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-i|--identity-file IDENTITY-FILE] [--] [<source>]
```

Insertar código en un entorno

### Argumentos

#### `source`

La referencia de origen: un nombre de rama o hash de confirmación

- Predeterminado: `HEAD`

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--target`

Nombre de la rama de destino. El valor predeterminado es la rama actual.

- Requiere un valor

#### `--force`, `-f`

Permitir actualizaciones que no sean de avance rápido

- Predeterminado: `false`
- No acepta un valor

#### `--force-with-lease`

Permitir actualizaciones que no sean de avance rápido, si la rama de seguimiento remoto está actualizada

- Predeterminado: `false`
- No acepta un valor

#### `--set-upstream`, `-u`

Establezca el entorno de destino como flujo ascendente para la rama de origen. Esto también establece el proyecto de destino como remoto para el repositorio local.

- Predeterminado: `false`
- No acepta un valor

#### `--activate`

Activar el entorno antes de insertar

- Predeterminado: `false`
- No acepta un valor

#### `--parent`

Establecer el nuevo entorno principal (solo se usa con —activate)

- Requiere un valor

#### `--type`

Defina el tipo de entorno (solo se usa con —activate )

- Requiere un valor

#### `--no-clone-parent`

No clonar los datos de la rama principal (solo se usa con —activate)

- Predeterminado: `false`
- No acepta un valor

#### `--no-wait`, `-W`

No espere a que se complete la operación

- Predeterminado: `false`
- No acepta un valor

#### `--wait`

Espere a que se complete la operación (valor predeterminado)

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--identity-file`, `-i`

Una identidad SSH (clave privada) que utilizar

- Requiere un valor


## `environment:redeploy`

```bash
magento-cloud redeploy [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait]
```

Volver a implementar un entorno

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--no-wait`, `-W`

No espere a que se complete la operación

- Predeterminado: `false`
- No acepta un valor

#### `--wait`

Espere a que se complete la operación (valor predeterminado)

- Predeterminado: `false`
- No acepta un valor


## `environment:relationships`

```bash
magento-cloud relationships [-P|--property PROPERTY] [--refresh] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-i|--identity-file IDENTITY-FILE] [--] [<environment>]
```

Mostrar las relaciones de un entorno

### Argumentos

#### `environment`

El entorno

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--property`, `-P`

La propiedad de relación que se va a ver

- Requiere un valor

#### `--refresh`

Si se actualizan las relaciones

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--app`, `-A`

El nombre de la aplicación remota

- Requiere un valor

#### `--identity-file`, `-i`

Una identidad SSH (clave privada) que utilizar

- Requiere un valor


## `environment:resume`

```bash
magento-cloud environment:resume [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait]
```

Reanudación de un entorno en pausa

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--no-wait`, `-W`

No espere a que se complete la operación

- Predeterminado: `false`
- No acepta un valor

#### `--wait`

Espere a que se complete la operación (valor predeterminado)

- Predeterminado: `false`
- No acepta un valor


## `environment:scp`

```bash
magento-cloud scp [-r|--recursive] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [-i|--identity-file IDENTITY-FILE] [--] [<files>]...
```

Copiar archivos a y desde un entorno mediante scp

### Argumentos

#### `files`

Archivos para copiar. Utilice el prefijo remote: para definir ubicaciones remotas.

- Predeterminado: `[]`
- Matriz

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--recursive`, `-r`

Copiar directorios completos de forma recursiva

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--app`, `-A`

El nombre de la aplicación remota

- Requiere un valor

#### `--worker`

Un nombre de trabajador

- Requiere un valor

#### `--instance`, `-I`

Un ID de instancia

- Requiere un valor

#### `--identity-file`, `-i`

Una identidad SSH (clave privada) que utilizar

- Requiere un valor


## `environment:ssh`

```bash
magento-cloud ssh [--pipe] [--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [-i|--identity-file IDENTITY-FILE] [--] [<cmd>]...
```

SSH al entorno actual

### Argumentos

#### `cmd`

Un comando para ejecutar en el entorno.

- Predeterminado: `[]`
- Matriz

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--pipe`

Envíe solo la dirección URL SSH.

- Predeterminado: `false`
- No acepta un valor

#### `--all`

Mostrar todas las URL SSH (de cada aplicación).

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--app`, `-A`

El nombre de la aplicación remota

- Requiere un valor

#### `--worker`

Un nombre de trabajador

- Requiere un valor

#### `--instance`, `-I`

Un ID de instancia

- Requiere un valor

#### `--identity-file`, `-i`

Una identidad SSH (clave privada) que utilizar

- Requiere un valor


## `environment:synchronize`

```bash
magento-cloud sync [--rebase] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<synchronize>]...
```

Sincronizar el código o los datos de un entorno desde su elemento principal

```
This command synchronizes to a child environment from its parent environment.

Synchronizing "code" means there will be a Git merge from the parent to the
child. Synchronizing "data" means that all files in all services (including
static files, databases, logs, search indices, etc.) will be copied from the
parent to the child.
```

### Argumentos

#### `synchronize`

Qué sincronizar: &quot;código&quot;, &quot;datos&quot; o ambos

- Predeterminado: `[]`
- Matriz

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--rebase`

Sincronizar el código cambiando el nombre en lugar de combinarlo

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--no-wait`, `-W`

No espere a que se complete la operación

- Predeterminado: `false`
- No acepta un valor

#### `--wait`

Espere a que se complete la operación (valor predeterminado)

- Predeterminado: `false`
- No acepta un valor


## `environment:url`

```bash
magento-cloud url [-1|--primary] [--browser BROWSER] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

Obtener las direcciones URL públicas de un entorno

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--primary`, `-1`

Devolver solo la dirección URL de la ruta principal

- Predeterminado: `false`
- No acepta un valor

#### `--browser`

Explorador que se utiliza para abrir la dirección URL. Establezca 0 para ninguno.

- Requiere un valor

#### `--pipe`

Envíe la URL a stdout.

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor


## `environment:xdebug`

```bash
magento-cloud xdebug [--port PORT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [-i|--identity-file IDENTITY-FILE]
```

Abrir un túnel a Xdebug en el entorno

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--port`

El puerto local

- Predeterminado: `9000`
- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--app`, `-A`

El nombre de la aplicación remota

- Requiere un valor

#### `--worker`

Un nombre de trabajador

- Requiere un valor

#### `--instance`, `-I`

Un ID de instancia

- Requiere un valor

#### `--identity-file`, `-i`

Una identidad SSH (clave privada) que utilizar

- Requiere un valor


## `integration:activity:get`

```bash
magento-cloud integration:activity:get [-P|--property PROPERTY] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [--] [<integration>] [<activity>]
```

Ver información detallada sobre una sola actividad de integración

### Argumentos

#### `integration`

Un ID de integración. Déjelo en blanco para elegir de una lista.


#### `activity`

El ID de actividad. El valor predeterminado es la actividad de integración más reciente.

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--property`, `-P`

La propiedad que se va a ver

- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

[Opción obsoleta, no utilizada]

- Requiere un valor

#### `--format`

El formato de salida: tabla, csv, tsv o sin formato

- Predeterminado: `table`
- Requiere un valor

#### `--columns`, `-c`

Columnas para mostrar. Los caracteres % o * se pueden usar como comodín. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--no-header`

No generar el encabezado de tabla

- Predeterminado: `false`
- No acepta un valor

#### `--date-fmt`

El formato de fecha (como una cadena de formato de fecha PHP)

- Predeterminado: `c`
- Requiere un valor


## `integration:activity:list`

```bash
magento-cloud int:act [--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [--limit LIMIT] [--start START] [--state STATE] [--result RESULT] [-i|--incomplete] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<id>]
```

Obtener una lista de actividades para una integración

### Argumentos

#### `id`

Un ID de integración. Déjelo en blanco para elegir de una lista.

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--type`

Filtrar actividades por tipo. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--exclude-type`, `-x`

Excluir actividades por tipo. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco. Los caracteres % o * se pueden usar como comodín para excluir tipos.

- Predeterminado: `[]`
- Requiere un valor

#### `--limit`

Limitar el número de resultados mostrados

- Predeterminado: `10`
- Requiere un valor

#### `--start`

Solo se enumerarán las actividades creadas antes de esta fecha

- Requiere un valor

#### `--state`

Filtrar actividades por estado. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--result`

Filtrado de actividades por resultado

- Requiere un valor

#### `--incomplete`, `-i`

Enumerar solo las actividades incompletas

- Predeterminado: `false`
- No acepta un valor

#### `--format`

El formato de salida: tabla, csv, tsv o sin formato

- Predeterminado: `table`
- Requiere un valor

#### `--columns`, `-c`

Columnas para mostrar. Columnas disponibles: id*, created*, description*, type*, state*, result*, completed (* = columnas predeterminadas). El carácter &quot;+&quot; se puede utilizar como marcador de posición para las columnas predeterminadas. Los caracteres % o * se pueden usar como comodín. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--no-header`

No generar el encabezado de tabla

- Predeterminado: `false`
- No acepta un valor

#### `--date-fmt`

El formato de fecha (como una cadena de formato de fecha PHP)

- Predeterminado: `c`
- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

[Opción obsoleta, no utilizada]

- Requiere un valor


## `integration:activity:log`

```bash
magento-cloud integration:activity:log [-t|--timestamps] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<integration>] [<activity>]
```

Mostrar el registro de una actividad de integración

### Argumentos

#### `integration`

Un ID de integración. Déjelo en blanco para elegir de una lista.


#### `activity`

El ID de actividad. El valor predeterminado es la actividad de integración más reciente.

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--timestamps`, `-t`

Mostrar una marca de tiempo junto a cada mensaje

- Predeterminado: `false`
- No acepta un valor

#### `--date-fmt`

El formato de fecha (como una cadena de formato de fecha PHP)

- Predeterminado: `c`
- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

[Opción obsoleta, no utilizada]

- Requiere un valor


## `integration:add`

```bash
magento-cloud integration:add [--type TYPE] [--base-url BASE-URL] [--bitbucket-url BITBUCKET-URL] [--username USERNAME] [--token TOKEN] [--key KEY] [--secret SECRET] [--license-key LICENSE-KEY] [--server-project SERVER-PROJECT] [--repository REPOSITORY] [--build-merge-requests BUILD-MERGE-REQUESTS] [--build-pull-requests BUILD-PULL-REQUESTS] [--build-draft-pull-requests BUILD-DRAFT-PULL-REQUESTS] [--build-pull-requests-post-merge BUILD-PULL-REQUESTS-POST-MERGE] [--build-wip-merge-requests BUILD-WIP-MERGE-REQUESTS] [--merge-requests-clone-parent-data MERGE-REQUESTS-CLONE-PARENT-DATA] [--pull-requests-clone-parent-data PULL-REQUESTS-CLONE-PARENT-DATA] [--resync-pull-requests RESYNC-PULL-REQUESTS] [--fetch-branches FETCH-BRANCHES] [--prune-branches PRUNE-BRANCHES] [--resources-init RESOURCES-INIT] [--url URL] [--shared-key SHARED-KEY] [--file FILE] [--events EVENTS] [--states STATES] [--environments ENVIRONMENTS] [--excluded-environments EXCLUDED-ENVIRONMENTS] [--from-address FROM-ADDRESS] [--recipients RECIPIENTS] [--channel CHANNEL] [--routing-key ROUTING-KEY] [--category CATEGORY] [--index INDEX] [--sourcetype SOURCETYPE] [--protocol PROTOCOL] [--syslog-host SYSLOG-HOST] [--syslog-port SYSLOG-PORT] [--facility FACILITY] [--message-format MESSAGE-FORMAT] [--auth-mode AUTH-MODE] [--auth-token AUTH-TOKEN] [--verify-tls VERIFY-TLS] [--header HEADER] [-p|--project PROJECT] [-W|--no-wait] [--wait]
```

Añadir una integración al proyecto

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--type`

El tipo de integración (&quot;bitbucket&quot;, &quot;bitbucket_server&quot;, &quot;github&quot;, &quot;gitlab&quot;, &quot;webhook&quot;, &quot;health.email&quot;, &quot;health.pagerduty&quot;, &quot;health.slack&quot;, &quot;health.webhook&quot;, &quot;httplog&quot;, &quot;script&quot;, &quot;newrelic&quot;, &quot;splunk&quot;, &quot;sumologic&quot;, &quot;syslog&quot;)

- Requiere un valor

#### `--base-url`

La URL base de la instalación del servidor

- Requiere un valor

#### `--bitbucket-url`

La URL base de la instalación del servidor Bitbucket

- Requiere un valor

#### `--username`

Nombre de usuario del servidor Bitbucket

- Requiere un valor

#### `--token`

Un token de autenticación o acceso para la integración

- Requiere un valor

#### `--key`

Una clave de consumidor de OAuth de Bitbucket

- Requiere un valor

#### `--secret`

Un secreto de consumidor de OAuth de Bitbucket

- Requiere un valor

#### `--license-key`

Clave de licencia de registros de New Relic

- Requiere un valor

#### `--server-project`

El proyecto (por ejemplo, &quot;área de nombres/repositorio&quot;)

- Requiere un valor

#### `--repository`

El repositorio que se va a rastrear (p. ej. &quot;propietario/repositorio&quot;)

- Requiere un valor

#### `--build-merge-requests`

GitLab: crear solicitudes de combinación como entornos

- Predeterminado: `true`
- Requiere un valor

#### `--build-pull-requests`

Generar cada solicitud de extracción como un entorno

- Predeterminado: `true`
- Requiere un valor

#### `--build-draft-pull-requests`

Generar solicitudes de extracción de borrador

- Predeterminado: `true`
- Requiere un valor

#### `--build-pull-requests-post-merge`

Generar solicitudes de extracción en función de su estado posterior a la combinación

- Predeterminado: `false`
- Requiere un valor

#### `--build-wip-merge-requests`

GitLab: crear solicitudes de combinación de WIP

- Predeterminado: `true`
- Requiere un valor

#### `--merge-requests-clone-parent-data`

GitLab: clonar datos para solicitudes de combinación

- Predeterminado: `true`
- Requiere un valor

#### `--pull-requests-clone-parent-data`

Clonar los datos del entorno principal para solicitudes de extracción

- Predeterminado: `true`
- Requiere un valor

#### `--resync-pull-requests`

Volver a sincronizar los datos del entorno de solicitud de extracción en cada compilación

- Predeterminado: `false`
- Requiere un valor

#### `--fetch-branches`

Recupere todas las ramas del remoto (como entornos inactivos)

- Predeterminado: `true`
- Requiere un valor

#### `--prune-branches`

Eliminar ramas que no existen en el remoto

- Predeterminado: `true`
- Requiere un valor

#### `--resources-init`

Los recursos que se deben utilizar al inicializar un nuevo servicio (&quot;mínimo&quot;, &quot;predeterminado&quot;, &quot;manual&quot;, &quot;principal&quot;)

- Requiere un valor

#### `--url`

Extremo de URL o API para la integración

- Requiere un valor

#### `--shared-key`

Webhook: la clave secreta compartida de JWS

- Requiere un valor

#### `--file`

Nombre de un archivo local que contiene el script que se va a cargar

- Requiere un valor

#### `--events`

Una lista de eventos en los que actuar, p. ej. environment.push

- Predeterminado: `*`
- Requiere un valor

#### `--states`

Una lista de estados en los que actuar, por ejemplo, pendiente, en curso, completo

- Predeterminado: `complete`
- Requiere un valor

#### `--environments`

Los ID de entorno que se incluirán

- Predeterminado: `*`
- Requiere un valor

#### `--excluded-environments`

Los ID de entorno que se excluirán

- Predeterminado: `[]`
- Requiere un valor

#### `--from-address`

[Opcional] Dirección REMITENTE personalizada para correos electrónicos de alerta

- Requiere un valor

#### `--recipients`

Las direcciones de correo electrónico de los destinatarios

- Predeterminado: `[]`
- Requiere un valor

#### `--channel`

El canal de Slack

- Requiere un valor

#### `--routing-key`

La clave de enrutamiento PagerDuty

- Requiere un valor

#### `--category`

La categoría Lógica de sumo, utilizada para filtrar

- Requiere un valor

#### `--index`

El índice de Splunk

- Requiere un valor

#### `--sourcetype`

El tipo de origen del evento Splunk

- Requiere un valor

#### `--protocol`

Protocolo de transporte Syslog (&#39;tcp&#39;, &#39;udp&#39;, &#39;tls&#39;)

- Predeterminado: `tls`
- Requiere un valor

#### `--syslog-host`

Host de recolector/relé Syslog

- Requiere un valor

#### `--syslog-port`

Puerto de recolector/relé Syslog

- Requiere un valor

#### `--facility`

Instalación de Syslog

- Predeterminado: `1`
- Requiere un valor

#### `--message-format`

Formato de mensaje Syslog (&#39;rfc3164&#39; o &#39;rfc5424&#39;)

- Predeterminado: `rfc5424`
- Requiere un valor

#### `--auth-mode`

Modo de autenticación (&quot;prefix&quot; o &quot;structured_data&quot;)

- Predeterminado: `prefix`
- Requiere un valor

#### `--auth-token`

Token de autenticación

- Requiere un valor

#### `--verify-tls`

Si la verificación de certificados HTTPS debe habilitarse (recomendado)

- Predeterminado: `true`
- Requiere un valor

#### `--header`

Encabezados HTTP para usar en solicitudes POST. Separe los nombres y valores con dos puntos (:).

- Predeterminado: `[]`
- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--no-wait`, `-W`

No espere a que se complete la operación

- Predeterminado: `false`
- No acepta un valor

#### `--wait`

Espere a que se complete la operación (valor predeterminado)

- Predeterminado: `false`
- No acepta un valor


## `integration:delete`

```bash
magento-cloud integration:delete [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<id>]
```

Eliminación de una integración de un proyecto

### Argumentos

#### `id`

El ID de integración. Déjelo en blanco para elegir de una lista.

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--no-wait`, `-W`

No espere a que se complete la operación

- Predeterminado: `false`
- No acepta un valor

#### `--wait`

Espere a que se complete la operación (valor predeterminado)

- Predeterminado: `false`
- No acepta un valor


## `integration:get`

```bash
magento-cloud integration:get [-P|--property [PROPERTY]] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [--] [<id>]
```

Visualización de detalles de una integración

### Argumentos

#### `id`

Un ID de integración. Déjelo en blanco para elegir de una lista.

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--property`, `-P`

La propiedad de integración que se va a ver

- Acepta un valor

#### `--format`

El formato de salida: tabla, csv, tsv o sin formato

- Predeterminado: `table`
- Requiere un valor

#### `--columns`, `-c`

Columnas para mostrar. Los caracteres % o * se pueden usar como comodín. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--no-header`

No generar el encabezado de tabla

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor


## `integration:list`

```bash
magento-cloud integrations [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT]
```

Ver una lista de las integraciones del proyecto

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--format`

El formato de salida: tabla, csv, tsv o sin formato

- Predeterminado: `table`
- Requiere un valor

#### `--columns`, `-c`

Columnas para mostrar. Columnas disponibles: ID, resumen, tipo. Los caracteres % o * se pueden usar como comodín. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--no-header`

No generar el encabezado de tabla

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor


## `integration:update`

```bash
magento-cloud integration:update [--type TYPE] [--base-url BASE-URL] [--bitbucket-url BITBUCKET-URL] [--username USERNAME] [--token TOKEN] [--key KEY] [--secret SECRET] [--license-key LICENSE-KEY] [--server-project SERVER-PROJECT] [--repository REPOSITORY] [--build-merge-requests BUILD-MERGE-REQUESTS] [--build-pull-requests BUILD-PULL-REQUESTS] [--build-draft-pull-requests BUILD-DRAFT-PULL-REQUESTS] [--build-pull-requests-post-merge BUILD-PULL-REQUESTS-POST-MERGE] [--build-wip-merge-requests BUILD-WIP-MERGE-REQUESTS] [--merge-requests-clone-parent-data MERGE-REQUESTS-CLONE-PARENT-DATA] [--pull-requests-clone-parent-data PULL-REQUESTS-CLONE-PARENT-DATA] [--resync-pull-requests RESYNC-PULL-REQUESTS] [--fetch-branches FETCH-BRANCHES] [--prune-branches PRUNE-BRANCHES] [--resources-init RESOURCES-INIT] [--url URL] [--shared-key SHARED-KEY] [--file FILE] [--events EVENTS] [--states STATES] [--environments ENVIRONMENTS] [--excluded-environments EXCLUDED-ENVIRONMENTS] [--from-address FROM-ADDRESS] [--recipients RECIPIENTS] [--channel CHANNEL] [--routing-key ROUTING-KEY] [--category CATEGORY] [--index INDEX] [--sourcetype SOURCETYPE] [--protocol PROTOCOL] [--syslog-host SYSLOG-HOST] [--syslog-port SYSLOG-PORT] [--facility FACILITY] [--message-format MESSAGE-FORMAT] [--auth-mode AUTH-MODE] [--auth-token AUTH-TOKEN] [--verify-tls VERIFY-TLS] [--header HEADER] [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<id>]
```

Actualización de una integración

### Argumentos

#### `id`

El ID de la integración que se va a actualizar

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--type`

El tipo de integración (&quot;bitbucket&quot;, &quot;bitbucket_server&quot;, &quot;github&quot;, &quot;gitlab&quot;, &quot;webhook&quot;, &quot;health.email&quot;, &quot;health.pagerduty&quot;, &quot;health.slack&quot;, &quot;health.webhook&quot;, &quot;httplog&quot;, &quot;script&quot;, &quot;newrelic&quot;, &quot;splunk&quot;, &quot;sumologic&quot;, &quot;syslog&quot;)

- Requiere un valor

#### `--base-url`

La URL base de la instalación del servidor

- Requiere un valor

#### `--bitbucket-url`

La URL base de la instalación del servidor Bitbucket

- Requiere un valor

#### `--username`

Nombre de usuario del servidor Bitbucket

- Requiere un valor

#### `--token`

Un token de autenticación o acceso para la integración

- Requiere un valor

#### `--key`

Una clave de consumidor de OAuth de Bitbucket

- Requiere un valor

#### `--secret`

Un secreto de consumidor de OAuth de Bitbucket

- Requiere un valor

#### `--license-key`

Clave de licencia de registros de New Relic

- Requiere un valor

#### `--server-project`

El proyecto (por ejemplo, &quot;área de nombres/repositorio&quot;)

- Requiere un valor

#### `--repository`

El repositorio que se va a rastrear (p. ej. &quot;propietario/repositorio&quot;)

- Requiere un valor

#### `--build-merge-requests`

GitLab: crear solicitudes de combinación como entornos

- Predeterminado: `true`
- Requiere un valor

#### `--build-pull-requests`

Generar cada solicitud de extracción como un entorno

- Predeterminado: `true`
- Requiere un valor

#### `--build-draft-pull-requests`

Generar solicitudes de extracción de borrador

- Predeterminado: `true`
- Requiere un valor

#### `--build-pull-requests-post-merge`

Generar solicitudes de extracción en función de su estado posterior a la combinación

- Predeterminado: `false`
- Requiere un valor

#### `--build-wip-merge-requests`

GitLab: crear solicitudes de combinación de WIP

- Predeterminado: `true`
- Requiere un valor

#### `--merge-requests-clone-parent-data`

GitLab: clonar datos para solicitudes de combinación

- Predeterminado: `true`
- Requiere un valor

#### `--pull-requests-clone-parent-data`

Clonar los datos del entorno principal para solicitudes de extracción

- Predeterminado: `true`
- Requiere un valor

#### `--resync-pull-requests`

Volver a sincronizar los datos del entorno de solicitud de extracción en cada compilación

- Predeterminado: `false`
- Requiere un valor

#### `--fetch-branches`

Recupere todas las ramas del remoto (como entornos inactivos)

- Predeterminado: `true`
- Requiere un valor

#### `--prune-branches`

Eliminar ramas que no existen en el remoto

- Predeterminado: `true`
- Requiere un valor

#### `--resources-init`

Los recursos que se deben utilizar al inicializar un nuevo servicio (&quot;mínimo&quot;, &quot;predeterminado&quot;, &quot;manual&quot;, &quot;principal&quot;)

- Requiere un valor

#### `--url`

Extremo de URL o API para la integración

- Requiere un valor

#### `--shared-key`

Webhook: la clave secreta compartida de JWS

- Requiere un valor

#### `--file`

Nombre de un archivo local que contiene el script que se va a cargar

- Requiere un valor

#### `--events`

Una lista de eventos en los que actuar, p. ej. environment.push

- Predeterminado: `*`
- Requiere un valor

#### `--states`

Una lista de estados en los que actuar, por ejemplo, pendiente, en curso, completo

- Predeterminado: `complete`
- Requiere un valor

#### `--environments`

Los ID de entorno que se incluirán

- Predeterminado: `*`
- Requiere un valor

#### `--excluded-environments`

Los ID de entorno que se excluirán

- Predeterminado: `[]`
- Requiere un valor

#### `--from-address`

[Opcional] Dirección REMITENTE personalizada para correos electrónicos de alerta

- Requiere un valor

#### `--recipients`

Las direcciones de correo electrónico de los destinatarios

- Predeterminado: `[]`
- Requiere un valor

#### `--channel`

El canal de Slack

- Requiere un valor

#### `--routing-key`

La clave de enrutamiento PagerDuty

- Requiere un valor

#### `--category`

La categoría Lógica de sumo, utilizada para filtrar

- Requiere un valor

#### `--index`

El índice de Splunk

- Requiere un valor

#### `--sourcetype`

El tipo de origen del evento Splunk

- Requiere un valor

#### `--protocol`

Protocolo de transporte Syslog (&#39;tcp&#39;, &#39;udp&#39;, &#39;tls&#39;)

- Predeterminado: `tls`
- Requiere un valor

#### `--syslog-host`

Host de recolector/relé Syslog

- Requiere un valor

#### `--syslog-port`

Puerto de recolector/relé Syslog

- Requiere un valor

#### `--facility`

Instalación de Syslog

- Predeterminado: `1`
- Requiere un valor

#### `--message-format`

Formato de mensaje Syslog (&#39;rfc3164&#39; o &#39;rfc5424&#39;)

- Predeterminado: `rfc5424`
- Requiere un valor

#### `--auth-mode`

Modo de autenticación (&quot;prefix&quot; o &quot;structured_data&quot;)

- Predeterminado: `prefix`
- Requiere un valor

#### `--auth-token`

Token de autenticación

- Requiere un valor

#### `--verify-tls`

Si la verificación de certificados HTTPS debe habilitarse (recomendado)

- Predeterminado: `true`
- Requiere un valor

#### `--header`

Encabezados HTTP para usar en solicitudes POST. Separe los nombres y valores con dos puntos (:).

- Predeterminado: `[]`
- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--no-wait`, `-W`

No espere a que se complete la operación

- Predeterminado: `false`
- No acepta un valor

#### `--wait`

Espere a que se complete la operación (valor predeterminado)

- Predeterminado: `false`
- No acepta un valor


## `integration:validate`

```bash
magento-cloud integration:validate [-p|--project PROJECT] [--] [<id>]
```

Validación de una integración existente

```
This command allows you to check whether an integration is valid.

An exit code of 0 means the integration is valid, while 4 means it is invalid.
Any other exit code indicates an unexpected error.

Integrations are validated automatically on creation and on update. However,
because they involve external resources, it is possible for a valid integration
to become invalid. For example, an access token may be revoked, or an external
repository may be deleted.
```

### Argumentos

#### `id`

Un ID de integración. Déjelo en blanco para elegir de una lista.

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor


## `local:build`

```bash
magento-cloud build [-a|--abslinks] [-s|--source SOURCE] [-d|--destination DESTINATION] [-c|--copy] [--clone] [--run-deploy-hooks] [--no-clean] [--no-archive] [--no-backup] [--no-cache] [--no-build-hooks] [--no-deps] [--working-copy] [--concurrency CONCURRENCY] [--lock] [--] [<app>]...
```

Generar el proyecto actual localmente

### Argumentos

#### `app`

Especificar las aplicaciones que se van a generar

- Predeterminado: `[]`
- Matriz

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--abslinks`, `-a`

Uso de vínculos absolutos

- Predeterminado: `false`
- No acepta un valor

#### `--source`, `-s`

El directorio de origen. El valor predeterminado es la raíz del proyecto actual.

- Requiere un valor

#### `--destination`, `-d`

El destino al que se enlazará simbólicamente la raíz web de cada aplicación. Valor predeterminado: _www

- Requiere un valor

#### `--copy`, `-c`

Copie en un directorio de compilación, en lugar de realizar enlaces simbólicos desde el origen

- Predeterminado: `false`
- No acepta un valor

#### `--clone`

Utilice Git para clonar el HEAD actual en el directorio de compilación

- Predeterminado: `false`
- No acepta un valor

#### `--run-deploy-hooks`

Ejecutar vínculos de implementación o post_deploy

- Predeterminado: `false`
- No acepta un valor

#### `--no-clean`

No eliminar las compilaciones antiguas

- Predeterminado: `false`
- No acepta un valor

#### `--no-archive`

No cree ni utilice un archivo de compilación

- Predeterminado: `false`
- No acepta un valor

#### `--no-backup`

No realizar copia de seguridad de la versión anterior

- Predeterminado: `false`
- No acepta un valor

#### `--no-cache`

Deshabilitar almacenamiento en caché

- Predeterminado: `false`
- No acepta un valor

#### `--no-build-hooks`

No ejecutar vínculos posteriores a la compilación

- Predeterminado: `false`
- No acepta un valor

#### `--no-deps`

No instale las dependencias de compilación localmente

- Predeterminado: `false`
- No acepta un valor

#### `--working-copy`

Borrado: utilice Git para clonar un repositorio de cada módulo de Drupal en lugar de simplemente descargar una versión

- Predeterminado: `false`
- No acepta un valor

#### `--concurrency`

Borrado: define el número de proyectos simultáneos que se procesarán al mismo tiempo

- Predeterminado: `4`
- Requiere un valor

#### `--lock`

Borrado: crear o actualizar un archivo de bloqueo (solo disponible con la versión 7 o posterior de Borrado)

- Predeterminado: `false`
- No acepta un valor


## `local:dir`

```bash
magento-cloud dir [<subdir>]
```

Buscar la raíz del proyecto local

### Argumentos

#### `subdir`

El subdirectorio que se busca (&quot;local&quot;, &quot;web&quot; o &quot;compartido&quot;)

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).


## `metrics:all`

```bash
magento-cloud metrics [-B|--bytes] [-r|--range RANGE] [-i|--interval INTERVAL] [--to TO] [-1|--latest] [-s|--service SERVICE] [--type TYPE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

BETA Mostrar métricas de CPU, disco y memoria para un entorno

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--bytes`, `-B`

Mostrar tamaños en bytes

- Predeterminado: `false`
- No acepta un valor

#### `--range`, `-r`

El intervalo de tiempo. Las métricas se cargarán durante esta duración hasta la hora de finalización (—to). Puede especificar unidades: horas (h), minutos (m) o segundos (s). Mínimo 5 m, máximo 8 h o más (según el proyecto), predeterminado 10 m.

- Requiere un valor

#### `--interval`, `-i`

El intervalo de tiempo. El valor predeterminado es una división del intervalo. Puede especificar unidades: horas (h), minutos (m) o segundos (s). Mínimo 1 m.

- Requiere un valor

#### `--to`

La hora final. El valor predeterminado es ahora.

- Requiere un valor

#### `--latest`, `-1`

Mostrar solo el último punto de datos individual

- Predeterminado: `false`
- No acepta un valor

#### `--service`, `-s`

Filtrar por nombre de aplicación o servicio Los caracteres % o * se pueden utilizar como comodín.

- Predeterminado: `[]`
- Requiere un valor

#### `--type`

Filtrar por tipo de servicio (si no se proporciona —service). La versión no es obligatoria. Los caracteres % o * se pueden usar como comodín.

- Predeterminado: `[]`
- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--format`

El formato de salida: tabla, csv, tsv o sin formato

- Predeterminado: `table`
- Requiere un valor

#### `--columns`, `-c`

Columnas para mostrar. Columnas disponibles: timestamp*, service*, cpu_percent*, mem_percent*, disk_percent*, tmp_disk_percent*, cpu_limit, cpu_used, disk_limit, disk_used, inodes_limit, inodes_percent, inodes_used, mem_limit, mem_used, tmp_disk_limit, tmp_disk_used, tmp_inodes_limit, tmp_inodes_percent, tmp_inodes_used, type (* = columnas predeterminadas). El carácter &quot;+&quot; se puede utilizar como marcador de posición para las columnas predeterminadas. Los caracteres % o * se pueden usar como comodín. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--no-header`

No generar el encabezado de tabla

- Predeterminado: `false`
- No acepta un valor

#### `--date-fmt`

El formato de fecha (como una cadena de formato de fecha PHP)

- Predeterminado: `c`
- Requiere un valor


## `metrics:cpu`

```bash
magento-cloud cpu [-r|--range RANGE] [-i|--interval INTERVAL] [--to TO] [-1|--latest] [-s|--service SERVICE] [--type TYPE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

Uso de un entorno con Beta Show CPU

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--range`, `-r`

El intervalo de tiempo. Las métricas se cargarán durante esta duración hasta la hora de finalización (—to). Puede especificar unidades: horas (h), minutos (m) o segundos (s). Mínimo 5 m, máximo 8 h o más (según el proyecto), predeterminado 10 m.

- Requiere un valor

#### `--interval`, `-i`

El intervalo de tiempo. El valor predeterminado es una división del intervalo. Puede especificar unidades: horas (h), minutos (m) o segundos (s). Mínimo 1 m.

- Requiere un valor

#### `--to`

La hora final. El valor predeterminado es ahora.

- Requiere un valor

#### `--latest`, `-1`

Mostrar solo el último punto de datos individual

- Predeterminado: `false`
- No acepta un valor

#### `--service`, `-s`

Filtrar por nombre de aplicación o servicio Los caracteres % o * se pueden utilizar como comodín.

- Predeterminado: `[]`
- Requiere un valor

#### `--type`

Filtrar por tipo de servicio (si no se proporciona —service). La versión no es obligatoria. Los caracteres % o * se pueden usar como comodín.

- Predeterminado: `[]`
- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--format`

El formato de salida: tabla, csv, tsv o sin formato

- Predeterminado: `table`
- Requiere un valor

#### `--columns`, `-c`

Columnas para mostrar. Columnas disponibles: marca de tiempo*, servicio*, utilizadas*, límite*, porcentaje*, tipo (* = columnas predeterminadas). El carácter &quot;+&quot; se puede utilizar como marcador de posición para las columnas predeterminadas. Los caracteres % o * se pueden usar como comodín. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--no-header`

No generar el encabezado de tabla

- Predeterminado: `false`
- No acepta un valor

#### `--date-fmt`

El formato de fecha (como una cadena de formato de fecha PHP)

- Predeterminado: `c`
- Requiere un valor


## `metrics:disk-usage`

```bash
magento-cloud disk [-B|--bytes] [-r|--range RANGE] [-i|--interval INTERVAL] [--to TO] [-1|--latest] [-s|--service SERVICE] [--type TYPE] [--tmp] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

Mostrar el uso del disco de un entorno

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--bytes`, `-B`

Mostrar tamaños en bytes

- Predeterminado: `false`
- No acepta un valor

#### `--range`, `-r`

El intervalo de tiempo. Las métricas se cargarán durante esta duración hasta la hora de finalización (—to). Puede especificar unidades: horas (h), minutos (m) o segundos (s). Mínimo 5 m, máximo 8 h o más (según el proyecto), predeterminado 10 m.

- Requiere un valor

#### `--interval`, `-i`

El intervalo de tiempo. El valor predeterminado es una división del intervalo. Puede especificar unidades: horas (h), minutos (m) o segundos (s). Mínimo 1 m.

- Requiere un valor

#### `--to`

La hora final. El valor predeterminado es ahora.

- Requiere un valor

#### `--latest`, `-1`

Mostrar solo el último punto de datos individual

- Predeterminado: `false`
- No acepta un valor

#### `--service`, `-s`

Filtrar por nombre de aplicación o servicio Los caracteres % o * se pueden utilizar como comodín.

- Predeterminado: `[]`
- Requiere un valor

#### `--type`

Filtrar por tipo de servicio (si no se proporciona —service). La versión no es obligatoria. Los caracteres % o * se pueden usar como comodín.

- Predeterminado: `[]`
- Requiere un valor

#### `--tmp`

Informar del uso temporal del disco (muestra columnas: timestamp, service, tmp_used, tmp_limit, tmp_percent, tmp_ipercent)

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--format`

El formato de salida: tabla, csv, tsv o sin formato

- Predeterminado: `table`
- Requiere un valor

#### `--columns`, `-c`

Columnas para mostrar. Columnas disponibles: timestamp*, service*, used*, limit*, percent*, ipercent*, tmp_percent*, limit, iused, tmp_limit, tmp_ipercent, tmp_iused, tmp_limit, tmp_used, type (* = columnas predeterminadas). El carácter &quot;+&quot; se puede utilizar como marcador de posición para las columnas predeterminadas. Los caracteres % o * se pueden usar como comodín. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--no-header`

No generar el encabezado de tabla

- Predeterminado: `false`
- No acepta un valor

#### `--date-fmt`

El formato de fecha (como una cadena de formato de fecha PHP)

- Predeterminado: `c`
- Requiere un valor


## `metrics:memory`

```bash
magento-cloud mem [-B|--bytes] [-r|--range RANGE] [-i|--interval INTERVAL] [--to TO] [-1|--latest] [-s|--service SERVICE] [--type TYPE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

BETA Mostrar el uso de memoria de un entorno

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--bytes`, `-B`

Mostrar tamaños en bytes

- Predeterminado: `false`
- No acepta un valor

#### `--range`, `-r`

El intervalo de tiempo. Las métricas se cargarán durante esta duración hasta la hora de finalización (—to). Puede especificar unidades: horas (h), minutos (m) o segundos (s). Mínimo 5 m, máximo 8 h o más (según el proyecto), predeterminado 10 m.

- Requiere un valor

#### `--interval`, `-i`

El intervalo de tiempo. El valor predeterminado es una división del intervalo. Puede especificar unidades: horas (h), minutos (m) o segundos (s). Mínimo 1 m.

- Requiere un valor

#### `--to`

La hora final. El valor predeterminado es ahora.

- Requiere un valor

#### `--latest`, `-1`

Mostrar solo el último punto de datos individual

- Predeterminado: `false`
- No acepta un valor

#### `--service`, `-s`

Filtrar por nombre de aplicación o servicio Los caracteres % o * se pueden utilizar como comodín.

- Predeterminado: `[]`
- Requiere un valor

#### `--type`

Filtrar por tipo de servicio (si no se proporciona —service). La versión no es obligatoria. Los caracteres % o * se pueden usar como comodín.

- Predeterminado: `[]`
- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--format`

El formato de salida: tabla, csv, tsv o sin formato

- Predeterminado: `table`
- Requiere un valor

#### `--columns`, `-c`

Columnas para mostrar. Columnas disponibles: marca de tiempo*, servicio*, utilizadas*, límite*, porcentaje*, tipo (* = columnas predeterminadas). El carácter &quot;+&quot; se puede utilizar como marcador de posición para las columnas predeterminadas. Los caracteres % o * se pueden usar como comodín. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--no-header`

No generar el encabezado de tabla

- Predeterminado: `false`
- No acepta un valor

#### `--date-fmt`

El formato de fecha (como una cadena de formato de fecha PHP)

- Predeterminado: `c`
- Requiere un valor


## `mount:download`

```bash
magento-cloud mount:download [-a|--all] [-m|--mount MOUNT] [--target TARGET] [--source-path] [--delete] [--exclude EXCLUDE] [--include INCLUDE] [--refresh] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [-i|--identity-file IDENTITY-FILE]
```

Descargar archivos desde un montaje mediante rsync

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--all`, `-a`

Descargar desde todos los montajes

- Predeterminado: `false`
- No acepta un valor

#### `--mount`, `-m`

El montaje (como ruta relativa a la aplicación)

- Requiere un valor

#### `--target`

El directorio en el que se descargarán los archivos. Si se utiliza —all, se anexará la ruta de montaje

- Requiere un valor

#### `--source-path`

Utilice la ruta de origen del montaje (en lugar de la ruta de montaje) como subdirectorio del destino, cuando se utiliza —all

- Predeterminado: `false`
- No acepta un valor

#### `--delete`

Si se eliminarán archivos prescindibles en el directorio de destino

- Predeterminado: `false`
- No acepta un valor

#### `--exclude`

Archivos que se excluirán de la descarga (patrón)

- Predeterminado: `[]`
- Requiere un valor

#### `--include`

Archivo(s) no excluir (patrón)

- Predeterminado: `[]`
- Requiere un valor

#### `--refresh`

Si se actualiza la caché

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--app`, `-A`

El nombre de la aplicación remota

- Requiere un valor

#### `--worker`

Un nombre de trabajador

- Requiere un valor

#### `--instance`, `-I`

Un ID de instancia

- Requiere un valor

#### `--identity-file`, `-i`

Una identidad SSH (clave privada) que utilizar

- Requiere un valor


## `mount:list`

```bash
magento-cloud mounts [--paths] [--refresh] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE]
```

Obtener una lista de montajes

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--paths`

Salida de las rutas de montaje solamente (una por línea)

- Predeterminado: `false`
- No acepta un valor

#### `--refresh`

Si se actualiza la caché

- Predeterminado: `false`
- No acepta un valor

#### `--format`

El formato de salida: tabla, csv, tsv o sin formato

- Predeterminado: `table`
- Requiere un valor

#### `--columns`, `-c`

Columnas para mostrar. Columnas disponibles: definición, ruta. Los caracteres % o * se pueden usar como comodín. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--no-header`

No generar el encabezado de tabla

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--app`, `-A`

El nombre de la aplicación remota

- Requiere un valor

#### `--worker`

Un nombre de trabajador

- Requiere un valor

#### `--instance`, `-I`

Un ID de instancia

- Requiere un valor


## `mount:size`

```bash
magento-cloud mount:size [-B|--bytes] [--refresh] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE]
```

Compruebe el uso de los soportes en el disco

```
Use this command to check the disk size and usage for an application's mounts.

Mounts are directories mounted into the application from a persistent, writable
filesystem. They are configured in the mounts key in the application configuration.

The filesystem's total size is determined by the disk key in the same file.
```

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--bytes`, `-B`

Mostrar tamaños en bytes

- Predeterminado: `false`
- No acepta un valor

#### `--refresh`

Actualizar la caché

- Predeterminado: `false`
- No acepta un valor

#### `--format`

El formato de salida: tabla, csv, tsv o sin formato

- Predeterminado: `table`
- Requiere un valor

#### `--columns`, `-c`

Columnas para mostrar. Columnas disponibles: available, max, mounts, percent_used, tamaños, used. Los caracteres % o * se pueden usar como comodín. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--no-header`

No generar el encabezado de tabla

- Predeterminado: `false`
- No acepta un valor

#### `--identity-file`, `-i`

Una identidad SSH (clave privada) que utilizar

- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--app`, `-A`

El nombre de la aplicación remota

- Requiere un valor

#### `--worker`

Un nombre de trabajador

- Requiere un valor

#### `--instance`, `-I`

Un ID de instancia

- Requiere un valor


## `mount:upload`

```bash
magento-cloud mount:upload [--source SOURCE] [-m|--mount MOUNT] [--delete] [--exclude EXCLUDE] [--include INCLUDE] [--refresh] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [-i|--identity-file IDENTITY-FILE]
```

Cargar archivos a un montaje mediante rsync

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--source`

Directorio que contiene los archivos que se van a cargar

- Requiere un valor

#### `--mount`, `-m`

El montaje (como ruta relativa a la aplicación)

- Requiere un valor

#### `--delete`

Si se eliminarán archivos superfluos en el montaje

- Predeterminado: `false`
- No acepta un valor

#### `--exclude`

Archivos que se excluirán de la carga (patrón)

- Predeterminado: `[]`
- Requiere un valor

#### `--include`

Archivo(s) no excluir (patrón)

- Predeterminado: `[]`
- Requiere un valor

#### `--refresh`

Si se actualiza la caché

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--app`, `-A`

El nombre de la aplicación remota

- Requiere un valor

#### `--worker`

Un nombre de trabajador

- Requiere un valor

#### `--instance`, `-I`

Un ID de instancia

- Requiere un valor

#### `--identity-file`, `-i`

Una identidad SSH (clave privada) que utilizar

- Requiere un valor


## `operation:list`

```bash
magento-cloud ops [--full] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

Operaciones de tiempo de ejecución de lista de Beta en un entorno

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--full`

No limite la longitud del comando que se va a mostrar. El límite predeterminado es de 24 líneas.

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--app`, `-A`

El nombre de la aplicación remota

- Requiere un valor

#### `--worker`

Un nombre de trabajador

- Requiere un valor

#### `--format`

El formato de salida: tabla, csv, tsv o sin formato

- Predeterminado: `table`
- Requiere un valor

#### `--columns`, `-c`

Columnas para mostrar. Columnas disponibles: servicio*, nombre*, inicio*, función, detención (* = columnas predeterminadas). El carácter &quot;+&quot; se puede utilizar como marcador de posición para las columnas predeterminadas. Los caracteres % o * se pueden usar como comodín. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--no-header`

No generar el encabezado de tabla

- Predeterminado: `false`
- No acepta un valor


## `operation:run`

```bash
magento-cloud operation:run [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-W|--no-wait] [--wait] [--] [<operation>]
```

BETA Ejecute una operación en el entorno

### Argumentos

#### `operation`

El nombre de la operación

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--app`, `-A`

El nombre de la aplicación remota

- Requiere un valor

#### `--worker`

Un nombre de trabajador

- Requiere un valor

#### `--no-wait`, `-W`

No espere a que se complete la operación

- Predeterminado: `false`
- No acepta un valor

#### `--wait`

Espere a que se complete la operación (valor predeterminado)

- Predeterminado: `false`
- No acepta un valor


## `project:clear-build-cache`

```bash
magento-cloud project:clear-build-cache [-p|--project PROJECT]
```

Borrar la memoria caché de la versión de un proyecto

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor


## `project:get`

```bash
magento-cloud get [-e|--environment ENVIRONMENT] [--depth DEPTH] [--build] [-p|--project PROJECT] [-i|--identity-file IDENTITY-FILE] [--] [<project>] [<directory>]
```

Clonar un proyecto localmente

### Argumentos

#### `project`

El ID del proyecto


#### `directory`

El directorio en el que clonar. Valores predeterminados del título del proyecto

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--environment`, `-e`

El ID de entorno que se va a clonar. Valores predeterminados del proyecto o el primer entorno disponible

- Requiere un valor

#### `--depth`

Crear un clon superficial: limitar el número de confirmaciones en el historial

- Requiere un valor

#### `--build`

Cree el proyecto después de la clonación

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--identity-file`, `-i`

Una identidad SSH (clave privada) que utilizar

- Requiere un valor


## `project:info`

```bash
magento-cloud project:info [--refresh] [--date-fmt DATE-FMT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<property>] [<value>]
```

Leer o establecer las propiedades de un proyecto

### Argumentos

#### `property`

El nombre de la propiedad


#### `value`

Establezca un nuevo valor para la propiedad

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--refresh`

Si se actualiza la caché

- Predeterminado: `false`
- No acepta un valor

#### `--date-fmt`

El formato de fecha (como una cadena de formato de fecha PHP)

- Predeterminado: `c`
- Requiere un valor

#### `--format`

El formato de salida: tabla, csv, tsv o sin formato

- Predeterminado: `table`
- Requiere un valor

#### `--columns`, `-c`

Columnas para mostrar. Los caracteres % o * se pueden usar como comodín. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--no-header`

No generar el encabezado de tabla

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--no-wait`, `-W`

No espere a que se complete la operación

- Predeterminado: `false`
- No acepta un valor

#### `--wait`

Espere a que se complete la operación (valor predeterminado)

- Predeterminado: `false`
- No acepta un valor


## `project:list`

```bash
magento-cloud projects [--pipe] [--region REGION] [--title TITLE] [--my] [--refresh REFRESH] [--sort SORT] [--reverse] [--page PAGE] [-c|--count COUNT] [--format FORMAT] [--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

Obtener una lista de todos los proyectos activos

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--pipe`

Presente una lista sencilla de ID de proyecto. Deshabilita la paginación.

- Predeterminado: `false`
- No acepta un valor

#### `--region`

Filtrar por región (coincidencia exacta)

- Requiere un valor

#### `--title`

Filtrar por título (búsqueda sin distinción de mayúsculas y minúsculas)

- Requiere un valor

#### `--my`

Mostrar solo los proyectos de su propiedad

- Predeterminado: `false`
- No acepta un valor

#### `--refresh`

Si se actualiza la lista

- Predeterminado: `1`
- Requiere un valor

#### `--sort`

Una propiedad por la que ordenar

- Predeterminado: `title`
- Requiere un valor

#### `--reverse`

Ordenar en orden inverso (descendente)

- Predeterminado: `false`
- No acepta un valor

#### `--page`

Número de página. Esto habilita la paginación, a pesar de la configuración de o —count. Se ignora si se especifica —pipe.

- Requiere un valor

#### `--count`, `-c`

El número de proyectos que se mostrarán por página. Utilice 0 para desactivar la paginación. Se ignora si se especifica —page.

- Requiere un valor

#### `--format`

El formato de salida: tabla, csv, tsv o sin formato

- Predeterminado: `table`
- Requiere un valor

#### `--columns`

Columnas para mostrar. Columnas disponibles: id*, title*, region*, created_at, organization_id, organization_label, organization_name, status (* = columnas predeterminadas). El carácter &quot;+&quot; se puede utilizar como marcador de posición para las columnas predeterminadas. Los caracteres % o * se pueden usar como comodín. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--no-header`

No generar el encabezado de tabla

- Predeterminado: `false`
- No acepta un valor

#### `--date-fmt`

El formato de fecha (como una cadena de formato de fecha PHP)

- Predeterminado: `c`
- Requiere un valor


## `project:set-remote`

```bash
magento-cloud set-remote [<project>]
```

Establecer el proyecto remoto para el repositorio Git actual

### Argumentos

#### `project`

El ID del proyecto

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).


## `repo:cat`

```bash
magento-cloud repo:cat [-c|--commit COMMIT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] <path>
```

Leer un archivo en el repositorio del proyecto

### Argumentos

#### `path`

La ruta al archivo

- Requerido

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--commit`, `-c`

El SHA de compromiso. Esto también puede aceptar sufijos de &quot;HEAD&quot; y acento circunflejo (^) o tilde (~) para confirmaciones principales.

- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor


## `repo:ls`

```bash
magento-cloud repo:ls [-d|--directories] [-f|--files] [--git-style] [-c|--commit COMMIT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<path>]
```

Enumerar archivos en el repositorio del proyecto

### Argumentos

#### `path`

La ruta a un subdirectorio

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--directories`, `-d`

Mostrar solo directorios

- Predeterminado: `false`
- No acepta un valor

#### `--files`, `-f`

Mostrar solo archivos

- Predeterminado: `false`
- No acepta un valor

#### `--git-style`

Salida de estilo similar a &quot;git ls-tree&quot;

- Predeterminado: `false`
- No acepta un valor

#### `--commit`, `-c`

El SHA de compromiso. Esto también puede aceptar sufijos de &quot;HEAD&quot; y acento circunflejo (^) o tilde (~) para confirmaciones principales.

- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor


## `repo:read`

```bash
magento-cloud read [-c|--commit COMMIT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<path>]
```

Leer un directorio o archivo en el repositorio del proyecto

### Argumentos

#### `path`

La ruta al directorio o archivo

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--commit`, `-c`

El SHA de compromiso. Esto también puede aceptar sufijos de &quot;HEAD&quot; y acento circunflejo (^) o tilde (~) para confirmaciones principales.

- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor


## `route:get`

```bash
magento-cloud route:get [--id ID] [-1|--primary] [-P|--property PROPERTY] [--refresh] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-i|--identity-file IDENTITY-FILE] [--] [<route>]
```

Ver información detallada sobre una ruta

### Argumentos

#### `route`

La URL original de la ruta

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--id`

ID de ruta que se va a seleccionar

- Requiere un valor

#### `--primary`, `-1`

Seleccionar la ruta principal

- Predeterminado: `false`
- No acepta un valor

#### `--property`, `-P`

La propiedad que se va a mostrar

- Requiere un valor

#### `--refresh`

Omitir la caché de rutas

- Predeterminado: `false`
- No acepta un valor

#### `--date-fmt`

El formato de fecha (como una cadena de formato de fecha PHP)

- Predeterminado: `c`
- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--app`, `-A`

[Opción obsoleta, ya no se utiliza]

- Requiere un valor

#### `--identity-file`, `-i`

[Opción obsoleta, ya no se utiliza]

- Requiere un valor


## `route:list`

```bash
magento-cloud routes [--refresh] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<environment>]
```

Enumerar todas las rutas de un entorno

### Argumentos

#### `environment`

El ID de entorno

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--refresh`

Omitir la caché de rutas

- Predeterminado: `false`
- No acepta un valor

#### `--format`

El formato de salida: tabla, csv, tsv o sin formato

- Predeterminado: `table`
- Requiere un valor

#### `--columns`, `-c`

Columnas para mostrar. Columnas disponibles: route*, type*, to*, url (* = columnas predeterminadas). El carácter &quot;+&quot; se puede utilizar como marcador de posición para las columnas predeterminadas. Los caracteres % o * se pueden usar como comodín. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--no-header`

No generar el encabezado de tabla

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor


## `self:install`

```bash
magento-cloud self:install [--shell-type SHELL-TYPE]
```

Instalar o actualizar archivos de configuración de CLI

```
This command automatically installs shell configuration for the Magento Cloud CLI,
adding autocompletion support and handy aliases. Bash and ZSH are supported.
```

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--shell-type`

El tipo de shell para el autocompletado (bash o zsh)

- Requiere un valor


## `self:update`

```bash
magento-cloud update [--no-major] [--unstable] [--manifest MANIFEST] [--current-version CURRENT-VERSION] [--timeout TIMEOUT]
```

Actualice la CLI a la versión más reciente

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--no-major`

Actualizar solo entre versiones secundarias o de parche

- Predeterminado: `false`
- No acepta un valor

#### `--unstable`

Actualizar a una nueva versión inestable, si está disponible

- Predeterminado: `false`
- No acepta un valor

#### `--manifest`

Omitir la ubicación del archivo de manifiesto

- Requiere un valor

#### `--current-version`

Anular la versión actual

- Requiere un valor

#### `--timeout`

Tiempo de espera para la comprobación de versión

- Predeterminado: `30`
- Requiere un valor


## `service:list`

```bash
magento-cloud services [--refresh] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

Enumerar servicios en el proyecto

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--refresh`

Si se actualiza la caché

- Predeterminado: `false`
- No acepta un valor

#### `--pipe`

Mostrar solo una lista de nombres de servicio

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--format`

El formato de salida: tabla, csv, tsv o sin formato

- Predeterminado: `table`
- Requiere un valor

#### `--columns`, `-c`

Columnas para mostrar. Columnas disponibles: disco, nombre, tamaño, tipo. Los caracteres % o * se pueden usar como comodín. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--no-header`

No generar el encabezado de tabla

- Predeterminado: `false`
- No acepta un valor


## `service:mongo:dump`

```bash
magento-cloud mongodump [-c|--collection COLLECTION] [-z|--gzip] [-o|--stdout] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

Crear un volcado de archivo binario de datos de MongoDB

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--collection`, `-c`

Colección que se va a volcar

- Requiere un valor

#### `--gzip`, `-z`

Comprimir el volcado utilizando gzip

- Predeterminado: `false`
- No acepta un valor

#### `--stdout`, `-o`

Salida a STDOUT en lugar de a un archivo

- Predeterminado: `false`
- No acepta un valor

#### `--relationship`, `-r`

La relación de servicio que se va a utilizar

- Requiere un valor

#### `--identity-file`, `-i`

Una identidad SSH (clave privada) que utilizar

- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--app`, `-A`

El nombre de la aplicación remota

- Requiere un valor


## `service:mongo:export`

```bash
magento-cloud mongoexport [-c|--collection COLLECTION] [--jsonArray] [--type TYPE] [-f|--fields FIELDS] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

Exportación de datos de MongoDB

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--collection`, `-c`

Colección que se va a exportar

- Requiere un valor

#### `--jsonArray`

Exportar datos como una sola matriz JSON

- Predeterminado: `false`
- No acepta un valor

#### `--type`

El tipo de exportación, p. ej. &quot;csv&quot;

- Requiere un valor

#### `--fields`, `-f`

Los campos que se van a exportar

- Predeterminado: `[]`
- Requiere un valor

#### `--relationship`, `-r`

La relación de servicio que se va a utilizar

- Requiere un valor

#### `--identity-file`, `-i`

Una identidad SSH (clave privada) que utilizar

- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--app`, `-A`

El nombre de la aplicación remota

- Requiere un valor


## `service:mongo:restore`

```bash
magento-cloud mongorestore [-c|--collection COLLECTION] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

Restaurar un archivo binario volcado de datos en MongoDB

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--collection`, `-c`

Colección que se va a restaurar

- Requiere un valor

#### `--relationship`, `-r`

La relación de servicio que se va a utilizar

- Requiere un valor

#### `--identity-file`, `-i`

Una identidad SSH (clave privada) que utilizar

- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--app`, `-A`

El nombre de la aplicación remota

- Requiere un valor


## `service:mongo:shell`

```bash
magento-cloud mongo [--eval EVAL] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

Usar el shell de MongoDB

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--eval`

Pasar un fragmento de JavaScript al shell

- Requiere un valor

#### `--relationship`, `-r`

La relación de servicio que se va a utilizar

- Requiere un valor

#### `--identity-file`, `-i`

Una identidad SSH (clave privada) que utilizar

- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--app`, `-A`

El nombre de la aplicación remota

- Requiere un valor


## `service:redis-cli`

```bash
magento-cloud redis [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--] [<args>]
```

Acceso a la CLI de Redis

### Argumentos

#### `args`

Argumentos para agregar al comando Redis

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--relationship`, `-r`

La relación de servicio que se va a utilizar

- Requiere un valor

#### `--identity-file`, `-i`

Una identidad SSH (clave privada) que utilizar

- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--app`, `-A`

El nombre de la aplicación remota

- Requiere un valor


## `snapshot:create`

```bash
magento-cloud backup [--live] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<environment>]
```

Crear una instantánea de un entorno

### Argumentos

#### `environment`

El entorno

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--live`

Copia de seguridad en vivo: no detenga el entorno. Si se establece, el entorno se mantendrá en ejecución y abierto a las conexiones durante la copia de seguridad. Esto reduce el tiempo de inactividad, con el riesgo de realizar copias de seguridad de los datos en un estado incoherente.

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--no-wait`, `-W`

No espere a que se complete la operación

- Predeterminado: `false`
- No acepta un valor

#### `--wait`

Espere a que se complete la operación (valor predeterminado)

- Predeterminado: `false`
- No acepta un valor


## `snapshot:delete`

```bash
magento-cloud snapshot:delete [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<id>]
```

Eliminar una instantánea de entorno

### Argumentos

#### `id`

El ID de la instantánea. Necesario en modo no interactivo.

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--no-wait`, `-W`

No espere a que se complete la operación

- Predeterminado: `false`
- No acepta un valor

#### `--wait`

Espere a que se complete la operación (valor predeterminado)

- Predeterminado: `false`
- No acepta un valor


## `snapshot:get`

```bash
magento-cloud snapshot:get [-P|--property PROPERTY] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--date-fmt DATE-FMT] [--] [<id>]
```

Ver una instantánea de entorno

### Argumentos

#### `id`

El ID de la instantánea. El valor predeterminado es el más reciente.

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--property`, `-P`

Propiedad que se va a mostrar.

- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--date-fmt`

El formato de fecha (como una cadena de formato de fecha PHP)

- Predeterminado: `c`
- Requiere un valor


## `snapshot:list`

```bash
magento-cloud snapshots [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

Enumerar las instantáneas disponibles de un entorno

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--format`

El formato de salida: tabla, csv, tsv o sin formato

- Predeterminado: `table`
- Requiere un valor

#### `--columns`, `-c`

Columnas para mostrar. Los caracteres % o * se pueden usar como comodín. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--no-header`

No generar el encabezado de tabla

- Predeterminado: `false`
- No acepta un valor

#### `--date-fmt`

El formato de fecha (como una cadena de formato de fecha PHP)

- Predeterminado: `c`
- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor


## `snapshot:restore`

```bash
magento-cloud snapshot:restore [--target TARGET] [--branch-from BRANCH-FROM] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<snapshot>]
```

Restaurar una instantánea de entorno

### Argumentos

#### `snapshot`

Nombre de la instantánea. Valores predeterminados del más reciente

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--target`

El entorno en el que restaurar. Valores predeterminados del entorno actual de la instantánea

- Requiere un valor

#### `--branch-from`

Si —target aún no existe, especifica el elemento principal del nuevo entorno

- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--no-wait`, `-W`

No espere a que se complete la operación

- Predeterminado: `false`
- No acepta un valor

#### `--wait`

Espere a que se complete la operación (valor predeterminado)

- Predeterminado: `false`
- No acepta un valor


## `source-operation:list`

```bash
magento-cloud source-ops [--full] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

Enumerar operaciones de origen en un entorno

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--full`

No limite la longitud del comando que se va a mostrar. El límite predeterminado es de 24 líneas.

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--format`

El formato de salida: tabla, csv, tsv o sin formato

- Predeterminado: `table`
- Requiere un valor

#### `--columns`, `-c`

Columnas para mostrar. Columnas disponibles: aplicación, comando, operación. Los caracteres % o * se pueden usar como comodín. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--no-header`

No generar el encabezado de tabla

- Predeterminado: `false`
- No acepta un valor


## `source-operation:run`

```bash
magento-cloud source-operation:run [--variable VARIABLE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<operation>]
```

Ejecutar una operación de origen

### Argumentos

#### `operation`

El nombre de la operación

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--variable`

Variable que se va a establecer durante la operación con el formato type:name=value

- Predeterminado: `[]`
- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--no-wait`, `-W`

No espere a que se complete la operación

- Predeterminado: `false`
- No acepta un valor

#### `--wait`

Espere a que se complete la operación (valor predeterminado)

- Predeterminado: `false`
- No acepta un valor


## `ssh-cert:load`

```bash
magento-cloud ssh-cert:load [--refresh-only] [--new] [--new-key]
```

Generación de un certificado SSH

```
This command checks if a valid SSH certificate is present, and generates a
new one if necessary.

Certificates allow you to make SSH connections without having previously
uploaded a public key. They are more secure than keys and they allow for
other features.

Normally the certificate is loaded automatically during login, or when
making an SSH connection. So this command is seldom needed.

If you want to set up certificates without login and without an SSH-related
command, for example if you are writing a script that uses an API token via
an environment variable, then you would probably want to run this command
explicitly. For unattended scripts, remember to turn off interaction via
--yes or the MAGENTO_CLOUD_CLI_NO_INTERACTION environment variable.
```

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--refresh-only`

Actualice el certificado solo si es necesario (no escriba la configuración SSH)

- Predeterminado: `false`
- No acepta un valor

#### `--new`

Forzar la actualización del certificado

- Predeterminado: `false`
- No acepta un valor

#### `--new-key`

[Obsoleto] Use —nuevo en su lugar

- Predeterminado: `false`
- No acepta un valor


## `ssh-key:add`

```bash
magento-cloud ssh-key:add [--name NAME] [--] [<path>]
```

Añadir una nueva clave SSH

```
This command lets you add an SSH key to your account. It can generate a key using OpenSSH.

Notice:
SSH keys are no longer needed by default, as SSH certificates are supported.
Certificates offer more security than keys.

To load or check your SSH certificate, run: magento-cloud ssh-cert:load
```

### Argumentos

#### `path`

La ruta a una clave pública SSH existente

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--name`

Un nombre para identificar la clave

- Requiere un valor


## `ssh-key:delete`

```bash
magento-cloud ssh-key:delete [<id>]
```

Eliminación de una clave SSH

```
This command lets you delete SSH keys from your account.

Notice:
SSH keys are no longer needed by default, as SSH certificates are supported.
Certificates offer more security than keys.

To load or check your SSH certificate, run: magento-cloud ssh-cert:load
```

### Argumentos

#### `id`

El ID de la clave SSH que se va a eliminar

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).


## `ssh-key:list`

```bash
magento-cloud ssh-keys [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

Obtenga una lista de claves SSH en su cuenta

```
This command lets you list SSH keys in your account.

Notice:
SSH keys are no longer needed by default, as SSH certificates are supported.
Certificates offer more security than keys.

To load or check your SSH certificate, run: magento-cloud ssh-cert:load
```

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--format`

El formato de salida: tabla, csv, tsv o sin formato

- Predeterminado: `table`
- Requiere un valor

#### `--columns`, `-c`

Columnas para mostrar. Columnas disponibles: id*, title*, path*, huella digital (* = columnas predeterminadas). El carácter &quot;+&quot; se puede utilizar como marcador de posición para las columnas predeterminadas. Los caracteres % o * se pueden usar como comodín. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--no-header`

No generar el encabezado de tabla

- Predeterminado: `false`
- No acepta un valor


## `subscription:info`

```bash
magento-cloud subscription:info [-s|--id ID] [--date-fmt DATE-FMT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [--] [<property>] [<value>]
```

Leer o modificar propiedades de suscripción

### Argumentos

#### `property`

El nombre de la propiedad


#### `value`

Establezca un nuevo valor para la propiedad

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--id`, `-s`

ID de suscripción

- Requiere un valor

#### `--date-fmt`

El formato de fecha (como una cadena de formato de fecha PHP)

- Predeterminado: `c`
- Requiere un valor

#### `--format`

El formato de salida: tabla, csv, tsv o sin formato

- Predeterminado: `table`
- Requiere un valor

#### `--columns`, `-c`

Columnas para mostrar. Los caracteres % o * se pueden usar como comodín. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--no-header`

No generar el encabezado de tabla

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor


## `tunnel:close`

```bash
magento-cloud tunnel:close [-a|--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

Cierre de túneles SSH

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--all`, `-a`

Cerrar todos los túneles

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--app`, `-A`

El nombre de la aplicación remota

- Requiere un valor


## `tunnel:info`

```bash
magento-cloud tunnel:info [-P|--property PROPERTY] [-c|--encode] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

Ver información de relación para túneles SSH

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--property`, `-P`

La propiedad de relación que se va a ver

- Requiere un valor

#### `--encode`, `-c`

Salida como JSON con codificación base64

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--app`, `-A`

El nombre de la aplicación remota

- Requiere un valor


## `tunnel:list`

```bash
magento-cloud tunnels [-a|--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

Enumeración de túneles SSH

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--all`, `-a`

Ver todos los túneles

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--app`, `-A`

El nombre de la aplicación remota

- Requiere un valor

#### `--format`

El formato de salida: tabla, csv, tsv o sin formato

- Predeterminado: `table`
- Requiere un valor

#### `--columns`, `-c`

Columnas para mostrar. Columnas disponibles: puerto*, proyecto*, entorno*, aplicación*, relación*, URL (* = columnas predeterminadas). El carácter &quot;+&quot; se puede utilizar como marcador de posición para las columnas predeterminadas. Los caracteres % o * se pueden usar como comodín. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--no-header`

No generar el encabezado de tabla

- Predeterminado: `false`
- No acepta un valor


## `tunnel:open`

```bash
magento-cloud tunnel:open [-g|--gateway-ports] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-i|--identity-file IDENTITY-FILE]
```

Apertura de túneles SSH para las relaciones de una aplicación

```
This command opens SSH tunnels to all of the relationships of an application.

Connections can then be made to the application's services as if they were
local, for example a local MySQL client can be used, or the Solr web
administration endpoint can be accessed through a local browser.

This command requires the posix and pcntl PHP extensions (as multiple
background CLI processes are created to keep the SSH tunnels open). The
tunnel:single command can be used on systems without these
extensions.
```

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--gateway-ports`, `-g`

Permitir que los hosts remotos se conecten a los puertos reenviados locales

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--app`, `-A`

El nombre de la aplicación remota

- Requiere un valor

#### `--identity-file`, `-i`

Una identidad SSH (clave privada) que utilizar

- Requiere un valor


## `tunnel:single`

```bash
magento-cloud tunnel:single [--port PORT] [-g|--gateway-ports] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE]
```

Apertura de un solo túnel SSH para una relación de aplicación

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--port`

El puerto local

- Requiere un valor

#### `--gateway-ports`, `-g`

Permitir que los hosts remotos se conecten a los puertos reenviados locales

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--app`, `-A`

El nombre de la aplicación remota

- Requiere un valor

#### `--relationship`, `-r`

La relación de servicio que se va a utilizar

- Requiere un valor

#### `--identity-file`, `-i`

Una identidad SSH (clave privada) que utilizar

- Requiere un valor


## `user:add`

```bash
magento-cloud user:add [-r|--role ROLE] [--force-invite] [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<email>]
```

Agregar un usuario al proyecto

### Argumentos

#### `email`

La dirección de correo electrónico del usuario

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--role`, `-r`

La función de proyecto del usuario (&quot;administrador&quot; o &quot;visualizador&quot;) o la función de tipo de entorno (por ejemplo, &quot;ensayo:colaborador&quot; o &quot;producción:visualizador&quot;). Para quitar un usuario de un tipo de entorno, establezca la función como &quot;ninguno&quot;. Los caracteres % o * se pueden usar como comodín para el tipo de entorno, p. ej. &#39;%:viewer&#39; para dar al usuario la función &#39;viewer&#39; en todos los tipos. La función puede abreviarse, por ejemplo, &quot;producción:v&quot;.

- Predeterminado: `[]`
- Requiere un valor

#### `--force-invite`

Enviar una invitación, incluso si ya se ha enviado una

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--no-wait`, `-W`

No espere a que se complete la operación

- Predeterminado: `false`
- No acepta un valor

#### `--wait`

Espere a que se complete la operación (valor predeterminado)

- Predeterminado: `false`
- No acepta un valor


## `user:delete`

```bash
magento-cloud user:delete [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] <email>
```

Eliminar un usuario del proyecto

### Argumentos

#### `email`

La dirección de correo electrónico del usuario

- Requerido

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--no-wait`, `-W`

No espere a que se complete la operación

- Predeterminado: `false`
- No acepta un valor

#### `--wait`

Espere a que se complete la operación (valor predeterminado)

- Predeterminado: `false`
- No acepta un valor


## `user:get`

```bash
magento-cloud user:get [-l|--level LEVEL] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [-r|--role ROLE] [--] [<email>]
```

Ver las funciones de un usuario

### Argumentos

#### `email`

La dirección de correo electrónico del usuario

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--level`, `-l`

El nivel de función (&quot;proyecto&quot; o &quot;entorno&quot;)

- Requiere un valor

#### `--pipe`

Enviar la función a stdout (después de realizar cualquier cambio)

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--no-wait`, `-W`

No espere a que se complete la operación

- Predeterminado: `false`
- No acepta un valor

#### `--wait`

Espere a que se complete la operación (valor predeterminado)

- Predeterminado: `false`
- No acepta un valor

#### `--role`, `-r`

[Obsoleto: usar usuario:actualizar para cambiar los roles de un usuario]

- Requiere un valor


## `user:list`

```bash
magento-cloud users [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT]
```

Enumerar usuarios de proyecto

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--format`

El formato de salida: tabla, csv, tsv o sin formato

- Predeterminado: `table`
- Requiere un valor

#### `--columns`, `-c`

Columnas para mostrar. Columnas disponibles: email*, name*, role*, id*, granted_at, updated_at (* = columnas predeterminadas). El carácter &quot;+&quot; se puede utilizar como marcador de posición para las columnas predeterminadas. Los caracteres % o * se pueden usar como comodín. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--no-header`

No generar el encabezado de tabla

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor


## `user:update`

```bash
magento-cloud user:update [-r|--role ROLE] [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<email>]
```

Actualización de las funciones del usuario en un proyecto

### Argumentos

#### `email`

La dirección de correo electrónico del usuario

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--role`, `-r`

La función de proyecto del usuario (&quot;administrador&quot; o &quot;visualizador&quot;) o la función de tipo de entorno (por ejemplo, &quot;ensayo:colaborador&quot; o &quot;producción:visualizador&quot;). Para quitar un usuario de un tipo de entorno, establezca la función como &quot;ninguno&quot;. Los caracteres % o * se pueden usar como comodín para el tipo de entorno, p. ej. &#39;%:viewer&#39; para dar al usuario la función &#39;viewer&#39; en todos los tipos. La función puede abreviarse, por ejemplo, &quot;producción:v&quot;.

- Predeterminado: `[]`
- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--no-wait`, `-W`

No espere a que se complete la operación

- Predeterminado: `false`
- No acepta un valor

#### `--wait`

Espere a que se complete la operación (valor predeterminado)

- Predeterminado: `false`
- No acepta un valor


## `variable:create`

```bash
magento-cloud variable:create [-u|--update] [-l|--level LEVEL] [--name NAME] [--value VALUE] [--json JSON] [--sensitive SENSITIVE] [--prefix PREFIX] [--enabled ENABLED] [--inheritable INHERITABLE] [--visible-build VISIBLE-BUILD] [--visible-runtime VISIBLE-RUNTIME] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<name>]
```

Crear una variable

### Argumentos

#### `name`

El nombre de la variable

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--update`, `-u`

Actualizar la variable si ya existe

- Predeterminado: `false`
- No acepta un valor

#### `--level`, `-l`

Nivel en el que se va a establecer la variable (&quot;proyecto&quot; o &quot;entorno&quot;)

- Requiere un valor

#### `--name`

El nombre de la variable

- Requiere un valor

#### `--value`

El valor de la variable

- Requiere un valor

#### `--json`

Si el valor de la variable tiene formato JSON

- Predeterminado: `false`
- Requiere un valor

#### `--sensitive`

Si el valor de la variable es confidencial

- Predeterminado: `false`
- Requiere un valor

#### `--prefix`

El prefijo del nombre de la variable que puede determinar su tipo, por ejemplo &quot;env&quot;. Solo es aplicable si el nombre aún no contiene un prefijo. (p. ej. &#39;ninguno&#39; o &#39;env:&#39;)

- Predeterminado: `none`
- Requiere un valor

#### `--enabled`

Si la variable debe habilitarse en el entorno

- Predeterminado: `true`
- Requiere un valor

#### `--inheritable`

Si los entornos secundarios heredan la variable

- Predeterminado: `true`
- Requiere un valor

#### `--visible-build`

Si la variable debe ser visible en el momento de la compilación

- Requiere un valor

#### `--visible-runtime`

Si la variable debe ser visible durante la ejecución

- Predeterminado: `true`
- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--no-wait`, `-W`

No espere a que se complete la operación

- Predeterminado: `false`
- No acepta un valor

#### `--wait`

Espere a que se complete la operación (valor predeterminado)

- Predeterminado: `false`
- No acepta un valor


## `variable:delete`

```bash
magento-cloud variable:delete [-l|--level LEVEL] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

Eliminar una variable

### Argumentos

#### `name`

El nombre de la variable

- Requerido

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--level`, `-l`

El nivel de variable (&quot;proyecto&quot;, &quot;entorno&quot;, &quot;p&quot; o &quot;e&quot;)

- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--no-wait`, `-W`

No espere a que se complete la operación

- Predeterminado: `false`
- No acepta un valor

#### `--wait`

Espere a que se complete la operación (valor predeterminado)

- Predeterminado: `false`
- No acepta un valor


## `variable:get`

```bash
magento-cloud vget [-P|--property PROPERTY] [-l|--level LEVEL] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--pipe] [--] [<name>]
```

Ver una variable

### Argumentos

#### `name`

Nombre de la variable

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--property`, `-P`

Ver una sola propiedad de variable

- Requiere un valor

#### `--level`, `-l`

El nivel de variable (&quot;proyecto&quot;, &quot;entorno&quot;, &quot;p&quot; o &quot;e&quot;)

- Requiere un valor

#### `--format`

El formato de salida: tabla, csv, tsv o sin formato

- Predeterminado: `table`
- Requiere un valor

#### `--columns`, `-c`

Columnas para mostrar. Los caracteres % o * se pueden usar como comodín. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--no-header`

No generar el encabezado de tabla

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--pipe`

[Opción obsoleta]: envíe solo el valor de la variable

- Predeterminado: `false`
- No acepta un valor


## `variable:list`

```bash
magento-cloud variables [-l|--level LEVEL] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

Variables de lista

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--level`, `-l`

El nivel de variable (&quot;proyecto&quot;, &quot;entorno&quot;, &quot;p&quot; o &quot;e&quot;)

- Requiere un valor

#### `--format`

El formato de salida: tabla, csv, tsv o sin formato

- Predeterminado: `table`
- Requiere un valor

#### `--columns`, `-c`

Columnas para mostrar. Columnas disponibles: is_enabled, level, name, value. Los caracteres % o * se pueden usar como comodín. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--no-header`

No generar el encabezado de tabla

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor


## `variable:update`

```bash
magento-cloud variable:update [--allow-no-change] [-l|--level LEVEL] [--value VALUE] [--json JSON] [--sensitive SENSITIVE] [--enabled ENABLED] [--inheritable INHERITABLE] [--visible-build VISIBLE-BUILD] [--visible-runtime VISIBLE-RUNTIME] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

Actualización de una variable

### Argumentos

#### `name`

El nombre de la variable

- Requerido

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--allow-no-change`

Devolver correctamente (código de salida cero) si no se proporcionaron cambios

- Predeterminado: `false`
- No acepta un valor

#### `--level`, `-l`

El nivel de variable (&quot;proyecto&quot;, &quot;entorno&quot;, &quot;p&quot; o &quot;e&quot;)

- Requiere un valor

#### `--value`

El valor de la variable

- Requiere un valor

#### `--json`

Si el valor de la variable tiene formato JSON

- Predeterminado: `false`
- Requiere un valor

#### `--sensitive`

Si el valor de la variable es confidencial

- Predeterminado: `false`
- Requiere un valor

#### `--enabled`

Si la variable debe habilitarse en el entorno

- Predeterminado: `true`
- Requiere un valor

#### `--inheritable`

Si los entornos secundarios heredan la variable

- Predeterminado: `true`
- Requiere un valor

#### `--visible-build`

Si la variable debe ser visible en el momento de la compilación

- Requiere un valor

#### `--visible-runtime`

Si la variable debe ser visible durante la ejecución

- Predeterminado: `true`
- Requiere un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--no-wait`, `-W`

No espere a que se complete la operación

- Predeterminado: `false`
- No acepta un valor

#### `--wait`

Espere a que se complete la operación (valor predeterminado)

- Predeterminado: `false`
- No acepta un valor


## `worker:list`

```bash
magento-cloud workers [--refresh] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

Obtener una lista de todos los trabajadores implementados

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--refresh`

Si se actualiza la caché

- Predeterminado: `false`
- No acepta un valor

#### `--pipe`

Mostrar solo una lista de nombres de trabajador

- Predeterminado: `false`
- No acepta un valor

#### `--project`, `-p`

El ID o la URL del proyecto

- Requiere un valor

#### `--environment`, `-e`

El ID del entorno. Utilice &quot;.&quot; para seleccionar el entorno predeterminado del proyecto.

- Requiere un valor

#### `--format`

El formato de salida: tabla, csv, tsv o sin formato

- Predeterminado: `table`
- Requiere un valor

#### `--columns`, `-c`

Columnas para mostrar. Columnas disponibles: comandos, nombre, tipo. Los caracteres % o * se pueden usar como comodín. Los valores se pueden dividir por comas (por ejemplo, &quot;a, b, c&quot;) y/o espacios en blanco.

- Predeterminado: `[]`
- Requiere un valor

#### `--no-header`

No generar el encabezado de tabla

- Predeterminado: `false`
- No acepta un valor
