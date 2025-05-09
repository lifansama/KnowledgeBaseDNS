---
title: Sintaxis de las reglas de filtrado DNS
sidebar_position: 2
toc_min_heading_level: 2
toc_max_heading_level: 4
---

:::info

Aquí te mostramos cómo escribir reglas de filtrado DNS personalizadas para uso en productos AdGuard

Quick links: [Download AdGuard Ad Blocker](https://agrd.io/download-kb-adblock), [Get AdGuard Home](https://github.com/AdguardTeam/AdGuardHome#getting-started), [Try AdGuard DNS](https://agrd.io/download-dns)

:::

## Introduction {#introduction}

Puedes usar la sintaxis de las reglas de filtrado DNS de AdGuard para hacer que las reglas sean más flexibles, de esa manera pueden bloquear contenidos según tus preferencias. La sintaxis de las reglas de filtrado DNS de AdGuard puede ser usada en diferentes productos de AdGuard como AdGuard Home, AdGuard DNS y AdGuard para Windows/Mac/Android.

Hay tres diferentes enfoques para escribir listas de bloqueo de hosts:

- [Adblock-style syntax][]: the modern approach to writing filtering rules based on using a subset of the Adblock-style rule syntax. De esta manera, las listas de bloqueo creadas son compatibles con las de los bloqueadores de anuncios para navegador.

- [`/etc/hosts` Sintaxis](#etc-hosts-syntax): el enfoque antiguo y probado, que usa la misma sintaxis que los sistemas operativos usan para sus archivos de hosts.

- [Sintaxis de dominios](#domains-only-syntax): una simple lista de nombres de dominio.

If you are creating a blocklist, we recommend using the [Adblock-style syntax][]. Esta sintaxis posee algunas ventajas importantes respecto a la sintaxis antigua:

- **Tamaño de las listas de bloqueo.** El uso de comparación de patrones le permite tener una sola regla en lugar de cientos de entradas `/etc/hosts`.

- **Compatibilidad.** Su lista de bloqueo será compatible con los bloqueadores de anuncios para navegador, y será más fácil compartir reglas con una lista de filtros de navegador.

- **Extensibilidad.** Durante la última década, la sintaxis de estilo Adblock ha evolucionado enormemente, y no vemos por qué no extenderla aún más y proporcionar funciones adicionales para los bloqueadores de toda la red.

Si estás manteniendo una lista de bloqueo de estilo `/etc/hosts`, o múltiples listas de filtrado (independientemente del tipo), proporcionamos una herramienta para la compilación de listas de bloqueo. We named it [Hostlist compiler][] and we use it ourselves to create [AdGuard DNS filter][].

## Basic examples {#basic-examples}

- `||example.org^`: bloquea el acceso al dominio `example.org` y todos sus subdominios, como `www.example.org`.

- `@@||example.org^`: desbloquea el acceso al dominio `example.org` y todos sus subdominios.

- `1.2.3.4 example.org`: (atención, sintaxis estilo `/etc/hosts` antiguo) en AdGuard Home, el programa responde con `1.2.3.4` a las consultas para el dominio `example.org`, pero **no** a sus subdominios. En el DNS privado de AdGuard, bloquea el acceso a `example.org`. `www.example.org` sigue estando permitido.

  En AdGuard Home, usar la dirección IP no especificada (`0.0.0.0`) o una dirección local (`127.0.0.1` y similares) para un host es básicamente lo mismo que bloquear ese host.

  ```none
  # Devuelve la dirección IP 1.2.3.4 para example.org.
  1.2.3.4 example.org
  # Bloquea example.org respondiendo con 0.0.0.0.
  0.0.0.0 example.org
  ```

- `example.org`: una regla de dominio simple. Bloquea el dominio `example.org` pero **no** sus subdominios. `www.example.org` sigue estando permitido.

- `! Es un comentario ` y `# También un comentario`: comentarios.

- `/REGEX/`: bloquea el acceso a los dominios que coincidan con la expresión regular especificada.

## Adblock-style syntax {#adblock-style-syntax}

This is a subset of the [traditional Adblock-style syntax][] which is used by browser ad blockers.

```none
     rule = ["@@"] pattern [ "$" modifiers ]
modifiers = [modifier0, modifier1[, ...[, modifierN]]]
```

- `pattern` (patrón): la máscara del nombre de host. Cada nombre de host se compara con esta máscara. El patrón también puede contener caracteres especiales, que se describen a continuación.

- `@@`: el marcador que se utiliza en las reglas de excepción. Inicie su regla con este marcador si desea desactivar el filtrado para los nombres de host que coincidan.

- `modifiers` (modificadores): parámetros que aclaran la regla. Pueden limitar el alcance de la norma o incluso cambiar por completo su funcionamiento.

### Special characters {#special-characters}

- `*`: el carácter comodín. Se utiliza para representar cualquier conjunto de caracteres. Esto también puede ser una cadena de texto vacía o una cadena de texto de cualquier longitud.

- `||`: coincide con el comienzo de un nombre de host, incluido cualquier subdominio. Por ejemplo, `||example.org` coincide con `example.org` y `test.example.org` pero no con `testexample.org`.

- `^`: el carácter de separador. A diferencia del bloqueo de anuncios para navegador, no hay nada que separar en un nombre de host, por lo que el único propósito de este carácter es marcar el final del nombre de host.

- `|`: un puntero al principio o al final del nombre de host. El valor depende de la ubicación del carácter. Por ejemplo, la regla `ample.org|` corresponde a `example.org` pero no a `example.org.com`. `|example` corresponde a `example.org` pero no a `test.example`.

### Regular expressions {#regular-expressions}

If you want even more flexibility in making rules, you can use [regular expressions][regexp] instead of the default simplified matching syntax. Si desea utilizar una expresión regular, el patrón debe verse así:

```none
pattern = "/" regexp "/"
```

**Ejemplos:**

- `/example.*/` bloqueará los hosts que coincidan con la regexp `example.*`.

- `@@/example.*/$important` desbloqueará los hosts que coincidan con la regexp `example.*`. Tenga en cuenta que esta regla también implica el modificador `important` (importante).

### Comments {#comments}

Cualquier línea que comience con un signo de exclamación o de almohadilla es un comentario y será ignorado por el motor de filtrado. Los comentarios suelen colocarse encima de las reglas y se utilizan para describir lo que hace una regla.

**Ejemplo:**

```none
! Esto es un comentario.
# Esto también es un comentario.
```

### Rule modifiers {#rule-modifiers}

Puedes cambiar el comportamiento de una regla agregando modificadores. Los modificadores deben situarse al final de la regla después del carácter `$` y estar separados por comas.

**Ejemplos:**

- ```none ||example.org^$important
   ```

  `||example.org^` es el patrón de coincidencia. `$` es el delimitador, que señala que el resto de la regla son modificadores. El modificador es "importante".

- You may want to use multiple modifiers in a rule. En ese caso, sepáralos con comas:

  ```none
  ||example.org^$client=127.0.0.1,dnstype=A
  ```

  `||example.org^` es el patrón de coincidencia. `$` es el delimitador, que señala que el resto de la regla son modificadores. `client=127.0.0.1` is the [`client`][] modifier with its value, `127.0.0.1`. `,` is the delimiter between modifiers. And finally, `dnstype=A` is the [`dnstype`][] modifier with its value, `A`.

**NOTA:** Si una regla contiene un modificador que no aparece en este documento, toda la regla **debe ser ignorada**. De este modo, evitamos los falsos positivos cuando alguien intenta utilizar listas de filtros para bloqueadores de anuncios sin modificarlas, como EasyList o EasyPrivacy.

#### `client` {#client-modifier}

El modificador `client` (cliente) permite especificar los clientes a los que se aplica esta regla. Hay dos formas principales de identificar a un cliente:

- Por su dirección IP o prefijo CIDR. Esta forma funciona para todo tipo de clientes.

- Por su nombre. Este modo sólo funciona para los clientes persistentes (en AdGuard Home) y los dispositivos (en Private AdGuard DNS), que usted ha añadido manualmente.

  **NOTA:** En AdGuard Home, actualmente no se admiten ClientIDs, sólo nombres. Si ha añadido un cliente con el nombre "Mi Cliente" y ClientID `mi-cliente` escriba su modificador como `$client='Mi Cliente'` en lugar de `$client=mi-cliente`.

La sintaxis es:

```none
$client=valor1|valor2|...
```

También puedes excluir clientes añadiendo un carácter `~` antes del valor. En este caso, la regla no se aplica a las solicitudes DNS de este cliente.

```none
$client=~valor1
```

Los nombres de los clientes suelen contener espacios u otros caracteres especiales, por lo que debes colocar el nombre entre comillas. Se admiten comillas ASCII simples y dobles. Utiliza la barra invertida (`\`) para escapar de las comillas (`"` y `'`), comas (`,`) y barras verticales (`|`).

**NOTA:** Cuando excluyes a un cliente, debes **** colocar `~` fuera de las comillas.

**Ejemplos:**

- `@@||*^$client=127.0.0.1`: desbloquea todo para localhost.

- `||example.org^$client="Frank\'s laptop"`: bloquea `example.org` sólo para el cliente llamado `"Frank\'s laptop"`. Ten en cuenta que las comillas (`"`) en el nombre deben escaparse.

- `||example.org^$client=~'Mary\'s\, John\'s\, and Boris\'s laptops'`: block `example.org` para todos excepto para el cliente llamado `Mary's, John's, and Boris's laptops`. Ten en cuenta que la coma (`,`) también debe escaparse.

- `||example.org^$client=~Mom|~Dad|Kids`: bloque `example.org` para `Kids`, pero no para `Mom` y `Dad`. Este ejemplo demuestra cómo especificar varios clientes en una regla.

- `||example.org^$client=192.168.0.0/24`: block `example.org` para todos los clientes con direcciones IP en el rango de `192.168.0.0` a `192.168.0.255`.

#### `denyallow` {#denyallow-modifier}

Puedes usar el modificador `denyallow` para excluir dominios de la regla de bloqueo. Para agregar múltiples dominios a una regla, usa el carácter `|` como separador.

La sintaxis es:

```none
$denyallow=domain1|domain2|...
```

Este modificador permite evitar crear reglas de excepción innecesarias cuando nuestra regla de bloqueo abarca demasiados dominios. Es posible que desees bloquear todo excepto un par de dominios TLD. Puedes utilizar el enfoque estándar, es decir, reglas como ésta:

```none
! Bloquear todo.
/.*/

! Desbloquee un par de TLD.
@@||com^
@@||net^
```

El problema con este enfoque es que de esta manera también desbloquearás los dominios de seguimiento que se encuentran en esos TLD (es decir, `google-analytics.com`). Aquí se explica cómo resolver esto con `denyallow`:

```none
*$denyallow=com|net
```

**Ejemplos:**

- `*$denyallow=com|net`: bloquea todo excepto `*.com` y `*.net`.

- `@@*$denyallow=com|net`: desbloquea todo excepto `*.com` y `*.net`.

- `||example.org^$denyallow=sub.example.org`. bloquea `example.org` y `*.example.org` pero no bloquea `sub.example.org`.

#### `dnstype` {#dnstype-modifier}

El modificador `dnstype` permite especificar el tipo de solicitud o respuesta de DNS en el que se activará esta regla.

La sintaxis es:

```none
$dnstype=value1|value2|...
$dnstype=~value1|~value2|~...
```

Los nombres de los tipos no distinguen entre mayúsculas y minúsculas, pero se validan con un conjunto de tipos de registros de recursos DNS (RR) reales.

No combines las reglas de exclusión con las de inclusión. Esto:

```none
$dnstype=~value1|value2
```

es equivalente a esto:

```none
$dnstype=value2
```

**Ejemplos:**

- `||example.org^$dnstype=AAAA`: bloquea las consultas de DNS para las direcciones IPv6 de `example.org`.

- `||example.org^$dnstype=~A|~CNAME`: solo permite consultas DNS `A` y `CNAME` para `example.org`, bloquea el resto.

**NOTA:** Antes de la versión **v0.108.0,** AdGuard Home usaba el tipo de solicitud para filtrar los registros de respuesta, a diferencia del tipo de registro de respuesta en sí.  Eso causó problemas, ya que significaba que no podía escribir reglas que permitieran ciertos registros `CNAME` en respuestas en solicitudes `A` y `AAAA`. En **v0.108.0** se cambió ese comportamiento, así que ahora esto:

```none
||canon.example.com^$dnstype=~CNAME
```

te permite evitar el filtrado de la siguiente respuesta:

```none
RESPUESTAS:
->  ejemplo.com
    nombre canónico = canon.ejemplo.com.
    ttl = 60
->  canon.example.com
    dirección de Internet = 1.2.3.4
    ttl = 60
```

#### `dnsrewrite` {#dnsrewrite-modifier}

El modificador de respuesta `dnsrewrite` permite reemplazar el contenido de la respuesta a la solicitud de DNS para los hosts coincidentes. Ten en cuenta que este modificador en AdGuard Home funciona en todas las reglas, pero en Private AdGuard DNS, solo en las personalizadas.

**Rules with the `dnsrewrite` response modifier have higher priority than other rules in AdGuard Home and AdGuard DNS.**

Responses to all requests for a host matching a `dnsrewrite` rule will be replaced. The answer section of the replacement response will only contain RRs that match the request's query type and, possibly, CNAME RRs. Note that this means that responses to some requests may become empty (`NODATA`) if the host matches a `dnsrewrite` rule.

La sintaxis abreviada es:

```none
$dnsrewrite=1.2.3.4
$dnsrewrite=abcd::1234
$dnsrewrite=example.net
$dnsrewrite=REFUSED
```

Las palabras clave DEBEN estar en mayúsculas (por ejemplo `NOERROR`). Las reescrituras de palabras clave tienen prioridad sobre las demás y darán como resultado una respuesta vacía con un código de respuesta adecuado.

La sintaxis completa es de la forma `RCODE;RRTYPE;VALUE`:

```none
$dnsrewrite=NOERROR;A;1.2.3.4
$dnsrewrite=NOERROR;AAAA;abcd::1234
$dnsrewrite=NOERROR;CNAME;example.net
$dnsrewrite=REFUSED;;
```

El modificador `$dnsrewrite` con el código de respuesta `NOERROR` también puede tener campos `RRTYPE` y `VALUE` vacíos.

El `CNAME` es especial porque AdGuard Home resolverá el host y añadirá su información a la respuesta. Es decir, si `example.net` tiene IP `1.2.3.4`, y el usuario tiene esto en sus reglas de filtro:

```none
||ejemplo.com^$dnsrewrite=ejemplo.net
! O:
||ejemplo.com^$dnsrewrite=NOERROR;CNAME;ejemplo.net
```

entonces la respuesta será algo como:

```sh
nslookup example.com my.adguard.local
```

```none
Server: my.adguard.local
Address: 127.0.0.1#53

Non-authoritative answer:
example.com canonical name = example.net.
Name: example.net
Address: 1.2.3.4
```

A continuación, se reescribe el `CNAME`. Después, los valores de todos los demás registros se suman como una sola respuesta, por lo que esto:

```none
||ejample.com^$dnsrewrite=NOERROR;A;1.2.3.4
||ejample.com^$dnsrewrite=NOERROR;A;1.2.3.5
```

dará como resultado una respuesta con dos registros `A`.

Tipos de RR admitidos actualmente con ejemplos:

- `||4.3.2.1.in-addr.arpa^$dnsrewrite=NOERROR;PTR;example.net.` adds a `PTR` record for reverse DNS. Invertir solicitudes DNS para `1.2.3.4` al servidor DNS dará como resultado `example.net`.

  **NOTA:** la IP DEBE estar en orden inverso. See [RFC 1035][rfc1035].

- `||example.com^$dnsrewrite=NOERROR;A;1.2.3.4` agrega un registro `A` con el valor `1.2.3.4`.

- `||ejample.com^$dnsrewrite=NOERROR;AAAA;abcd::1234` añade un registro `AAAA` con el valor `abcd::1234`.

- `||example.com^$dnsrewrite=NOERROR;CNAME;example.org` agrega un registro `CNAME`. Consulta la explicación anterior.

- `||example.com^$dnsrewrite=NOERROR;HTTPS;32 example.com alpn=h3` agrega un registro `HTTPS`. Sólo se admite un subconjunto de valores de parámetros: los valores deben ser `contiguos` y, cuando `` se `espera` una lista de valores, actualmente sólo se admite un valor:

   ```none
   ipv4hint=127.0.0.1             // Compatible.
   ipv4hint="127.0.0.1"           // No compatible.
   ipv4hint=127.0.0.1,127.0.0.2   // No compatible.
   ipv4hint="127.0.0.1,127.0.0.2" // No compatible.
   ```

  Esto cambiará en el futuro.

- `||ejample.com^$dnsrewrite=NOERROR;MX;32 example.mail` añade un registro `MX` con valor de precedencia `32` y valor de intercambio `example.mail`.

- `||ejemplo.com^$dnsrewrite=NOERROR;SVCB;32 ejemplo.com alpn=h3` añade un valor `SVCB`. Consulta el ejemplo anterior de `HTTPS`.

- `||ejample.com^$dnsrewrite=NOERROR;TXT;hello_world` añade un registro `TXT` con el valor `hello_world`.

- `||_svctype._tcp.example.com^$dnsrewrite=NOERROR;SRV;10 60 8080 example.com` agrega un registro `SRV` con valor de prioridad `10`, valor de peso `60`, puerto`8080`y destino valor `ejemplo.com`.

- `||ejemplo.com^$dnsrewrite=DOMINIONX;;` responde con un código `NXDOMAIN`.

- `$dnstype=AAAA,denyallow=example.org,dnsrewrite=NOERROR;;` responde con un vacío `NOERROR` responde para todas las solicitudes `AAAA` excepto las de `example.org`.

Exception rules unblock one or all rules:

- `@@||example.com^$dnsrewrite` unblocks all DNS rewrite rules.

- `@@||example.com^$dnsrewrite=1.2.3.4` unblocks the DNS rewrite rule that adds an `A` record with the value `1.2.3.4`.

:::info

If you are maintaining a blocklist that is included in AdGuard DNS and AdGuard Home (i.e. included into [HostlistsRegistry][hostlistsregistry]), `$dnsrewrite` rules will be automatically filtered out. If these rules are required for your blocklist, please request permission by opening a new issue in the [HostlistsRegistry][hostlistsregistry] repo.

:::

#### `important` {#important-modifier}

The `important` modifier applied to a rule increases its priority over any other rule without the modifier. Even over basic exception rules.

**Ejemplos:**

- En este ejemplo:

  ```none
  ||example.org^$important
  @@||example.org^
  ```

  `||example.org^$important` bloqueará todas las solicitudes a `*.example.org` a pesar de la regla de excepción.

- En este ejemplo:

  ```none
  ||example.org^$important
  @@||example.org^$important
  ```

  la regla de excepción también tiene el modificador `importante`, por lo que funcionará.

#### `badfilter` {#badfilter-modifier}

The rules with the `badfilter` modifier disable other basic rules to which they refer. It means that the text of the disabled rule should match the text of the `badfilter` rule (without the `badfilter` modifier).

**Ejemplos:**

- `||ejemplo.com$badfilter` deshabilita `||ejemplo.com`.

- `@@||example.org^$badfilter` deshabilita `@@||example.org^`.

  **NOTA:** El modificador `badfilter` actualmente no funciona con `/etc/hosts`reglas de estilo. `127.0.0.1 example.org$badfilter` **no** deshabilitará la regla original `127.0.0.1 example.org`.

#### `ctag` {#ctag-modifier}

**The `ctag` modifier can only be used in AdGuard Home.**

It allows to block domains only for specific types of DNS client tags. You can assign tags to clients in the AdGuard Home UI. In the future, we plan to assign tags automatically by analyzing the behavior of each client.

La sintaxis es:

```none
$ctag=valor1|valor2|...
```

If one of client's tags matches the `ctag` values, this rule applies to the client. The syntax for exclusion is:

```none
$ctag=~valor1|~valor2|...
```

If one of client's tags matches the exclusion `ctag` values, this rule doesn't apply to the client.

**Ejemplos:**

- `||example.org^$ctag=device_pc|device_phone`: bloquea `example.org` para clientes taggeados como `device_pc` o `device_phone`.

- `||example.org^$ctag=~device_phone`:Bloquear `example.org` para todos los clientes excepto aquellos etiquetados como `device_phone`.

The list of allowed tags:

- Por tipo de dispositivo:

    - `device_audio`: dispositivos de audio.
    - `device_camera`: cámaras.
    - `device_gameconsole`: consolas de juegos.
    - `device_laptop`: laptops.
    - `device_nas`: NAS (Network-attached Storages).
    - `device_pc`: PCs.
    - `device_phone`: celulares.
    - `device_printer`: impresoras.
    - `device_securityalarm`: alarmas de seguridad.
    - `device_tablet`: tabletas.
    - `device_tv`: televisores.
    - `device_other`: otros dispositivos.

- Por sistema operativo:

    - `os_android`: Android.
    - `os_ios`: iOS.
    - `os_linux`: Linux.
    - `os_macos`: macOS.
    - `os_windows`: Windows.
    - `os_other`: otros sistemas operativos.

- Por grupo de usuarios:

    - `user_admin`: administradores.
    - `user_regular`: usuarios regulares.
    - `user_child`: niños.

## `/etc/hosts`-style syntax {#etc-hosts-syntax}

For each host a single line should be present with the following information:

```none
IP_address canonical_hostname [aliases...]
```

Fields of the entries are separated by any number of space or tab characters. Text from the `#` character until the end of the line is a comment and is ignored.

Hostnames may contain only alphanumeric characters, hyphen-minus signs (`-`), and periods (`.`). They must begin with an alphabetic character and end with an alphanumeric character. Optional aliases provide for name changes, alternate spellings, shorter hostnames, or generic hostnames (for example, `localhost`).

**Ejemplo:**

```none
# Esto es un comentario
127.0.0.1 example.org ejemplo.info
127.0.0.1 example.org
127.0.0.1 example.org # esto también es un comentario
```

In AdGuard Home, the IP addresses are used to respond to DNS queries for these domains. In Private AdGuard DNS, these addresses are simply blocked.

## Domains-only syntax {#domains-only-syntax}

A simple list of domain names, one name per line.

**Ejemplo:**

```none
# Este es un comentario
example.org
example.org
example.org # esto también es un comentario
```

If a string is not a valid domain (e.g. `*.example.org`), AdGuard Home will consider it to be an [Adblock-style syntax][] rule.

## Hostlist compiler {#hostlist-compiler}

If you are maintaining a blocklist and use different sources in it, [Hostlist compiler][] may be useful to you. It is a simple tool that makes it easier to compile a hosts blocklist compatible with AdGuard Home, Private AdGuard DNS or any other AdGuard product with DNS filtering.

What it's capable of:

1. Compila una lista de bloqueo única de varias fuentes.

2. Excluye las reglas que no necesitas.

3. Limpia la lista resultante: deduplica, borra reglas no válidas y comprime la lista.

<!-- local links -->


<!-- external links -->
[hostlistsregistry]: https://github.com/AdguardTeam/HostlistsRegistry
[Adblock-style syntax]: #adblock-style-syntax
[`client`]: #client-modifier
[`dnstype`]: #dnstype-modifier
[AdGuard DNS filter]: https://github.com/AdguardTeam/AdGuardSDNSFilter
[Hostlist compiler]: https://github.com/AdguardTeam/HostlistCompiler
[regexp]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions
[rfc1035]: https://tools.ietf.org/html/rfc1035#section-3.5
[traditional Adblock-style syntax]: https://adguard.com/kb/general/ad-filtering/create-own-filters/
