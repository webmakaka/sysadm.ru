---
layout: page
title: Настройка статической адресации сетевых интерфейсов в ubuntu 18.04 в командной строке
permalink: /linux/desktops/ubuntu/network/static-ip/
---

# Настройка статической адресации сетевых интерфейсов в ubuntu 18.04 в командной строке

**Чтобы работало, сначала нужно отключить network-manager !!! Но я теперь ленюсь и настраиваю конфиги в формочках**

<br/>

    # vi /etc/network/interfaces

<br/>

    auto enp2s0
    iface enp2s0 inet static
                 address 192.168.1.5
                 netmask 255.255.255.0
                 gateway 192.168.1.1

<br/>

    # /etc/init.d/networking restart


<br/>

### Отключить ipv6 в Ubuntu 18.04 (В тестировании)

    # vi /etc/sysctl.conf

    net.ipv6.conf.all.disable_ipv6 = 1
    net.ipv6.conf.default.disable_ipv6 = 1
    net.ipv6.conf.lo.disable_ipv6 = 1
