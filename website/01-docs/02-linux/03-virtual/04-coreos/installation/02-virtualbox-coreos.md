---
layout: page
title: Инсталляция CoreOS в virtualBox
permalink: /linux/virtual/coreos/installation/virtualbox-coreos/
---




<br/>

### Подготовка виртуального жесткого диска virtualbox с coreos

    $ cd /mnt/dsk0/machines

    $ mkdir templates

    $ wget $ https://raw.github.com/coreos/scripts/master/contrib/create-coreos-vdi

    $ chmod +x create-coreos-vdi

    $ ./create-coreos-vdi -V stable -d ./templates


Далее я попробовал запустить. Операционная система стартовала и попросила пароль.

Стало понятно, что пароли вроде root/root, core/core не подходят и нужно копать дальше.

<br/>

### Создаем Config-Drive


Нужно сгенерировать rsa ключ и подложить его.

    $ ssh-keygen -t rsa

На все вопросы [Enter]


    $ wget https://raw.github.com/coreos/scripts/master/contrib/create-basic-configdrive

    $ chmod +x create-basic-configdrive

    $ ./create-basic-configdrive -H my_vm01 -S ~/.ssh/id_rsa.pub
    Success! The config-drive image was created on /mnt/dsk0/my_vm01.iso


<br/>

### Запускаем виртуальную машину ViertualBox с CoreOS

Vdi диск подключил как жесткий диск. Iso как CD-ROM.

По DHCP у меня IP адреса не раздаются, а хост машина подключена к роутеру по кабелю.

Добавляю 1 сетевой адаптер типа Bridge и сообщаю, что он должен работать с локальным eh0.

Запускаю виртуальную машину.


Далее появилось приглашение ввести логин/пароль.

Остается с хостовой машины подключиться по SSH к гостевой.

**Внимание!!! Чтобы узнать по какому IP подключаться. Нужно в окне приглашения (где нужно ввести login) несколько раз нажать на [Enter]. Появится окно, в котором будет написано, к какому IP подлючаться**


    $ ssh core@192.168.1.250
    Last login: Sat Jan 16 12:12:40 2016 from 192.168.1.5
    CoreOS stable (835.9.0)


Docker уже установлен.
Мне пока больше ничего и не нужно.

    $ docker -v
    Docker version 1.8.3, build cedd534-dirty


Так. Интернет пока в CoreOS не заработал.
Локальная сеть доступна. Значит нужно просто настроить.


<br/>

### Настройка сети в CoreOS

    $ sudo su -

    # vi /etc/systemd/network/static.network

<br/>

    [Match]
    Name=enp0s8

    [Network]
    Address=192.168.1.11/24
    Gateway=192.168.1.1
    DNS=192.168.1.1


<br/>

    # systemctl restart systemd-networkd

Не помогло, пришлось рестартовать.

<br/><br/>

    $ ssh core@192.168.1.11

<br/>

    core@my_vm01 ~ $ ping ya.ru
    PING ya.ru (93.158.134.3) 56(84) bytes of data.
    64 bytes from ya.ru (93.158.134.3): icmp_seq=1 ttl=55 time=4.37 ms
    64 bytes from ya.ru (93.158.134.3): icmp_seq=2 ttl=55 time=2.39 ms


<br/><br/>

Материалы:  

[Running CoreOS on VirtualBox](https://coreos.com/os/docs/774.0.0/booting-on-virtualbox.html)

[Network Configuration with networkd](https://coreos.com/os/docs/latest/network-config-with-networkd.html)
