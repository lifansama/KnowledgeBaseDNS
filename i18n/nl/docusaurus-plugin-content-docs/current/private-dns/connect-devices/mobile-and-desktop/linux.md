---
title: Linux
sidebar_position: 6
---

To connect a Linux device to AdGuard DNS, first add it to _Dashboard_:

1. Go to _Dashboard_ and click _Connect new device_.
2. In the drop-down menu _Device type_, select Linux.
3. Name the device.
    ![Connecting device \*mobile_border](https://cdn.adtidy.org/content/kb/dns/private/new_dns/connect/choose_linux.png)

## Use AdGuard DNS Client

AdGuard DNS Client is a cross-platform console utility that allows you to use encrypted DNS protocols to access AdGuard DNS.

You can learn more about this in the [related article](/dns-client/overview/).

## Use AdGuard VPN CLI

You can set up Private AdGuard DNS using the AdGuard VPN CLI (command-line interface). To get started with AdGuard VPN CLI, you’ll need to use Terminal.

1. Install AdGuard VPN CLI by following [these instructions](https://adguard-vpn.com/kb/adguard-vpn-for-linux/installation/).
2. Ga naar [Instellingen](https://adguard-vpn.com/kb/adguard-vpn-for-linux/settings/).
3. To set a specific DNS server, use the command: `adguardvpn-cli config set-dns <server_address>`, where `<server_address>` is your private server’s address.
4. Activate the DNS settings by entering `adguardvpn-cli config set-system-dns on`.

## Configure manually on Ubuntu (linked IP or dedicated IP required)

1. Click _System_ → _Settings_ → _Network_.
2. Select the _Wireless_ tab, then choose the network you’re connected to.
3. Go to _IPv4_.
4. Stel _Automatisch (DHCP)_ in op _Handmatig_.
5. Change the listed DNS addresses to the following addresses:
    - `94.140.14.49`
    - `94.140.14.59`
6. Click _Apply_.
7. Go to _IPv6_.
8. Stel _Automatisch_ in op _Handmatig_.
9. Change the listed DNS addresses to the following addresses:
    - `2a10:50c0:0:0:0:0:ded:ff`
    - `2a10:50c0:0:0:0:0:dad:ff`
10. Click _Apply_.
11. Link your IP address (or your dedicated IP if you have a Team subscription):
    - [Toegewezen IP's](/private-dns/connect-devices/other-options/dedicated-ip.md)
    - [Gekoppelde IP's](/private-dns/connect-devices/other-options/linked-ip.md)

## Configure manually on Debian (linked IP or dedicated IP required)

1. Open the Terminal.
2. In the command line, type: `su`.
3. Enter your `admin` password.
4. In the command line, type: `nano /etc/resolv.conf`.
5. Change the listed DNS addresses to the following:
    - IPv4: `94.140.14.49 and 94.140.14.59`
    - IPv6: `2a10:50c0:0:0:0:0:ded:ff and 2a10:50c0:0:0:0:0:dad:ff`
6. Press _Ctrl + O_ to save the document.
7. Press _Enter_.
8. Press _Ctrl + X_ to save the document.
9. In the command line, type: `/etc/init.d/networking restart`.
10. Press _Enter_.
11. Close the Terminal.
12. Link your IP address (or your dedicated IP if you have a Team subscription):
    - [Toegewezen IP's](/private-dns/connect-devices/other-options/dedicated-ip.md)
    - [Gekoppelde IP's](/private-dns/connect-devices/other-options/linked-ip.md)

## Use dnsmasq

1. Install dnsmasq using the following commands:

    `sudo apt updatesudo`

    `apt install`

    `dnsmasqsudo nano /etc/dnsmasq.conf`

2. Use the following commands in dnsmasq.conf:

    `no-resolv`

    `bogus-priv`

    `strict-order`

    `server=94.140.14.49`

    `server=94.140.14.59`

    `port=5353`

    `add-cpe-id={Your_Device_ID}`

3. Restart the dnsmasq service:

    `sudo service dnsmasq restart`

All done! Your device is successfully connected to AdGuard DNS.

:::note Important

If you see a notification that you are not connected to AdGuard DNS, most likely the port on which dnsmasq is running is occupied by other services. Use [these instructions](https://github.com/AdguardTeam/AdGuardHome/wiki/FAQ#bindinuse) to solve the problem.

:::

## EDNS (Extended DNS) gebruiken

EDNS breidt het DNS-protocol uit, waarbij grotere UDP-pakketten extra gegevens kunnen bevatten. In AdGuard DNS staat het toe om DeviceID door te geven in standaard DNS met behulp van een extra parameter.

DeviceID, een achtcijferige hexadecimale identificator (bijv. `1a2b3c4d`), helpt bij het koppelen van DNS-verzoeken aan specifieke apparaten. Voor versleutelde DNS is deze ID onderdeel van het domein (bijv., `1a2b3c4d.d.adguard-dns.com`). Voor onbeveiligde DNS is EDNS vereist om deze identificator over te dragen.

AdGuard DNS gebruikt EDNS om DeviceID op te halen door te zoeken naar optie nummer `65074`. Als er een dergelijke optie bestaat, wordt de DeviceID daaruit gelezen. For this, you can use the `dig` command in the terminal:

```sh
dig @94.140.14.49 'www.example.com' A IN +ednsopt=65074:3031323334353637
```

Hier is `65074` de optie-ID en `3031323334353637` is de waarde in hex-formaat (DeviceID: `01234567`).

Voltooid! DeviceID moet worden weergegeven.

:::note

De `dig`-opdracht is slechts een voorbeeld, je kunt elke DNS-software gebruiken met de mogelijkheid om EDNS-opties toe te voegen om deze actie uit te voeren.

:::

## Use plain DNS

If you prefer not to use extra software for DNS configuration, you can opt for unencrypted DNS. You have two choices: using linked IPs or dedicated IPs:

- [Toegewezen IP's](/private-dns/connect-devices/other-options/dedicated-ip.md)
- [Gekoppelde IP's](/private-dns/connect-devices/other-options/linked-ip.md)
