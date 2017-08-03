---
layout: page
title: Virtual Box Основные команды
permalink: /linux/virtual/virtualbox/commands/
---

# Virtual Box Основные команды

-- Получить список виртуальных машин

    $ vboxmanage list vms

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
