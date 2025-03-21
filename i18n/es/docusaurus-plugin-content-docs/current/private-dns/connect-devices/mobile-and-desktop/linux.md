---
title: Linux
sidebar_position: 6
---

Para conectar un dispositivo Linux a AdGuard DNS, primero agrégalo a _Dashboard_:

1. Ve a _Dashboard_ y haz clic en _Conectar nuevo dispositivo_.
2. En el menú desplegable _Tipo de dispositivo_, selecciona Linux.
3. Dale un nombre al dispositivo.
    ![Conectando dispositivo \*mobile_border](https://cdn.adtidy.org/content/kb/dns/private/new_dns/connect/choose_linux.png)

## Usar AdGuard DNS Client

AdGuard DNS Client es una utilidad de consola multiplataforma que permite utilizar protocolos DNS encriptados para acceder a AdGuard DNS.

Puedes obtener más información sobre esto en el [artículo relacionado](/dns-client/overview/).

## Usar AdGuard VPN CLI

Puedes configurar AdGuard DNS privado utilizando el AdGuard VPN CLI (interfaz de línea de comandos). Para comenzar con AdGuard VPN CLI, necesitarás usar Terminal.

1. Instala AdGuard VPN CLI siguiendo [estas instrucciones](https://adguard-vpn.com/kb/adguard-vpn-for-linux/installation/).
2. Go to [Settings](https://adguard-vpn.com/kb/adguard-vpn-for-linux/settings/).
3. Para establecer un servidor DNS específico, utiliza el comando: `adguardvpn-cli config set-dns <server_address>`, donde `<server_address>` es la dirección de tu servidor privado.
4. Activa la configuración DNS ingresando `adguardvpn-cli config set-system-dns on`.

## Configura manualmente en Ubuntu (se requiere IP vinculada o IP dedicada)

1. Click _System_ → _Settings_ → _Network_.
2. Selecciona la pestaña _Inalámbrico_, luego elige la red a la que estás conectado.
3. Go to _IPv4_.
4. Set _Automatic (DHCP)_ to _Manual_.
5. Change the listed DNS addresses to the following addresses:
    - `94.140.14.49`
    - `94.140.14.59`
6. Haz clic en _Aplicar_.
7. Ve a _IPv6_.
8. Set _Automatic_ to _Manual_.
9. Change the listed DNS addresses to the following addresses:
    - `2a10:50c0:0:0:0:0:ded:ff`
    - `2a10:50c0:0:0:0:0:dad:ff`
10. Haz clic en _Aplicar_.
11. Vincula tu dirección IP (o tu IP dedicada si tienes una suscripción a Team):
    - [Dedicated IPs](/private-dns/connect-devices/other-options/dedicated-ip.md)
    - [Linked IPs](/private-dns/connect-devices/other-options/linked-ip.md)

## Configura manualmente en Debian (se requiere IP vinculada o IP dedicada)

1. Abre el Terminal.
2. En la línea de comandos, escribe: `su`.
3. Ingresa tu contraseña `admin`.
4. En la línea de comandos, escribe: `nano /etc/resolv.conf`.
5. Cambia las direcciones DNS enumeradas por las siguientes:
    - IPv4: `94.140.14.49 y 94.140.14.59`
    - IPv6: `2a10:50c0:0:0:0:0:ded:ff y 2a10:50c0:0:0:0:0:dad:ff`
6. Presiona _Ctrl + O_ para guardar el documento.
7. Presiona _Enter_.
8. Presiona _Ctrl + X_ para guardar el documento.
9. En la línea de comandos, escribe: `/etc/init.d/networking restart`.
10. Presiona _Enter_.
11. Cierra el Terminal.
12. Vincula tu dirección IP (o tu IP dedicada si tienes una suscripción a Team):
    - [Dedicated IPs](/private-dns/connect-devices/other-options/dedicated-ip.md)
    - [Linked IPs](/private-dns/connect-devices/other-options/linked-ip.md)

## Usar dnsmasq

1. Instala dnsmasq utilizando los siguientes comandos:

    `sudo apt updatesudo`

    `apt install`

    `dnsmasqsudo nano /etc/dnsmasq.conf`

2. Usa los siguientes comandos en dnsmasq.conf:

    `no-resolv`

    `bogus-priv`

    `strict-order`

    `server=94.140.14.49`

    `server=94.140.14.59`

    `port=5353`

    `add-cpe-id={Your_Device_ID}`

3. Reinicia el servicio dnsmasq:

    `sudo service dnsmasq restart`

¡Todo listo! Tu dispositivo está conectado correctamente a AdGuard DNS.

:::note Importante

Nota: Si ves una notificación que indica que no estás conectado a AdGuard DNS, lo más probable es que el puerto en el que se está ejecutando dnsmasq está ocupado por otros servicios. Utiliza [estas instrucciones](https://github.com/AdguardTeam/AdGuardHome/wiki/FAQ#bindinuse) para resolver el problema.

:::

## Use EDNS (Extended DNS)

EDNS extends the DNS protocol, enabling larger UDP packets to carry additional data. In AdGuard DNS, it allows passing DeviceID in plain DNS using an extra parameter.

DeviceID, an eight-digit hexadecimal identifier (e.g., `1a2b3c4d`), helps link DNS requests to specific devices. For encrypted DNS, this ID is part of the domain (e.g., `1a2b3c4d.d.adguard-dns.com`). For unencrypted DNS, EDNS is required to transfer this identifier.

AdGuard DNS uses EDNS to retrieve DeviceID by looking for option number `65074`. If such an option exists, it will read DeviceID from there. For this, you can use the `dig` command in the terminal:

```sh
dig @94.140.14.49 'www.example.com' A IN +ednsopt=65074:3031323334353637
```

Here, `65074` is the option ID, and `3031323334353637` is its value in hex format (DeviceID: `01234567`).

¡Todo listo! DeviceID should be displayed.

:::note

The `dig` command is merely an example, you can use any DNS software with an ability to add EDNS options to perform this action.

:::

## Usar DNS simple

Si prefieres no usar software adicional para la configuración de DNS, puedes optar por DNS no encriptado. Tienes dos opciones: usar IPs vinculadas o IPs dedicadas:

- [Dedicated IPs](/private-dns/connect-devices/other-options/dedicated-ip.md)
- [Linked IPs](/private-dns/connect-devices/other-options/linked-ip.md)
