---
title: Xiaomi
sidebar_position: 11
---

Xiaomi routers have many advantages: a stable, strong signal, network security, robust performance, and smart management. Users can connect up to 64 devices to a local Wi-Fi network.

Unfortunately, it doesn't support encrypted DNS, but it's great for setting up AdGuard DNS via linked IP.

## Use your router admin panel

Use these instructions if your Keenetic router does not support DNS-over-HTTPS or DNS-over-TLS configuration:

1. Open the router admin panel. It can be accessed at `192.168.31.1` or the IP address of your router.
1. Enter the administrator username (usually, it’s admin) and router password.
1. Open *Advanced Settings* or *Advanced*, depending on your router model.
1. Open *Network* or *Internet* and look for DNS or DNS Settings.
1. Choose *Manual DNS*. Select *Use These DNS Servers* or *Specify DNS Server Manually* and enter the following DNS server addresses:
    - IPv4: `94.140.14.49` and `94.140.14.59`
    - IPv6: `2a10:50c0:0:0:0:0:ded:ff` and `2a10:50c0:0:0:0:0:dad:ff`
1. Save the settings.
1. Link your IP (or your dedicated IP if you have a Team subscription).

- [Dedicated IPs](/private-dns/connect-devices/other-options/dedicated-ip.md)
- [Linked IPs](/private-dns/connect-devices/other-options/linked-ip.md)
