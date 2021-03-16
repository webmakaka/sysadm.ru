---
layout: page
title: VBoxManage error Nonexistent host networking interface name eth0 (VERR_INTERNAL_ERROR)
description: VBoxManage error Nonexistent host networking interface name eth0 (VERR_INTERNAL_ERROR)
keywords: VBoxManage error Nonexistent host networking interface name eth0 (VERR_INTERNAL_ERROR)
permalink: /adm/virtual/virtualbox/network/nonexistent-host-networking-interface/
---

# VBoxManage: error: Nonexistent host networking interface, name 'eth0' (VERR_INTERNAL_ERROR)

Делаю:  
11.08.2019

Делаю импорт виртуальной машины с одного хоста на другой. Был centos 6.x стала ubuntu 18.04

<br/>

    $ ifconfig
    enp4s0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
    ***

<br/>

### Показать конфиг виртаульной машины:

    $ VBoxManage showvminfo ${vm}

<br/>

    NIC 1:                       MAC: 0800270F6E88, Attachment: Bridged Interface 'eth0', Cable connected: on, Trace: off (file: none), Type: 82540EM, Reported speed: 0 Mbps, Boot priority: 0, Promisc Policy: deny, Bandwidth group: none

<br/>

Удаляю виртуальный интерфейс командой:

    $ VBoxManage modifyvm ${vm} --nic1 none

<br/>

(Мой компьютер подключен к маршрутизатору (обычный домашний роутер). Обмен данных между моим компьютером и виртуальной машиной будет проходить через него.

Наберите команду:

    $ VBoxManage list bridgedifs

Обратите внимание на значение:

    Name:            enp4s0

Я использую enp4s0 как основной физический интерфейс, который будут использовать виртуальные машины в качестве моста.

    $ VBoxManage modifyvm ${vm} \
    --nictype1 82540EM \
    --nic1 bridged \
    --bridgeadapter1 enp4s0
