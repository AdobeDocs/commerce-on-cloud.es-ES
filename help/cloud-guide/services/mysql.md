---
title: Configurar el servicio MySQL
description: Aprenda a administrar el servicio MySQL para el almacenamiento de datos persistentes con Adobe Commerce en la infraestructura en la nube.
feature: Cloud, Services, Storage
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '773'
ht-degree: 1%

---

# Configurar el servicio MySQL

El servicio `mysql` proporciona almacenamiento de datos persistente basado en [MariaDB](https://mariadb.com/) versiones 10.2 a 10.4, compatible con el motor de almacenamiento [XtraDB](https://docs.percona.com/percona-xtradb-cluster/8.0/index.html) y funciones reimplementadas de MySQL 5.6 y 5.7.

La reindexación en MariaDB 10.4 tarda más tiempo en comparación con otras versiones de MariaDB o MySQL. Consulte [Indexadores](https://experienceleague.adobe.com/docs/commerce-operations/performance-best-practices/configuration.html#indexers) en la guía de _Prácticas recomendadas de rendimiento_.

>[!WARNING]
>
>Tenga cuidado al actualizar MariaDB de la versión 10.1 a la 10.2. MariaDB 10.1 es la última versión que admite _XtraDB_ como motor de almacenamiento. MariaDB 10.2 usa _InnoDB_ para el motor de almacenamiento. Después de actualizar de 10.1 a 10.2, no podrá revertir el cambio. Adobe Commerce admite ambos motores de almacenamiento; sin embargo, debe comprobar las extensiones y otros sistemas utilizados por el proyecto para asegurarse de que son compatibles con MariaDB 10.2. Ver [Cambios incompatibles entre 10.1 y 10.2](https://mariadb.com/kb/en/upgrading-from-mariadb-101-to-mariadb-102/#incompatible-changes-between-101-and-102).

{{service-instruction}}

**Para habilitar MySQL**:

1. Agregue el nombre, tipo y valor de disco necesarios (en MB) al archivo `.magento/services.yaml`.

   ```yaml
   mysql:
       type: mysql:<version>
       disk: 5120
   ```

   >[!TIP]
   >
   >Los errores de MySQL, como `PDO Exception: MySQL server has gone away`, pueden producirse como resultado de la falta de espacio en disco. Compruebe que ha asignado suficiente espacio en disco al servicio en el archivo [`.magento/services.yaml`](services-yaml.md#disk).

1. Configure las relaciones en el archivo `.magento.app.yaml`.

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

1. Agregue, confirme e inserte los cambios de código.

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable mysql service" && git push origin <branch-name>
   ```

1. [Compruebe las relaciones de servicio](services-yaml.md#service-relationships).

{{service-change-tip}}

## Configurar la base de datos MySQL

Dispone de las siguientes opciones al configurar la base de datos MySQL:

- **`schemas`**: un esquema define una base de datos. El esquema predeterminado es la base de datos `main`.
- **`endpoints`**: cada extremo representa una credencial con privilegios específicos. El extremo predeterminado es `mysql`, que tiene acceso de `admin` a la base de datos `main`.
- **`properties`**: las propiedades se utilizan para definir configuraciones de base de datos adicionales.

El siguiente es un ejemplo básico de configuración en el archivo `.magento/services.yaml`:

```yaml
mysql:
    type: mysql:10.4
    disk: 5120
    configuration:
        properties:
            optimizer_switch: "rowid_filter=off"
            optimizer_use_condition_selectivity: 1
```

El `properties` del ejemplo anterior modifica la configuración predeterminada de `optimizer` como se recomienda en la guía de prácticas recomendadas de rendimiento](https://experienceleague.adobe.com/docs/commerce-operations/performance-best-practices/configuration.html#indexers).[

**Opciones de configuración de MariaDB**:

| Opciones | Descripción | Valor predeterminado |
| -------------------- | --------------------------------------------------- | ------------------ |
| `default_charset` | Conjunto de caracteres predeterminado. | utf8mb4 |
| `default_collation` | Intercalación predeterminada. | utf8mb4_unicode_ci |
| `max_allowed_packet` | Tamaño máximo de los paquetes, en MB. Intervalo `1` a `100`. | 16 |
| `optimizer_switch` | Establezca valores para el optimizador de consultas. Ver [documentación de MariaDB](https://mariadb.com/kb/en/server-system-variables/#optimizer_switch). | |
| `optimizer_use_condition_selectivity` | Seleccione las estadísticas utilizadas por el optimizador. Intervalo `1` a `5`. Ver [documentación de MariaDB](https://mariadb.com/kb/en/server-system-variables/#optimizer_use_condition_selectivity). | 4 para 10.4 y versiones posteriores |

### Configurar varios usuarios de base de datos

Opcionalmente, puede configurar varios usuarios con permisos diferentes para tener acceso a la base de datos `main`.

De manera predeterminada, hay un extremo denominado `mysql` que tiene acceso de administrador a la base de datos. Para configurar varios usuarios de base de datos, debe definir varios extremos en el archivo `services.yaml` y declarar las relaciones en el archivo `.magento.app.yaml`. Para los entornos de ensayo y producción de Pro, [envíe un vale de soporte de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para solicitar al usuario adicional.

Utilice una matriz anidada para definir los puntos finales para el acceso específico del usuario. Cada extremo puede designar el acceso a uno o más esquemas (bases de datos) y diferentes niveles de permisos en cada uno.

Los niveles de permiso válidos son:

- `ro`: solo se permiten consultas SELECT.
- `rw`: se permiten las consultas SELECT y las consultas INSERT, UPDATE y DELETE.
- `admin`: se permiten todas las consultas, incluidas las consultas DDL (CREATE TABLE, DROP TABLE, etc.).

Por ejemplo:

```yaml
mysql:
    type: mysql:10.4
    disk: 5120
    configuration:
        schemas:
            - main
        endpoints:
            admin:
                default_schema: main
                privileges:
                    main: admin
            reporter:
                privileges:
                    main: ro
            importer:
                privileges:
                    main: rw
        properties:
            optimizer_switch: "rowid_filter=off"
            optimizer_use_condition_selectivity: 1
```

En el ejemplo anterior, el extremo `admin` proporciona acceso de nivel de administrador a la base de datos `main`, el extremo `reporter` proporciona acceso de solo lectura y el extremo `importer` proporciona acceso de lectura y escritura, lo que significa que:

- El usuario `admin` tiene control total de la base de datos.
- El usuario `reporter` solo tiene privilegios SELECT.
- El usuario `importer` tiene privilegios de DELETE, SELECT, INSERT, UPDATE y.

Agregue los extremos definidos en el ejemplo anterior a la propiedad `relationships` del archivo `.magento.app.yaml`. Por ejemplo:

```yaml
relationships:
    database: "mysql:admin"
    databasereporter: "mysql:reporter"
    databaseimporter: "mysql:importer"
```

>[!NOTE]
>
>Si configura un usuario MySQL, no puede utilizar el mecanismo de control de acceso [`DEFINER`](https://dev.mysql.com/doc/refman/8.0/en/show-grants.html) para procedimientos y vistas almacenados.

## Conexión a la base de datos

El acceso a la base de datos MariaDB requiere directamente que utilice un SSH para iniciar sesión en el entorno remoto de la nube y conectarse a la base de datos.

1. Utilice SSH para iniciar sesión en el entorno remoto.

   ```bash
   magento-cloud ssh
   ```

1. Recupere las credenciales de inicio de sesión de MySQL de las propiedades `database` y `type` en la variable [$MAGENTO_CLOUD_RELATIONSHIPS](../application/properties.md#relationships).

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   o

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   En la respuesta, busque la información de MySQL. Por ejemplo:

   ```json
   "database" : [
      {
         "password" : "",
         "rel" : "mysql",
         "hostname" : "nnnnnnnn.mysql.service._.magentosite.cloud",
         "service" : "mysql",
         "host" : "database.internal",
         "ip" : "###.###.###.###",
         "port" : 3306,
         "path" : "main",
         "cluster" : "projectid-integration-id",
         "query" : {
            "is_master" : true
         },
         "type" : "mysql:10.3",
         "username" : "user",
         "scheme" : "mysql"
      }
   ],
   ```

1. Conéctese a la base de datos.

   - Para empezar, utilice el siguiente comando:

     ```bash
     mysql -h database.internal -u <username>
     ```

   - Para Pro, utilice el siguiente comando con el nombre de host, el número de puerto, el nombre de usuario y la contraseña recuperados de la variable `$MAGENTO_CLOUD_RELATIONSHIPS`.

     ```bash
     mysql -h <hostname> -P <number> -u <username> -p'<password>'
     ```

>[!TIP]
>
>Puede usar el comando `magento-cloud db:sql` para conectarse a la base de datos remota y ejecutar comandos SQL.

## Conectar con base de datos secundaria

>[!IMPORTANT]
>
>Esta función solo está disponible en los clústeres de ensayo y producción profesional.

A veces, debe conectarse a la base de datos secundaria para mejorar el rendimiento de la base de datos o resolver los problemas de bloqueo de la base de datos. Si se requiere esta configuración, use `"port" : 3304` para establecer la conexión. Consulte el tema [Práctica recomendada para configurar la conexión esclava MySQL](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration.html) en la guía _Prácticas recomendadas de implementación_.

## Resolución de problemas

Consulte los siguientes artículos de soporte de Adobe Commerce para obtener ayuda con la resolución de problemas de MySQL:

- [Comprobando consultas y procesos lentos en MySQL](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/database/checking-slow-queries-and-processes-mysql.html)
- [Crear volcado de base de datos en la nube](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/create-database-dump-on-cloud.html)
- [Solución de problemas de la herramienta de migración de datos](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/data-migration-tool-troubleshooting.html)
- [actualización de Adobe Commerce: compacta a tablas dinámicas 2.2.x, 2.3.x a 2.4.x](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/maintenance/commerce-235-upgrade-prerequisites-mariadb.html)
