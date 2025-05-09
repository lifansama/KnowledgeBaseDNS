---
title: Universal instructions
sidebar_position: 2
---

Here are some general instructions for setting up Private AdGuard DNS on routers. You can refer to this guide if you can't find your specific router in the main list. Please note that the configuration details provided here are approximate and may differ from the settings on your particular model.

## Use your router admin panel

1. Open the preferences for your router. Usually you can access them from your browser. Depending on the model of your router, try entering one the following addresses:
    - Linksys and Asus routers typically use: [http://192.168.1.1](http://192.168.1.1/)
    - Netgear routers typically use: [http://192.168.0.1](http://192.168.0.1/) or [http://192.168.1.1](http://192.168.1.1/) D-Link routers typically use [http://192.168.0.1](http://192.168.0.1/)
    - Ubiquiti routers typically use: [http://unifi.ubnt.com](http://unifi.ubnt.com/)

2. Enter the router's password.

    :::note Important

    If the password is unknown, you can often reset it by pressing a button on the router; it will also reset the router to its factory settings. Some models have a dedicated management application, which should already be installed on your computer.

    :::

3. Find where DNS settings are located in the router's admin console. Change the listed DNS addresses to the following addresses:
    - IPv4: `94.140.14.49` and `94.140.14.59`
    - IPv6: `2a10:50c0:0:0:0:0:ded:ff` and `2a10:50c0:0:0:0:0:dad:ff`

4. Save the settings.

5. Link your IP (or your dedicated IP if you have a Team subscription).

- [Dedicated IPs](/private-dns/connect-devices/other-options/dedicated-ip.md)
- [Linked IPs](/private-dns/connect-devices/other-options/linked-ip.md)
