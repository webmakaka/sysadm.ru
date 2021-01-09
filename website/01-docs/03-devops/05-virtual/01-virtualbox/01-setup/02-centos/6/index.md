---
layout: page
title: Инсталляция VirtualBox в командной строке в RedHat/Centos
description: Инсталляция VirtualBox в командной строке в RedHat/Centos
keywords: Инсталляция VirtualBox в командной строке в RedHat/Centos
permalink: /devops/linux/virtual/virtualbox/setup/centos/6/
---

# Инсталляция VirtualBox в командной строке в RedHat/Centos

    # yum repolist
    # yum update -y

<br/>

    # yum install -y \
    wget  \
    make \
    gcc \
    kernel-devel \
    perl

<br/>

    # wget -P /etc/yum.repos.d http://download.virtualbox.org/virtualbox/rpm/rhel/virtualbox.repo

<br/>

    # yum list VirtualBox*

<br/>

    # wget -q http://download.virtualbox.org/virtualbox/debian/oracle_vbox.asc -O- | rpm --import -

<br/>

    # yum install -y VirtualBox

<br/>

### Инсталляция Extension Pack

    # cd /tmp/
    # wget http://download.virtualbox.org/virtualbox/4.3.4/Oracle_VM_VirtualBox_Extension_Pack-4.3.4.vbox-extpack

<br/>

    # VBoxManage extpack install

    Oracle_VM_VirtualBox_Extension_Pack-4.3.4.vbox-extpack
    0%...10%...20%...30%...40%...50%...60%...70%...80%...90%...100%
    Successfully installed "Oracle VM VirtualBox Extension Pack".

<br/>

    # VBoxManage list extpacks

    Extension Packs: 1
    Pack no. 0:   Oracle VM VirtualBox Extension Pack
    Version:      4.3.4
    Revision:     91027
    Edition:
    Description:  USB 2.0 Host Controller, Host Webcam, VirtualBox RDP, PXE ROM with E1000 support.
    VRDE Module:  VBoxVRDP
    Usable:       true
    Why unusable:

<br/>

    # useradd -m vmadm

<br/>

    # passwd vmadm

<br/>

    # usermod -G vboxusers vmadm

<br/>

### Запрет входа root по SSH

    # cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bkp

<br/>

    # vi /etc/ssh/sshd_config

<br/>

    #PermitRootLogin yes

меняем на

    PermitRootLogin no

<br/>

    # service sshd restart

<br/>

### Настройка параметров учетной записи для работы с виртуальными машинами

    # su - vmadm

<br/>

    $  vi ~/.bash_profile

<br/>

Добавляю:

```
# User specific environment and startup programs
############################################
#### VirtualBox Parameters

            export VM_HOME=$HOME/machines
            export VM_BACKUPS=$HOME/backups
            export VM_ISO=$HOME/iso

############################################
```

Применить новые параметры:

    $ source ~/.bash_profile

<br/>

    $ mkdir -p ${VM_HOME}
    $ mkdir -p ${VM_BACKUPS}
    $ mkdir -p ${VM_ISO}

Далее создаем виртуальную машину

<br/>

# Инсталляция Guest Additions в командной строке в Centos x64

Пакет Guest Additions как минимум нужен для того, чтобы мышка по экрану нормально перемещалась, работала copy+paste и может быть что-то еще. Нужно ли устанавливать guest additions, если предстоит работать только в командной строке, наверное нет.

Installation guide

http://www.virtualbox.org/manual/ch04.html#idp11277648

<br/>

### Centos x64

    # wget http://pkgs.repoforge.org/rpmforge-release/rpmforge-release-0.5.3-1.el6.rf.x86_64.rpm

    # rpm -ivh rpmforge-release-0.5.3-1.el6.rf.x86_64.rpm

    # yum install -y p7zip

    # 7za x ./VBoxGuestAdditions_5.0.10.iso -o./VBoxGuestAdditions_5.0.10/

    # cd VBoxGuestAdditions_5.2.0/

    # chmod +x ./VBoxLinuxAdditions.run

    # ./VBoxLinuxAdditions.run

    # reboot

На клиенте, после инсталляции guest additions можно выполнить команды:

    $ VBoxClient

    Options:

      --clipboard            start the shared clipboard service
      --draganddrop          start the drag and drop service
      --display              start the display management service
      --checkhostversion start the host version notifier service
      --seamless             start the seamless windows service
      -d, --nodaemon         continue running as a system service

Буфер обмена постоянно перестает работать.

К сожалению, мне пока не удалось найти решения, которое позволило бы полностью побороть данную проблему.

Как вариант,

// Найти процесс clipboard

    $ ps -Af | grep VBoxClient

// кильнуть его по -9

    $ kill -9

// Стартовать его заново

    $ VBoxClient --clipboard

Или попробовать использовать команды:

    $ killall VBoxClient

    $ VBoxClient-all

Если гостевая машина windows, можно попробовать убить процесс VBoxTray.exe

// Investigating shared clipboard problems on X11 guests or hosts

https://www.virtualbox.org/wiki/X11Clipboard
