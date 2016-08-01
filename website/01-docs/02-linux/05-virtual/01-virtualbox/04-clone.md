---
layout: page
title: Клонирование виртуальных машин VirtualBox в командной строке
permalink: /linux/virtual/virtualbox/clone/
---

### Клонирование виртуальных машин VirtualBox в командной строке

Указываем место куда клонировать виртуальную машину. Вместо ${VM_HOME} укажите каталог, гуда следует клонировать.

    $ VBoxManage setproperty machinefolder ${VM_HOME}

<br/>

    $ vboxmanage clonevm <source-machine-name> --name <target-machine-name> --register


После клонирования centos6, пришлось подключаться к виртуальной машине по rdp и стирать правила udev в файле /etc/udev/rules.d/70.persistent-net.rules

В centos7 не пришлось менять правила udev.
