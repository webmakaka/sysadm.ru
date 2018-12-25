---
layout: page
title: Настройка статической адресации сетевых интерфейсов в ubuntu 18.04 в командной строке
permalink: /linux/desktops/ubuntu/network/static-ip/
---

# Настройка статической адресации сетевых интерфейсов в ubuntu 18.04 в командной строке

**Вроде, сначала нужно отключить и удалить всякие network-manager !!!**

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
