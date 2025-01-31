---
title: Propiedad Firewall
description: Consulte ejemplos sobre cómo configurar la propiedad del cortafuegos en el archivo de configuración de la aplicación Commerce.
feature: Cloud, Configuration, Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '844'
ht-degree: 0%

---

# Propiedad Firewall

>[!IMPORTANT]
>
>Solo proyectos iniciales

En los proyectos iniciales, la propiedad `firewall` agrega un firewall _saliente_ a la aplicación. Este cortafuegos no afecta a las solicitudes entrantes. Define qué `tcp` solicitudes salientes pueden _dejar_ un sitio de Adobe Commerce. Esto se denomina filtrado de salida. El cortafuegos de salida filtra lo que puede salir: salir o escapar del sitio. Limitar lo que puede escapar añade una potente herramienta de seguridad al servidor.

## Directivas de restricción predeterminadas

El firewall proporciona dos directivas predeterminadas para controlar el tráfico saliente: `allow` y `deny`. La directiva `allow` _permite_ todo el tráfico saliente de forma predeterminada. Y la directiva `deny` _deniega_ todo el tráfico saliente de forma predeterminada. Sin embargo, al agregar una regla, se anula la directiva predeterminada y el firewall bloquea **todo** el tráfico saliente que no está permitido por la regla.

Para los planes de inicio, la directiva predeterminada se establece en `allow`. Esta configuración garantiza que todo el tráfico saliente actual permanezca desbloqueado hasta que agregue las reglas de filtrado de salida. La directiva predeterminada se puede establecer en `deny` si se solicita.

**Para comprobar su directiva predeterminada**:

```bash
magento-cloud p:curl --project PROJECT_ID /settings | grep -i outbound
```

A menos que haya solicitado `deny` para su directiva, el comando debe mostrar la directiva establecida en `allow`:

```json
"outbound_restrictions_default_policy": "allow"
```

>[!NOTE]
>
>**Comida para llevar clave**: Cuando agrega una regla de salida, bloquea todo el tráfico saliente excepto los dominios, las direcciones IP o los puertos que agrega a la regla. Por lo tanto, es importante tener una lista saliente completa definida y probada antes de agregarla al sitio de producción.

## Opciones de cortafuegos

En el siguiente ejemplo de configuración del archivo `.magento.app.yaml` se muestran todas las opciones de `firewall` que puede usar para agregar reglas para el filtrado de salida.

```yaml
firewall:
    outbound:
        - # Common accessed domains
            domains:
                - newrelic.com
                - fastly.com
                - magento.com
                - magentocommerce.com
                - google.com
            ports:
                - 80
                - 443
            protocol: tcp # Can be omitted from rules.

        - # Adobe Stock integration
            domains:
                - account.adobe.com
                - stock.adobe.com
                - console.adobe.io
            ports:
                - 80
                - 443

        - # Payment services
            domains:
                - braintreepayments.com
                - paypal.com
            ports:
                - 80
                - 443

        - # Shipping services
            domains:
                - ups.com
                - usps.com
                - fedex.com
                - dhl.com
            ports:
                - 80
                - 443

        - # Vertex Integrated Address Cleansing
            domains:
                - mgcsconnect.vertexsmb.com
            ports:
                - 80
                - 443

        - # New Relic networks
            ips:
                - 162.247.240.0/22 # US region accounts
                - 185.221.84.0/22 # EU region accounts
            ports:
                - 443

        - # New Relic endpoints
            domains:
                - collector.newrelic.com, # US region accounts
                - collector.eu01.nr-data.net # EU region accounts
            ports:
                - 443

        - # Fastly IP ranges
            ips:
                - 23.235.32.0/20
                - 43.249.72.0/22
                - 103.244.50.0/24
                - 103.245.222.0/23
                - 103.245.224.0/24
                - 104.156.80.0/20
                - 146.75.0.0/16
                - 151.101.0.0/16
                - 157.52.64.0/18
                - 167.82.0.0/17
                - 167.82.128.0/20
                - 167.82.160.0/20
                - 167.82.224.0/20
                - 172.111.64.0/18
                - 185.31.16.0/22
                - 199.27.72.0/21
                - 199.232.0.0/16
            ports:
                - 80
                - 443
```

## Reglas de filtrado de salida

Las configuraciones del cortafuegos saliente están formadas por reglas. Puede definir tantas reglas como necesite. Los requisitos para las reglas son los siguientes.

**Cada regla:**

- Debe comenzar con un guión (`-`). Añadir un comentario en la misma línea ayuda a documentar y separar visualmente una regla de la siguiente.
- Debe definir al menos una de las siguientes opciones: `domains`, `ips` o `ports`.
- Debe usar el protocolo `tcp`. Como este es el protocolo predeterminado para todas las reglas, puede omitirlo de la regla.
- Puede definir `domains` o `ips`, pero no ambos en la misma regla.
- Puede incluir `yaml` comentarios (`#`) y saltos de línea para organizar los dominios, las direcciones IP y los puertos permitidos.

Cada regla utiliza las siguientes propiedades:

### `domains`

La opción `domains` permite una lista de nombres de dominio completos (FQDN).

Si una regla define `domains` pero no `ports`, el firewall permite las solicitudes de dominio en cualquier puerto.

### `ips`

La opción `ips` permite una lista de direcciones IP en la notación CIDR. Puede especificar direcciones IP únicas o rangos de direcciones IP.

Para especificar una sola dirección IP, agregue el prefijo CIDR `/32` al final de su dirección IP:

```
172.217.11.174/32  # google.com
```

Para especificar un intervalo de direcciones IP, use la calculadora [Intervalo IP a CIDR](https://ipaddressguide.com/cidr).

Si una regla define `ips` pero no `ports`, el firewall permite las solicitudes de IP en cualquier puerto.

### `ports`

La opción `ports` permite una lista de puertos de 1 a 65535. Para la mayoría de las reglas del ejemplo, los puertos `80` y `443` permiten solicitudes HTTP y HTTPS. Pero para New Relic, las reglas solo permiten el acceso a dominios y direcciones IP en el puerto `443`, como se recomienda en la documentación de New Relic sobre [Tráfico de red](https://docs.newrelic.com/docs/new-relic-solutions/get-started/networks/#agents).

Si una regla solo define `ports`, el firewall permite el acceso a todos los dominios y direcciones IP de los puertos definidos.

>[!NOTE]
>
>El puerto `25`, el puerto SMTP para enviar correo electrónico, siempre está bloqueado, sin excepción.

### `protocol`

Como se ha mencionado, TCP es el protocolo predeterminado y el único permitido para las reglas. UDP y sus puertos no están permitidos. Por este motivo, puede omitir la opción `protocol` de todas las reglas. Si desea incluirlo de todos modos, debe establecer el valor en `tcp`, como se muestra en la primera regla del ejemplo.

## Búsqueda de nombres de dominio para permitir

Para ayudarle a identificar los dominios que desea incluir en las reglas de filtrado de salida, utilice el siguiente comando para analizar el archivo `dns.log` del servidor y mostrar una lista de todas las solicitudes DNS que ha registrado el sitio:

```shell
awk '($5 ~/query/)' /var/log/dns.log | awk '{print $6}' | sort | uniq -c | sort -rn
```

Este comando también muestra las solicitudes DNS realizadas pero bloqueadas por las reglas de filtrado de salida. La salida no muestra qué dominios se bloquearon, solo que se realizaron solicitudes. La salida no muestra las solicitudes realizadas mediante una dirección IP.

```
Example output:

97 magento.com
93 magentocommerce.com
88 google.com
70 metadata.google.internal.0
70 metadata.google.internal
65 newrelic.com
56 fastly.com
17 mcprod-0vunku5xn24ip.ap-4.magentosite.cloud
6 advancedreporting.rjmetrics.com
```

A diferencia de las direcciones IP, los dominios suelen ser más específicos y seguros para el filtrado de salidas. Por ejemplo, si agrega una dirección IP para un servicio que utiliza una CDN, está permitiendo la dirección IP para la CDN, que puede ser utilizada por cientos o miles de otros dominios. Con una dirección IP, podría estar permitiendo el acceso saliente a miles de otros servidores.

## Prueba de reglas de filtrado de salida

Después de recopilar y configurar las reglas de acceso para los dominios y las direcciones IP que necesita su sitio, es hora de realizar notificaciones push y pruebas.

Para probar las reglas de filtrado de salida:

1. Cree un script de shell de `curl` comandos para acceder a los dominios y las direcciones IP de las reglas. Incluya comandos que prueben el acceso a dominios y direcciones IP que deben bloquearse.

1. Configure un vínculo `post_deploy` en el archivo `.magento.app.yaml` para ejecutar el script.

1. Inserte la configuración de `firewall` y el script de prueba en la rama de `integration`.

1. Examine el resultado `post_deploy` de sus comandos `curl`.

1. Refine las reglas de `firewall`, actualice el script `curl`, confirme, inserte y repita.

### Ejemplo de script `curl`

```shell
# curl-tests-for-egress-filtering.sh

# Use the -v option to display connection details

# Check domain access
curl -v newrelic.com
curl -v fastly.com
curl -v magento.com
curl -v magentocommerce.com
curl -v google.com
curl -v account.adobe.com
curl -v stock.adobe.com
curl -v console.adobe.io
curl -v braintreepayments.com
curl -v paypal.com
curl -v ups.com
curl -v usps.com
curl -v fedex.com
curl -v dhl.com
curl -v devdocs.magento.com

# Check domain denials
curl -v amazon.com
curl -v facebook.com
curl -v twitter.com

# IP address access
...

# IP address denials
...
```

### Ejemplo de `post_deploy`

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: "php ./vendor/bin/ece-tools run scenario/deploy.xml\n"
    post_deploy: |
        set -e
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
        echo "[$(date)] post-deploy hook end"
        ./curl-tests-for-egress-filtering.sh
        echo "[$(date)] curl finished"
```
