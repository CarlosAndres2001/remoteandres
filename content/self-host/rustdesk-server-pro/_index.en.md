---
title: RustDesk Server Pro
weight: 200
pre: "<b>2.2. </b>"
---

RustDesk Server Pro has more features compared to the open source version.

- No concurrent connection limit, aka no limit of simultaneous connections (OSS version has no limit either, but TeamViewer etc. have this limit)
- [Web console](https://rustdesk.com/docs/en/self-host/rustdesk-server-pro/console/)
- API
- [OIDC](https://rustdesk.com/docs/en/self-host/rustdesk-server-pro/oidc/), LDAP, [2FA](https://rustdesk.com/docs/en/self-host/rustdesk-server-pro/2fa/)
- Address book
- Rename
- Log management (Connection, file transfer, alarm, etc.)
- Device management
- [Security Settings sync](https://rustdesk.com/docs/en/self-host/rustdesk-server-pro/strategy/)
- [Access control](https://rustdesk.com/docs/en/self-host/rustdesk-server-pro/permissions/)
- [Multiple relay servers](https://rustdesk.com/docs/en/self-host/rustdesk-server-pro/relay/) (automatically selects your closest relay)

{{% notice note %}}
RustDesk Client ≥ 1.2.0 required
{{% /notice %}}
{{% notice note %}}
If you build you own server on your home/office, and can't connect it through public IP/domain, please check [this article](https://rustdesk.com/docs/en/self-host/nat-loopback-issues/).
{{% /notice %}}

### Hardware requirement

Lowest level VPS is enough for your use case. The server software is not CPU and memory intensive. Our public ID server hosted on a 2 CPU/4 GB Vultr server serves 1.5+ million endpoints.

### Download

[https://github.com/rustdesk/rustdesk-server-pro/releases/latest](https://github.com/rustdesk/rustdesk-server-pro/releases/latest)

### Installation

#### Simple Install

To make life easy, we have developed scripts which take care of everything (install/upgrade/convert from open source) [Simple Install Script](https://rustdesk.com/docs/en/self-host/rustdesk-server-pro/installscript/).

{{% notice note %}}
Don't forget to get your license from [https://rustdesk.com/pricing.html](https://rustdesk.com/pricing.html), check [license](https://rustdesk.com/docs/en/self-host/rustdesk-server-pro/license/) page for more details.
{{% /notice %}}

#### Manual Installation

Almost the same as [the open source version](https://rustdesk.com/docs/en/self-host/rustdesk-server-oss/install/), but you do not need to run hbbs/hbbr with any arguments. These can be set later in web console.

- `-k _` is set by default
- `-r <server:host>` is not needed any more if the relay server runs on the same machine with hbbs, and you can set multiple relay servers in the web console

#### Ports Required

You need port 21114-21119 TCP and 21116 UDP open, please ensure these ports are setup when you set firewall rules and Docker port mapping.

{{% notice note %}}
You should use a proxy like Nginx to deliver this over HTTPS requiring port 443 to be opened.
{{% /notice %}}
