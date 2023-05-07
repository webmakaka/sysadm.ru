---
layout: page
title: Export и Import виртуальных машин VirtualBox
description: Export и Import виртуальных машин VirtualBox
keywords: virtualbox, export, import
permalink: /adm/virtual/virtualbox/export-import/
---

# Export и Import виртуальных машин VirtualBox

<br/>

### Подготовка к Export'у виртуальной машины

Делаю:
12.03.2023

Может быть использован как вариант создания резервной копии или для создания копии уже работающей виртуальной машины.

<br/>

```
// Задаю переменную с именем виртуальной машины:
$ vboxmanage list vms
"Notes" {2e04338d-4bd9-415c-967c-65364490bcb4}
```

<br/>

```
// Выключить виртуальную машину, при необходимости
$ VBoxManage controlvm ${VM} poweroff
```

<br/>

```
$ export VM=Notes
$ export VM_HOME=${HOME}/machines
$ export VM_BACKUPS=${VM_HOME}/backups
```

```
$ echo ${VM}
$ echo ${VM_HOME}
$ echo ${VM_BACKUPS}
```

<br/>

### Команды Export'a виртуальной машины

```
// Создать каталог для backup
$ mkdir -p ${VM_BACKUPS}/${VM}
```

<br/>

```
// Экспортировать виртуальную машину
$ VBoxManage export ${VM} -o ${VM_BACKUPS}/${VM}/${VM}.ovf
```

<br/>

### Подготовка к Import виртуальной машины

Делаю:
05.01.2023

<br/>

```
$ vboxmanage --version
7.0.4r154605
```

<br/>

Задаем переменную с именем импортируемой виртуальной машины.

```
$ export vm=notes
```

Создаем каталоги для виртуальной машины и для snapshots

<br/>

```
$ mkdir -p ${VM_HOME}/${vm}/snapshots
```

Определяем каталог, куда следует выполнить импорт

<br/>

```
$ VBoxManage setproperty machinefolder ${VM_HOME}/${vm}
```

Посмотреть переменные

```
$ vboxmanage list systemproperties | grep folder
```

<br/>

### Import виртуальной машины

Переходим в каталог с бекапами виртуальных машинам

```
$ cd ${VM_BACKUPS}

$ cd <machine>

$ VBoxManage import ./Notes.ovf
```

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

<br/>

```
$ vboxmanage list vms
```

<br/>

### Запустить импортированную виртуальную машину

Нужно проверить, что создана виртуальная сеть. В моем случае.

File -> Network Manager -> Host-only Networks -> Create -> ip 192.168.56.1 -> DHCP Server (Disabled)

<br/>

```
$ vm=Notes
$ vboxmanage startvm ${vm} -type headless &
```

<br/>

```
$ ifconfig
```

```
vboxnet0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.56.1  netmask 255.255.255.0  broadcast 192.168.56.255
        inet6 fe80::800:27ff:fe00:0  prefixlen 64  scopeid 0x20<link>
        ether 0a:00:27:00:00:00  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 47  bytes 6931 (6.9 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

<br/>

### Дополнительно по настройке сети после импорта

Иногда после импорта отсутствуют сетевые адаптеры в системе.
По крайней мере в Centos 6.

Нужно как-то зайти в виртуалку и отредактировать файл /etc/udev/rules.d/70-persistent-net.rules

Достаточно удалить (рекомендую скопировать конфиг) или правильно настроить соответствие между устройствами и том, какие имена им будут присвоены в системе.

После следует перезагрузить виртуальную машину. (Или попробовать применить правила udev без перезагрузки).

<br/>

Если host-only.
Создать глобальный host-only адаптер, работающий на ip 192.168.56.1.

Моя конфигурация.

<br/>

```
# vi /etc/sysconfig/network-scripts/ifcfg-eth0
```

<br/>

```
DEVICE="eth0"
BOOTPROTO="static"
ONBOOT="yes"
IPADDR=192.168.56.11
NETMASK=255.255.255.0
GATEWAY=192.168.56.1
```

<br/>

```
# service network restart
```

<br/>

**Как подключиться без консоли VirtubalBox**

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

<br/>

### Ошибока при импорте виртуальной машины

<br/>

```
Progress state: NS_ERROR_INVALID_ARG
VBoxManage: error: Appliance import failed
VBoxManage: error: Code NS_ERROR_INVALID_ARG (0x80070057) - Invalid argument value (extended info not available)
VBoxManage: error: Context: "RTEXITCODE handleImportAppliance(HandlerArg*)" at line 1379 of file VBoxManageAppliance.cpp
```

<br/>

ХЗ что делать. В UI пепесоздавал. Был экспорт из 6.1 в последнюю 6.1.

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
