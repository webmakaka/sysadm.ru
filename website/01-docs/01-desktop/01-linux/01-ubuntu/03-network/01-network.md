---
layout: page
title: Настройка статической адресации сетевых интерфейсов в ubuntu 18.04 в командной строке
description: Настройка статической адресации сетевых интерфейсов в ubuntu 18.04 в командной строке
keywords: ubuntu, network, static ip
permalink: /desktop/linux/ubuntu/network/static-ip/
---

# Настройка статической адресации сетевых интерфейсов в ubuntu 18.04 в командной строке

**Не рекомндуется делать без особой необходимости**

**Чтобы работало, сначала нужно отключить network-manager !!! Но в 99% случаев достаточно и GUI инструментов настройки**

    # apt-get remove resolvconf

Пришлось сделать reboot

    # vi /etc/network/interfaces

<br/>

```
auto enp2s0
iface enp2s0 inet static
                address 192.168.1.5
                netmask 255.255.255.0
                gateway 192.168.1.1
```

<br/>

Я пересоздавал resolv.conf, т.к. это был файл - ссылка на еще что-то.

    # rm resolv.conf
    # touch resolv.conf

<br/>

    # vi resolv.conf

<br/>

    nameserver 192.168.1.1

<br/>

    # /etc/init.d/networking restart

<br/>

### Отключить ipv6 в Ubuntu 18.04 (В тестировании)

    # vi /etc/sysctl.conf

    net.ipv6.conf.all.disable_ipv6 = 1
    net.ipv6.conf.default.disable_ipv6 = 1
    net.ipv6.conf.lo.disable_ipv6 = 1
