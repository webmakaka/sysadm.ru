---
layout: page
title: Настройка статической адресации сетевых интерфейсов в ubuntu server 16.04
permalink: /linux/basics/ubuntu/static-networking/
---


### Настройка статической адресации сетевых интерфейсов в ubuntu server 16.04


    # apt-get remove resolvconf


Пришлось сделать reboot


    # vi /etc/network/interfaces

<br/>

    auto enp2s0
    iface enp2s0 inet static
                 address 192.168.1.5
                 netmask 255.255.255.0
                 gateway 192.168.1.1



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
