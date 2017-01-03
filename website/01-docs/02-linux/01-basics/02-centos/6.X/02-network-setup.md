---
layout: page
title: Настройка сети в centos 6
permalink: /linux/basics/centos/6/network-setup/
---


# Настройка сети в centos 6

    # vi /etc/sysconfig/network-scripts/ifcfg-eth0


<br/>

### DYNAMIC

    DEVICE=eth0
    HWADDR=08:00:27:7B:72:2B
    TYPE=Ethernet
    UUID=5851397f-b1d0-4312-a9de-dadbf5e520f1
    ONBOOT=yes
    NM_CONTROLLED=yes
    BOOTPROTO=dhcp

<br/>

### STATIC

Может выглядеть следующим образом.

    DEVICE=eth1
    HWADDR=08:00:27:75:0F:01
    TYPE=Ethernet
    UUID=0b797d44-d69f-48d9-b960-f8f2bbea5d97
    ONBOOT=yes
    NM_CONTROLLED=yes
    BOOTPROTO=static
    IPADDR=192.168.99.202
    NETMASK=255.255.255.0
    GATEWAY=192.168.99.1

У меня обычно на виртуалках выглядит так:

    DEVICE="eth0"
    ONBOOT="yes"
    BOOTPROTO="static"
    IPADDR=192.168.1.11
    NETMASK=255.255.255.0
    GATEWAY=192.168.1.1


Если вы подключились к серверу по RDP, я бы рекомендовал после ввода настроек сетевого интерфейса, перестартовать службу network и подключиться к серверу по ssh. И дальнейшие команды выполнять командами copy + paste.


Перестартовать службы отвечающую за параметры сетевых интерфейсов, можно с помощью команды:

    # service network restart

Подключиться к серверу можно командой

    $ ssh root@192.168.1.11



### Продолжаем настраивать параметры сетевого окружения


Необходимо выбрать подходящее имя для сервера, которое бы отражало его роль и назначение в сети.

Для этого, с помощью редактора (например, vi) отредактируйте файл /etc/sysconfig/network


Не рекомендуется в hostname использовать знак нижнего подчеркивания (_).(Enterprise Manager и другие web приложения не смогут подключиться к базе по http/https)

    # vi /etc/sysconfig/network

<br/>

    NETWORKING=yes
    NETWORKING_IPV6=no
    HOSTNAME=oracle12serv.localdomain

<br/>

    # vi /etc/resolv.conf

    nameserver 192.168.1.1


<br/>

    # vi /etc/hosts

<br/>

    ## Localdomain and Localhost (hosts file, DNS)
    127.0.0.1 localhost.localdomain localhost

    ## IPs Public Network (hosts file, DNS)
    192.168.1.11 oracle12serv.localdomain oracle12serv

Проверяем правильно ли все работает.

    # ping google.com
