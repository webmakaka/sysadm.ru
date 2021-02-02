---
layout: page
title: Export и Import виртуальных машин VirtualBox
description: Export и Import виртуальных машин VirtualBox
keywords: Export и Import виртуальных машин VirtualBox
permalink: /devops/linux/virtual/virtualbox/export-import/
---

# Export и Import виртуальных машин VirtualBox

### Предварительные шаги

Необходимо, чтобы переменные vm, VM_HOME и VM_BACKUPS были заданы.

Проверить их существование, можно командами:

    $ echo ${vm}
    $ echo ${VM_HOME}
    $ echo ${VM_BACKUPS}

При необходимости, нужно задать переменные:

    $ vboxmanage list vms
    $ export vm=<machine_name>

<br/>

    $ export VM_BACKUPS=${VM_HOME}/backup

    $ echo ${VM_BACKUPS}
    /mnt/dsk1/machines/backups

<br/>

### Подготовка к Export'у виртуальной машины

Делаю:  
18.08.2019

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

### Подготовка к Import виртуальной машины

Задаем переменную с именем импортируемой виртуальной машины.

    $ vm=vm_centos_jboss_postgresql

Создаем каталоги для виртуальной машины и для snapshots

    $ mkdir -p ${VM_HOME}/${vm}/snapshots

Определяем каталог, куда следует выполнить импорт

    $ VBoxManage setproperty machinefolder ${VM_HOME}/${vm}

Посмотреть переменные

    $ vboxmanage list systemproperties | grep folder

### Import виртуальной машины

Переходим в каталог с бекапами виртуальных машинам

    $ cd ${VM_BACKUPS}

    $ cd <machine>

    $ VBoxManage import ./vm_oel57_oradb112.ovf

    0%...10%...20%...30%...40%...50%...60%...70%...80%...90%...100%

Successfully imported the appliance.

Посмотреть список виртуальных машин

    $ vboxmanage list vms

Наверное, следует переименовать импортированную виртуальную машину

    $ VBoxManage modifyvm vm_centos_jboss_postgresql_1 --name ${vm}

Определяем каталог для снапшотов

    $ VBoxManage modifyvm ${vm} --snapshotfolder ${VM_HOME}/${vm}/snapshots

Посмотреть еще раз список виртуальных машин в системе и убедиться, что все ОК:

    $ vboxmanage list vms

Запустить импортированную виртуальную машину

    $ vboxmanage startvm ${vm} -type headless &

<br/>

### Дополнительно по настройке сети после импорта

Иногда после импорта отсутствуют сетевые адаптеры в системе.

Нужно отредактировать файл /etc/udev/rules.d/70-persistent-net.rules

Достаточно удалить или правильно настроить соответствие между устройствами и том, какие имена им будут присвоены в системе.

После следует перезагрузить виртуальную машину. (Или попробовать применить правила udev без перезагрузки).
