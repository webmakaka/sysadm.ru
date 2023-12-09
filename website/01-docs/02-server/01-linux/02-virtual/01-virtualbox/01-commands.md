---
layout: page
title: VirtualBox Основные команды
description: VirtualBox Основные команды
keywords: VirtualBox Основные команды
permalink: /server/linux/virtual/virtualbox/commands/
---

# VirtualBox Основные команды

<br/>

-- Получить список всех виртуальных машин

    $ vboxmanage list vms

-- Получить список стартованных виртуальных машин

    $ vboxmanage list runningvms

-- Удалить виртуальную машину

    $ vboxmanage unregistervm vm_oel7.3_oracle_db_12.2 --delete

<br/>

### Могут понадобиться следующие команды:

Извлечь DVD диск можно командой

    $ VBoxManage modifyvm ${vm} --dvd none

Reset виртуальной машины

    $ VBoxManage controlvm ${vm} reset

Выключить виртуальную машину

    $ VBoxManage controlvm ${vm} poweroff

<br/>

### Получить помощь по командам:

    $ vboxmanage help

<br/>

    $ VBoxManage list ostypes
