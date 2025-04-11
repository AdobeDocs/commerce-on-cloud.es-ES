---
source-git-commit: 713008d26d05f05e3e39e33c0d0df766f4dbe0b0
workflow-type: tm+mt
source-wordcount: '921'
ht-degree: 3%

---
# ece-tools

**Versión**: 2002.2.3

Esta referencia contiene 34 comandos disponibles mediante la herramienta de línea de comandos `ece-tools`.
La lista inicial se genera automáticamente mediante el comando `ece-tools list` en Adobe Commerce en la infraestructura en la nube.

## General

Esta referencia se genera a partir del código base de la aplicación. Para cambiar el contenido, _envíenos su opinión_ (busque el vínculo en la esquina superior derecha). Para ver las directrices de contribución, consulte [Contribuciones de código](https://developer.adobe.com/commerce/contributor/guides/code-contributions/).

### Opciones globales

#### `--help`, `-h`

Muestra la ayuda del comando especificado. Cuando no se proporciona ningún comando, se muestra la ayuda para el comando de lista

- Predeterminado: `false`
- No acepta un valor

#### `--quiet`, `-q`

No generar ningún mensaje

- Predeterminado: `false`
- No acepta un valor

#### `--verbose`, `-v|-vv|-vvv`

Aumente el nivel de detalle de los mensajes: 1 para un resultado normal, 2 para un resultado más detallado y 3 para una depuración

- Predeterminado: `false`
- No acepta un valor

#### `--version`, `-V`

Mostrar esta versión de la aplicación

- Predeterminado: `false`
- No acepta un valor

#### `--ansi`

Forzar (o deshabilitar —sin ansi) la salida ANSI

- No acepta un valor

#### `--no-ansi`

Anule la opción &quot;—ansi&quot;

- Predeterminado: `false`
- No acepta un valor

#### `--no-interaction`, `-n`

No haga ninguna pregunta interactiva

- Predeterminado: `false`
- No acepta un valor


## `_complete`

```bash
ece-tools _complete [-s|--shell SHELL] [-i|--input INPUT] [-c|--current CURRENT] [-a|--api-version API-VERSION] [-S|--symfony SYMFONY]
```

Comando interno para proporcionar sugerencias de finalización de shell

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--shell`, `-s`

El tipo de contenedor (&quot;bash&quot;, &quot;fish&quot;, &quot;zsh&quot;)

- Requiere un valor

#### `--input`, `-i`

Una matriz de tokens de entrada (por ejemplo, COMP_WORDS o argv)

- Predeterminado: `[]`
- Requiere un valor

#### `--current`, `-c`

Índice de la matriz de &quot;entrada&quot; en la que se encuentra el cursor (p. ej. COMP_CWORD)

- Requiere un valor

#### `--api-version`, `-a`

Versión de API del script de finalización

- Requiere un valor

#### `--symfony`, `-S`

obsoleto

- Requiere un valor


## `build`

```bash
ece-tools build
```

Crea una aplicación.

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).


## `completion`

```bash
ece-tools completion [--debug] [--] [<shell>]
```

Volcar el script de finalización de shell

```
The completion command dumps the shell completion script required
to use shell autocompletion (currently, bash, fish, zsh completion are supported).

Static installation
-------------------

Dump the script to a global completion file and restart your shell:

    bin/ece-tools completion  | sudo tee /etc/bash_completion.d/ece-tools

Or dump the script to a local file and source it:

    bin/ece-tools completion  > completion.sh

    # source the file whenever you use the project
    source completion.sh

    # or add this line at the end of your "~/.bashrc" file:
    source /path/to/completion.sh

Dynamic installation
--------------------

Add this to the end of your shell configuration file (e.g. "~/.bashrc"):

    eval "$(/var/jenkins/workspace/gendocs-ece-tools-cli/bin/ece-tools completion )"
```

### Argumentos

#### `shell`

El tipo de shell (p. ej. &quot;bash&quot;), el valor de la variable env &quot;$SHELL&quot; se usará si no se proporciona

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--debug`

Seguimiento del registro de depuración de finalización

- Predeterminado: `false`
- No acepta un valor


## `db-dump`

```bash
ece-tools db-dump [-d|--remove-definers] [-a|--dump-directory DUMP-DIRECTORY] [--] [<databases>...]
```

Crea copias de seguridad de base de datos.

### Argumentos

#### `databases`

Bases de datos para backup. Valores disponibles: [ventas de presupuesto principal]. Si no se especifica el valor del argumento, se crearán copias de seguridad de la base de datos con las credenciales almacenadas en la variable de entorno `MAGENTO_CLOUD_RELATIONSHIP` o con la propiedad `stage.deploy.DATABASE_CONFIGURATION` del archivo de configuración .magento.env.yaml.

- Predeterminado: `[]`
- Matriz

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--remove-definers`, `-d`

Quitar definidores del volcado de la base de datos

- Predeterminado: `false`
- No acepta un valor

#### `--dump-directory`, `-a`

Usar directorio alternativo para guardar el volcado

- Requiere un valor


## `deploy`

```bash
ece-tools deploy
```

Implementa la aplicación.

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).


## `help`

```bash
ece-tools help [--format FORMAT] [--raw] [--] [<command_name>]
```

Mostrar la ayuda de un comando

```
The help command displays help for a given command:

  bin/ece-tools help list

You can also output the help in other formats by using the --format option:

  bin/ece-tools help --format=xml list

To display the list of available commands, please use the list command.
```

### Argumentos

#### `command_name`

El nombre del comando

- Predeterminado: `help`

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--format`

El formato de salida (txt, xml, json o md)

- Predeterminado: `txt`
- Requiere un valor

#### `--raw`

Para generar la ayuda del comando raw

- Predeterminado: `false`
- No acepta un valor


## `list`

```bash
ece-tools list [--raw] [--format FORMAT] [--short] [--] [<namespace>]
```

Comandos de lista

```
The list command lists all commands:

  bin/ece-tools list

You can also display the commands for a specific namespace:

  bin/ece-tools list test

You can also output the information in other formats by using the --format option:

  bin/ece-tools list --format=xml

It's also possible to get raw list of commands (useful for embedding command runner):

  bin/ece-tools list --raw
```

### Argumentos

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

#### `--short`

Para omitir la descripción de argumentos de comandos

- Predeterminado: `false`
- No acepta un valor


## `patch`

```bash
ece-tools patch
```

Aplica parches personalizados.

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).


## `post-deploy`

```bash
ece-tools post-deploy
```

Realiza operaciones después de la implementación.

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).


## `run`

```bash
ece-tools run <scenario>...
```

Ejecución de escenarios.

### Argumentos

#### `scenario`

Escenario(s)

- Predeterminado: `[]`
- Requerido

- Matriz

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).


## `backup:list`

```bash
ece-tools backup:list
```

Muestra la lista de archivos de copia de seguridad.

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).


## `backup:restore`

```bash
ece-tools backup:restore [-f|--force] [--file [FILE]]
```

Restaure los archivos de configuración importantes. Ejecutar copia de seguridad: lista para mostrar la lista de archivos de copia de seguridad.

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--force`, `-f`

Sobrescribir archivos existentes al restaurar una copia de seguridad

- Predeterminado: `false`
- No acepta un valor

#### `--file`

Una ruta de recuperación de archivos específica

- Acepta un valor


## `build:generate`

```bash
ece-tools build:generate
```

Genera todos los archivos necesarios para la fase de compilación.

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).


## `build:transfer`

```bash
ece-tools build:transfer
```

Transfiere los archivos generados al directorio init.

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).


## `cloud:config:create`

```bash
ece-tools cloud:config:create <configuration>
```

Crea un archivo de `.magento.env.yaml` con la configuración de variables especificada de compilación, implementación y posterior a la implementación. Sobrescribe cualquier archivo `.magento.env.yaml` existente.

### Argumentos

#### `configuration`

Configuración en formato JSON

- Requerido

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).


## `cloud:config:update`

```bash
ece-tools cloud:config:update <configuration>
```

Actualiza el archivo `.magento.env.yaml` existente con la configuración especificada. Crea `.magento.env.yaml` archivo si no existe.

### Argumentos

#### `configuration`

Configuración en formato JSON

- Requerido

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).


## `cloud:config:validate`

```bash
ece-tools cloud:config:validate
```

Valida el archivo de configuración `.magento.env.yaml`

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).


## `config:dump`

```bash
ece-tools config:dumpdump
```

Configuración de volcado para la implementación de contenido estático.

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).


## `cron:disable`

```bash
ece-tools cron:disable
```

Deshabilite todos los procesos cron de Magento y finalice todos los procesos en ejecución.

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).


## `cron:enable`

```bash
ece-tools cron:enable
```

Habilita los procesos cron de Magento.

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).


## `cron:kill`

```bash
ece-tools cron:kill
```

Termina todos los procesos cron de Magento.

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).


## `cron:unlock`

```bash
ece-tools cron:unlock [--job-code [JOB-CODE]]
```

Desbloquee los trabajos cron que se quedaron en estado &quot;en ejecución&quot;.

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--job-code`

Código de trabajo Cron para desbloquear.

- Predeterminado: `[]`
- Acepta varios valores


## `dev:generate:schema-error`

```bash
ece-tools dev:generate:schema-error
```

Genera el archivo dist/error-codes.md a partir del archivo schema.error.yaml.

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).


## `dev:git:update-composer`

```bash
ece-tools dev:git:update-composer
```

Actualiza el Compositor para la implementación desde Git.

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).


## `env:config:show`

```bash
ece-tools env:config:show [<variable>...]
```

Muestre variables de entorno de configuración de nube codificadas.

### Argumentos

#### `variable`

Variables de entorno para mostrar, posibles opciones: servicios, rutas, variables

- Predeterminado: `[]`
- Matriz

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).


## `error:show`

```bash
ece-tools error:show [-j|--json] [--] [<error-code>]
```

Muestra información sobre errores por ID de error o sobre todos los errores de la última implementación.

### Argumentos

#### `error-code`

Código de error, si no se ha pasado el comando, se muestra información sobre todos los errores de la última implementación

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).

#### `--json`, `-j`

Se utiliza para obtener los resultados en formato JSON

- Predeterminado: `false`
- No acepta un valor


## `module:refresh`

```bash
ece-tools module:refresh
```

Actualiza la configuración para habilitar los módulos recién añadidos.

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).


## `schema:generate`

```bash
ece-tools schema:generate
```

Genera el archivo de esquema *.dist.

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).


## `wizard:ideal-state`

```bash
ece-tools wizard:ideal-state
```

Comprueba el estado ideal de configuración.

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).


## `wizard:master-slave`

```bash
ece-tools wizard:master-slave
```

Comprueba la configuración maestro-esclavo.

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).


## `wizard:scd-on-build`

```bash
ece-tools wizard:scd-on-build
```

Comprueba el SCD en la configuración de compilación.

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).


## `wizard:scd-on-demand`

```bash
ece-tools wizard:scd-on-demand
```

Comprueba la configuración de SCD bajo demanda.

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).


## `wizard:scd-on-deploy`

```bash
ece-tools wizard:scd-on-deploy
```

Comprueba el SCD en la configuración de implementación.

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).


## `wizard:split-db-state`

```bash
ece-tools wizard:split-db-state
```

Comprueba la capacidad de dividir la base de datos y si ya se ha dividido o no.

### Opciones

Para ver las opciones globales, consulte [Opciones globales](#global-options).
