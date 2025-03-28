---
title: 专用 IP 地址
sidebar_position: 2
---

## 什么是专用 IP 地址？

专用 IPv4 地址适用于团队版和企业版的用户，而关联 IP 地址适用于所有版本的用户。

如果您拥有团队版或企业版订阅，您将获得多个个人版专用 IP 地址。 发往这些地址的请求将被视为“您的”请求，并应用服务器级别的配置和过滤规则。 专用 IP 地址更加安全且易于管理。 使用关联 IP 地址时，每次设备的 IP 地址更改（每次重启后都会更改），用户都要手动重新连接或使用专用程序。

## 为什么要用专用 IP？

Unfortunately, the technical specifications of the connected device may not always allow you to set up an encrypted Private AdGuard DNS server. 在这种情况下，用户要使用标准的无加密的 DNS。 有两种方式可以设置 AdGuard DNS：[使用关联 IP 地址](/private-dns/connect-devices/other-options/linked-ip.md)和专用 IP 地址。

专用 IP 地址通常是更稳定的选择。 关联 IP 有一些限制，例如只允许住宅地址，提供商可以更改 IP，您需要重新关联 IP 地址。 通过专用 IP，用户将获得一个专属于您的 IP 地址，所有请求都将计入设备。

缺点是，用户可能会开始收到无关流量（扫描器，机器人），这在使用公共 DNS 解析器时总是会发生。 请[访问设置](/private-dns/server-and-settings/access.md)以限制机器人流量。

将专用 IP 连接到设备的指示说明如下：

## 使用专用 IP 地址连接 AdGuard DNS

1. 打开仪表盘。
2. 添加新设备或打开之前创建的设备设置。
3. 选择「_使用服务器地址_」。
4. 接下来，请打开「_无加密的 DNS 服务器地址_」。
5. 请选择想要使用的服务器。
6. 要绑定专用 IPv4 地址，请点击「_分配_」。
7. 如果要使用专用 IPv6 地址，请点击「_复制_」。
    ![复制地址 \*border](https://cdn.adtidy.org/content/kb/dns/private/new_dns/connect/dedicated_step7.png)
8. 复制并粘贴所选的专用地址到设备配置中。
