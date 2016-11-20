---
layout: page
title: Регистрация виртуальной машины VirtualBox
permalink: /linux/virtual/virtualbox/register/
---


### Регистрация виртуальной машины VirtualBox

В общем, если остались файлы виртуальной машины и их нужно заново зарегистрировать на какой-нибудь другой или после переустановки virtubalbox, операционной системы и т.д., можно это сделать следующей командой.


    $ vboxmanage registervm /mnt/dsk1/machines/vm_organizer/2k3Web.vbox


Я предпочитаю делать export/import, но и так иногда приходится поступать.
