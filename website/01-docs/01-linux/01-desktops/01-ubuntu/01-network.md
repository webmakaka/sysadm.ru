---
layout: page
title: Настройка статической адресации сетевых интерфейсов в ubuntu 18.04
permalink: /linux/desktops/ubuntu/network/
---

# Настройка статической адресации сетевых интерфейсов в ubuntu 18.04 в командной строке

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

### Соединение точка-точка (ad-hoc) по WiFi между Ubuntu Linux и Windows XP

Нужно было настроить ad-hoc на ubuntu с usb wi-fi адаптером.

Помогла статья:

http://gearmobile.github.io/linux/wifi-adhoc-ubuntu-linux-windowsxp/
