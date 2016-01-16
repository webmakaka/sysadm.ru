---
layout: page
title: Инсталляция CoreOS в virtualBox
permalink: /linux/virtual/coreos/installation/virtualbox-coreos/
---


[Running CoreOS on VirtualBox](https://coreos.com/os/docs/774.0.0/booting-on-virtualbox.html)

<br/>

### Подготовка виртуального жесткого диска virtualbox с coreos

    $ cd /mnt/dsk0/machines

    $ mkdir templates

    $ wget $ https://raw.github.com/coreos/scripts/master/contrib/create-coreos-vdi

    $ chmod +x create-coreos-vdi

    $ ./create-coreos-vdi -V stable -d ./templates


Далее я попробовал запустить. Операционная система стартовала и попросила пароль. Стало понятно, что пароли вроде root/root, core/core не подходят и нужно копать дальше.

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

Вообщем. Я vdi диск подключил как жесткий диск. Iso как CD-ROM.

По DHCP у меня IP адреса не раздаются, а хост машина подключена к роутеру по кабелю. Пока не знаю, имеет ли это какое-то значение или нет. Наверное нет, т.к. сам virtualBox для виртуальной машины будет выступать в качестве DHCP сервера (я так думаю).  

Вообщем добавляю еще 1 сетевой адаптер типа Bridge и коннекчу его к своему локальному eh0.

Запускаю виртуальную машину.


Далее появилось приглашение ввести логин/пароль.

Остается с хостовой машины подключиться по SSH к гостевой.

**Внимание!!! Чтобы узнать по какому IP подключаться. Нужно в окне приглашения, где нужно ввести login, нужно несколько раз нажать на [Enter]. Появится окно, в котором будет написано к какому IP подлючаться**


    $ ssh core@192.168.1.250
    Last login: Sat Jan 16 12:12:40 2016 from 192.168.1.5
    CoreOS stable (835.9.0)


Docker уже работает.
Мне пока больше ничего и не нужно.

    $ docker ps
    CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES


Так. Интернет пока в CoreOS не заработал.  
Будем посмотреть.
