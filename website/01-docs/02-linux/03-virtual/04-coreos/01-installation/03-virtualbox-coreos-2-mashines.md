---
layout: page
title: Инсталляция CoreOS на 2х виртуальных машинах virtualBox
permalink: /linux/virtual/coreos/installation/virtualbox-coreos-2-mashines/
---


<br/>

### Подготовка виртуального жесткого диска virtualbox с coreos

Создаю каталог, где будет все это добро хранится.

    $ mkdir -p /mnt/dsk0/machines/coreos/

    $ cd /mnt/dsk0/machines/coreos/

Следующий скрипт поможет нам скачать последнюю стабильную версию coreos

    $ wget $ https://raw.github.com/coreos/scripts/master/contrib/create-coreos-vdi

    $ chmod +x create-coreos-vdi

    $ ./create-coreos-vdi -V stable -d .

Лучше сразу расширить место на диске, чтобы можно было побольше всяких имиджей накачать. По умолчанию диск на 698M.

    $ VBoxManage modifyhd coreos_production_835.11.0.vdi --resize 20480

    $ mv coreos_production_835.11.0.vdi coreos1.vdi

    $ VBoxManage clonehd coreos1.vdi coreos2.vdi



<br/>

### Создаем Config-Drive

Для начала, нужно сгенерировать rsa ключ на хосте (если он не был создан ранее).

    $ ssh-keygen -t rsa

На все вопросы [Enter]

    $ wget https://raw.github.com/coreos/scripts/master/contrib/create-basic-configdrive

    $ mv create-basic-configdrive coreos1-config-drive

https://discovery.etcd.io/new?size=2

Генерирую ключ

Получилось
https://discovery.etcd.io/ff227745e30d8020b638d1032a91e1fa

При обращении к этому адресу


    {"action":"get","node":{"key":"/_etcd/registry/ff227745e30d8020b638d1032a91e1fa","dir":true,"modifiedIndex":981887305,"createdIndex":981887305}}


<br/>

    $ vi coreos1-config-drive

<br/>


После:

    - name: fleet.service
        command: start

Добавляю:

    - name: 00-eth0.network
      runtime: true
      content: |
        [Match]
        Name=enp0s3

        [Network]
        DNS=192.168.1.1
        Address=192.168.1.11/24
        Gateway=192.168.1.1


<br/>

    $ chmod +x coreos1-config-drive
    $ cp coreos1-config-drive coreos2-config-drive


<br/>


    $ vi coreos2-config-drive

    Address=192.168.1.12/24

<br/>

    $ ./coreos1-config-drive -H coreos1 -S ~/.ssh/id_rsa.pub -d https://discovery.etcd.io/ff227745e30d8020b638d1032a91e1fa
    $ ./coreos2-config-drive -H coreos2 -S ~/.ssh/id_rsa.pub -d https://discovery.etcd.io/ff227745e30d8020b638d1032a91e1fa


<br/>

### Запускаю виртуальные машины VirtualBox с CoreOS

Vdi диск подключаю как жесткий диск. ISO как CD-ROM.

Добавляю 1 сетевой адаптер типа Bridge и сообщаю, что он должен работать с локальным eh0.

Запускаю виртуальные машины.


    $ ssh core@192.168.1.11
    $ ssh core@192.168.1.12


При обращении по адресу:

https://discovery.etcd.io/ff227745e30d8020b638d1032a91e1fa


получаю


    {"action":"get","node":{"key":"/_etcd/registry/ff227745e30d8020b638d1032a91e1fa","dir":true,"nodes":[{"key":"/_etcd/registry/ff227745e30d8020b638d1032a91e1fa/69c92e7f1ea3c1b3","value":"coreos1=http://:2380","modifiedIndex":981895808,"createdIndex":981895808}],"modifiedIndex":981887305,"createdIndex":981887305}}


Ожидал, что будет представлено 2 узла. Но чего-то пока только один.

Будем копать.
