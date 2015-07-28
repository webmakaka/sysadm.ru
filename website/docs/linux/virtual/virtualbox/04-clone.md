---
layout: page
title: Клонирование виртуальных машин VirtualBox в командной строке
permalink: /linux/virtual/virtualbox/clone/
---



    vboxmanage clonevm <source-machine-name> --name <target-machine-name> --register


После клонирования, пришлось подключаться к виртуальной машине по rdp и стирать правила udev в файле /etc/udev/rules.d/70.persistent-net.rules
