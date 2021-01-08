---
layout: page
title: Настройка сети в Centos 6, когда используется несколько адаптеров
description: Настройка сети в Centos 6, когда используется несколько адаптеров
keywords: Настройка сети в Centos 6, когда используется несколько адаптеров
permalink: /devops/linux/virtual/virtualbox/network/centos-nat-host-only-internal/
---

# Настройка сети в Centos 6, когда используется несколько адаптеров

Итак.
Я работаю сейчас за машиной с виндовс, и мне нужно, чтобы на виртуалке был доступ к интернету, я мог к ней подключиться с помощью ssh клиента и нужна внутренняя сеть, в которой виртуалки будут общаться между собой. Т.к. сижу на работе, где я не являюсь сисадмином, тип соединения bridge использовать не буду.

По интерфейсам:

    eth0 - Nat
    eth1 - Host Only
    eth2 - Internal Network

eth0 с NAT не трогаю, он динамический.

eth1 - смотрю на хосте в какой подсети работаем. В уменя уже есть виртуалки, которые работают с 192.168.56.0, поэтому мне еще выдали 192.168.99.0

    # cp /etc/sysconfig/network-scripts/ifcfg-eth1 /etc/sysconfig/network-scripts/ifcfg-eth1.orig

<br/>

    # vi /etc/sysconfig/network-scripts/ifcfg-eth1
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

<br/>

eth2

<br/>

    DEVICE=eth2
    TYPE=Ethernet
    ONBOOT=yes
    BOOTPROTO=static
    IPADDR=192.168.100.2
    NETMASK=255.255.255.0

После

    # service network restart

Пропадает доступ к интернету:

    # route -n
    Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
    10.0.2.0        0.0.0.0         255.255.255.0   U     0      0        0 eth0
    192.168.99.0    0.0.0.0         255.255.255.0   U     0      0        0 eth1
    192.168.56.0    0.0.0.0         255.255.255.0   U     0      0        0 eth1
    169.254.0.0     0.0.0.0         255.255.0.0     U     1002   0        0 eth0
    169.254.0.0     0.0.0.0         255.255.0.0     U     1003   0        0 eth1
    0.0.0.0         192.168.56.1    0.0.0.0         UG    0      0        0 eth1

56.1 здесь - интерфейс с этим адресом, который VirtualBox создает по умолчанию на хостовой машине. Именно с ним и работает виртуальная машина, обмениваясь пакетами.

    $ route delete default gw 192.168.56.1 eth1
    $ route add default gw 10.0.2.2 eth0

10.0.2.2 я посмотрел в маршрутах, если оставить только eth0 включенным. А так догадаться, почему мужно указывать 10.0.2.2, я не знаю. И где посмотреть тоже не знаю. Буду признателен, если кто скажет, но это не очень важно, т.к сеть начинает работать.

<br/>

    # ping ya.ru
    PING ya.ru (213.180.193.3) 56(84) bytes of data.
    64 bytes from www.yandex.ru (213.180.193.3): icmp_seq=1 ttl=56 time=2.49 ms
    64 bytes from www.yandex.ru (213.180.193.3): icmp_seq=2 ttl=56 time=2.84 ms
    64 bytes from www.yandex.ru (213.180.193.3): icmp_seq=3 ttl=56 time=2.87 ms
    64 bytes from www.yandex.ru (213.180.193.3): icmp_seq=4 ttl=56 time=2.80 ms

Как сохранить нужные настройки маршрутов, чтобы после перезагрузки виртуальной машины они не сбросились, я пока не разобрался. (да и не очень то и старался.)
