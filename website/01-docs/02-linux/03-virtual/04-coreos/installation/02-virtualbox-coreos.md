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
