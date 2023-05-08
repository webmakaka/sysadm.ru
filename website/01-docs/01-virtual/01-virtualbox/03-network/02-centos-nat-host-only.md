---
layout: page
title: Oracle Linux 7 (ака Centos 7) Создание hostonly интерфейса руками, если он по какой-то причине не создался сам
description: Oracle Linux 7 (ака Centos 7) Создание hostonly интерфейса руками, если он по какой-то причине не создался сам
keywords: Oracle Linux 7 (ака Centos 7) Создание hostonly интерфейса руками, если он по какой-то причине не создался сам
permalink: /virtual/virtualbox/network/centos-nat-host-only/
---

# Oracle Linux 7 (ака Centos 7): Создание hostonly интерфейса руками, если он по какой-то причине не создался сам

Делаю: 15.02.2018

<br/>

Нужен доступ к виртуальной машине с хоста.
Для этого поднимаю host only интерфейс.

<br/>

ifconfig на хост машине должен выводить vboxnet0.  
vboxnet0 - виртуальный адаптер хостовой машины.

    $ ifconfig vboxnet0

но тут:

    vboxnet0: error fetching interface information: Device not found

<br/>

Если виртуального адаптера нет, нуно его самостоятельно создать.

    $ VBoxManage hostonlyif create

<br/>

**Возникала ошибка:**

<br/>

[/dev/vboxnetctl No such file or directory](/virtual/virtualbox/network/centos-dev-vboxnetctl-no-such-file-or-directory/)

<br/>

**Продолжаем:**

<br/>

    $ vboxmanage list hostonlyifs
    Name:            vboxnet0
    GUID:            786f6276-656e-4074-8000-0a0027000000
    DHCP:            Disabled
    IPAddress:       192.168.56.1
    NetworkMask:     255.255.255.0
    IPV6Address:
    IPV6NetworkMaskPrefixLength: 0
    HardwareAddress: 0a:00:27:00:00:00
    MediumType:      Ethernet
    Status:          Down
    VBoxNetworkName: HostInterfaceNetworking-vboxnet0

Присвоить ip, если у него нет.

    $ VBoxManage hostonlyif ipconfig vboxnet0 --ip 192.168.56.1

Стартовать, если нужно

    $ sudo ifconfig vboxnet0 up

<br/>

    $ ifconfig vboxnet0
    vboxnet0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
            inet 192.168.56.1  netmask 255.255.255.0  broadcast 192.168.56.255
            ether 0a:00:27:00:00:00  txqueuelen 1000  (Ethernet)
            RX packets 0  bytes 0 (0.0 B)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 0  bytes 0 (0.0 B)
            TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
