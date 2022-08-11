---
layout: page
title: Export и Import виртуальных машин VirtualBox
description: Export и Import виртуальных машин VirtualBox
keywords: virtualbox, export, import
permalink: /adm/virtual/virtualbox/export-import/
---

# Export и Import виртуальных машин VirtualBox

### Предварительные шаги

Необходимо, чтобы переменные vm, VM_HOME и VM_BACKUPS были заданы.

Проверить их существование, можно командами:

    $ echo ${vm}
    $ echo ${VM_HOME}
    $ echo ${VM_BACKUPS}

<br/>

```
// Обычно у меня
$ export VM_HOME=$HOME/machines
$ export VM_BACKUPS=$HOME/machines/backups
```

<br/>

```
// Создаю калалог, если нужно
$ mkdir -p ${VM_BACKUPS}
```

<br/>

```
// Задаю еременныю с именем виртуальной машины:
$ vboxmanage list vms
$ export vm=<machine_name>
```

<br/>

### Подготовка к Export'у виртуальной машины

Делаю:  
11.08.2022

Может быть использован как вариант создания резервной копии или для создания копии уже работающей виртуальной машины.

Перед выполнением команды export необходимо выключить виртуальную машину или поставить её на паузу:

Лучший вариант, штатными средствами операционной системы просто выключить ее.

// Выключить

    $ VBoxManage controlvm ${vm} poweroff

// Или поставить на паузу

    $ VBoxManage controlvm ${vm} pause

// Потом можно будет снять с паузы

    $ VBoxManage controlvm ${vm} resume

<br/>

### Команды Export'a виртуальной машины

// Создать каталог для backup

    $ mkdir -p ${VM_BACKUPS}/${vm}

// Экспортировать виртуальную машину

    $ VBoxManage export ${vm} -o ${VM_BACKUPS}/${vm}/${vm}.ovf

<br/>

### Подготовка к Import виртуальной машины

Делаю:  
25.04.2021

<br/>

Задаем переменную с именем импортируемой виртуальной машины.

    $ export vm=vm_centos_jboss_postgresql

Создаем каталоги для виртуальной машины и для snapshots

    $ mkdir -p ${VM_HOME}/${vm}/snapshots

Определяем каталог, куда следует выполнить импорт

    $ VBoxManage setproperty machinefolder ${VM_HOME}/${vm}

Посмотреть переменные

    $ vboxmanage list systemproperties | grep folder

<br/>

### Import виртуальной машины

Переходим в каталог с бекапами виртуальных машинам

    $ cd ${VM_BACKUPS}

    $ cd <machine>

    $ VBoxManage import ./vm_centos_jboss_postgresql.ovf

<br/>

```
***
    0%...10%...20%...30%...40%...50%...60%...70%...80%...90%...100%
Successfully imported the appliance.
```

<br/>

Посмотреть список виртуальных машин

    $ vboxmanage list vms

Наверное, следует переименовать импортированную виртуальную машину

    $ VBoxManage modifyvm vm_centos_jboss_postgresql_1 --name ${vm}

Определяем каталог для снапшотов

    $ VBoxManage modifyvm ${vm} --snapshotfolder ${VM_HOME}/${vm}/snapshots

Посмотреть еще раз список виртуальных машин в системе и убедиться, что все ОК:

```
$ vboxmanage list vms
```

Запустить импортированную виртуальную машину

```
$ vboxmanage startvm ${vm} -type headless &
```

<br/>

### Ошибока при запуске импортированной машины

```
$ Waiting for VM "my_vm_name" to power on...
VBoxManage: error: Nonexistent host networking interface, name 'enp4s0' (VERR_INTERNAL_ERROR)
VBoxManage: error: Details: code NS_ERROR_FAILURE (0x80004005), component ConsoleWrap, interface IConsole
```

Разные хосты. Разные имена сетевых интерфейсов. Если бы импорт был бы на той же самой машине, все было бы ок. А так придется поправить конфиг.

<br/>

```
$ cd ${VM_HOME}/${vm}
$ cd ${vm}
$ cp ${vm}.vbox ${vm}.vbox.original
```

<br/>

```
$ ifconfig
```

<br/>

Есть у меня enp7s1. Его и прописываю.

<br/>

```
$ vi ${vm}.vbox
```

<br/>

### Мдя, еще 1 ошибка - NS_ERROR_FAILURE (0x80004005), component SessionMachine, interface ISession

```
$ Waiting for VM "my_vm_name" to power on...
VBoxManage: error: The VM session was aborted
VBoxManage: error: Details: code NS_ERROR_FAILURE (0x80004005), component SessionMachine, interface ISession
```

<br/>

Потыкал в GUI. (Скопировал на хост и запустил).

Угадал. Ткнул пальцем и угадал. Заменив контроллер LsiLogicSas на IntelAHCI запустилось.

<br/>

$ VBoxManage showvminfo ${vm}

```
***
Storage Controller Name (0):            SAS Controller
Storage Controller Type (0):            LsiLogicSas
***
```

<br/>

```
// Удаляю контроллер
$ VBoxManage storagectl ${vm} \
    --name "SAS Controller" \
    --remove
```

<br/>

```
// Добавляю контроллер
$ VBoxManage storagectl ${vm} \
    --name "SATA Controller" \
    --add sata \
    --controller IntelAhci
```

<br/>

```
// Подключаю диск
$ VBoxManage storageattach ${vm} \
--storagectl "SATA Controller" \
--port 0 \
--type hdd \
--medium ${vm}-disk001.vmdk
```

<br/>

### Дополнительно по настройке сети после импорта

Иногда после импорта отсутствуют сетевые адаптеры в системе.
По крайней мере в Centos 6.

Нужно как-то зайти в виртуалку. и отредактировать файл /etc/udev/rules.d/70-persistent-net.rules

Достаточно удалить или правильно настроить соответствие между устройствами и том, какие имена им будут присвоены в системе.

После следует перезагрузить виртуальную машину. (Или попробовать применить правила udev без перезагрузки).

<br/>

**Всетаки, как подключиться**

Установить <a href="/adm/virtual/virtualbox/setup/ubuntu/">Extension Packs</a> (Там присутствуют какие-то лицензионные огранчения).

<br/>

```
$ VBoxManage controlvm ${vm} poweroff
```

<br/>

```
$ VBoxManage modifyvm ${vm} \
    --vrde on \
    --vrdemulticon on \
    --vrdeauthtype null \
    --vrdeaddress 192.168.1.101 \
    --vrdeport 3389
```

<br/>

192.168.1.101 - хост на котором запущен virtualbox

<br/>

```
$ vboxmanage startvm ${vm} -type headless &
```

<br/>

    $ sudo apt-get install -y rdesktop

<br/>

    $ rdesktop \
    -r sound:local \
    -k common  \
    -g  1600x1024 \
    10.20.65.225:3389

<br/>

Login

    $ su - root
    # rm /etc/udev/rules.d/70-persistent-net.rules
    # reboot
