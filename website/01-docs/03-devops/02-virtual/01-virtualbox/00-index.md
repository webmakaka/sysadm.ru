---
layout: page
title: VirtualBox
description: VirtualBox
keywords: linux, VirtualBox
permalink: /devops/linux/virtual/virtualbox/
---

# Virtual Box

Offtopic:

Лично я пользуюсь в последнее время контейнерами Docker. Если по какой-то причине, контейнеры docker не подходят, то виртуальные машины VirtualBox.

Но каждый раз создавать виртуальную машину, инсталлировать операционную систему или даже взять готовый вариант достаточно трудозатратно. Оказалось, что с помощью Vagrant, можно поднять виртуальную машину VirtualBox еще быстрее и менее трудозатратно. Достаточно скопировать файл описания виртуальной машины, и выполнить команду для запуска Vagrant. Подробности см. в разделе Vagrant.

<br/>

### Инсталляция

[Инсталляция VirtualBox](/devops/linux/virtual/virtualbox/setup/)

<br/>

### Команды

[Основные команды](/devops/linux/virtual/virtualbox/commands/)

<br/>

### Регистрация виртуальной машины (если нужно заново зарегистрировать)

[Регистрация виртуальной машины](/devops/linux/virtual/virtualbox/register/)

<br/>

### Создание виртуальных машин

[Создание виртуальной машины VirtualBox с Centos 6.X. в командной строке linux](/devops/linux/virtual/virtualbox/vm/centos-6/) Nat + HostOnly Network

[Создание виртуальной машины VirtualBox с Oracle Linux 6.X. в командной строке linux](/devops/linux/virtual/virtualbox/vm/oracle-linux-6/) Bridget Network

<br/>

### Основные задачи по работе с виртуальными машинами

[Работа со снапшотами](/devops/linux/virtual/virtualbox/snapshots/)

[Export и Import виртуальных машин VirtualBox](/devops/linux/virtual/virtualbox/export-import/)

[Клонирование виртуальных машин VirtualBox в командной строке](/devops/linux/virtual/virtualbox/clone/)

[Замена виртуального диска VirtualBox в командной строке](/devops/linux/virtual/virtualbox/replace-disk/)

[Уменьшить размер занимаемого виртуальной машиной дискового пространства](/devops/linux/virtual/virtualbox/decrease-disk-space/)

[Конвертировать wmdk в vdi](/devops/linux/virtual/virtualbox/convert-vmdk-vdi/)

<br/>

### Настрока сетевых интерфейсов:

[Настрока сетевых интерфейсов](/devops/linux/virtual/virtualbox/network/)

<br/>

### Подключение USB устройств:

[Подключение USB устройств](/devops/linux/virtual/virtualbox/usb/)

<br/>

### Подготовленные образы VirtualBox с Linux

<a href="http://www.osboxes.org/virtualbox-images/" rel="nofollow">osboxes</a>

<br/>

### Возникавшие ошибки при работе с VirtualBox

<a href="/devops/linux/virtual/virtualbox/errors/" rel="nofollow">osboxes</a>