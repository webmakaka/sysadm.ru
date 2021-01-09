---
layout: page
title: Инсталляция VirtualBox в командной строке в Centos / Oracle Linux
description: Инсталляция VirtualBox в командной строке в Centos / Oracle Linux
keywords: Инсталляция VirtualBox в командной строке в Centos / Oracle Linux
permalink: /devops/linux/virtual/virtualbox/setup/centos/7/
---

# Инсталляция VirtualBox в командной строке в Centos / Oracle Linux

    # yum install -y dnf
    # dnf update -y

<br/>

    # dnf install -y \
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

    -- последняя версия в репо 5.2
    # yum install -y VirtualBox-5.2.x86_64

<br/>

### The vboxdrv kernel module is not loaded.

    # vboxmanage list vms
    WARNING: The vboxdrv kernel module is not loaded. Either there is no module
             available for the current kernel (4.1.12-61.1.18.el7uek.x86_64) or it failed to
             load. Please recompile the kernel module and install it by

               sudo /sbin/vboxconfig

             You will not be able to start VMs until this problem is fixed.

<br/>

    # /sbin/vboxconfig
    vboxdrv.sh: Stopping VirtualBox services.
    vboxdrv.sh: Building VirtualBox kernel modules.
    This system is currently not set up to build kernel modules.
    Please install the Linux kernel "header" files matching the current kernel
    for adding new hardware support to the system.
    The distribution packages containing the headers are probably:
        kernel-uek-devel kernel-uek-devel-4.1.12-61.1.18.el7uek.x86_64
    This system is currently not set up to build kernel modules.
    Please install the Linux kernel "header" files matching the current kernel
    for adding new hardware support to the system.
    The distribution packages containing the headers are probably:
        kernel-uek-devel kernel-uek-devel-4.1.12-61.1.18.el7uek.x86_64

    There were problems setting up VirtualBox.  To re-start the set-up process, run
      /sbin/vboxconfig
    as root.

<br/>

    -- закопипаситл пакеты, которые нужно поставить из информационного сообщения
    # dnf install -y kernel-uek-devel-4.1.12-61.1.18.el7uek.x86_64

<br/>

    # /sbin/vboxconfig

<br/>

    # vboxmanage --version
    5.2.0r118431

<br/>

### Инсталляция Extension Pack

    # cd /tmp/
    # wget http://download.virtualbox.org/virtualbox/5.2.0/Oracle_VM_VirtualBox_Extension_Pack-5.2.0.vbox-extpack

<br/>

    # VBoxManage extpack install Oracle_VM_VirtualBox_Extension_Pack-5.2.0.vbox-extpack

<br/>

    # VBoxManage list extpacks
    Extension Packs: 1
    Pack no. 0:   Oracle VM VirtualBox Extension Pack
    Version:      5.2.0
    Revision:     118431
    Edition:
    Description:  USB 2.0 and USB 3.0 Host Controller, Host Webcam, VirtualBox RDP, PXE ROM, Disk Encryption, NVMe.
    VRDE Module:  VBoxVRDP
    Usable:       true
    Why unusable:

<br/>

### Создание специального пользователя (если нужно)

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

    # User specific environment and startup programs
    ############################################
    #### VirtualBox Parameters

               export VM_HOME=$HOME/machines
               export VM_BACKUPS=$HOME/backups
               export VM_ISO=$HOME/iso

    ############################################

Применить новые параметры:

    $ source ~/.bash_profile

<br/>

    $ mkdir -p ${VM_HOME}
    $ mkdir -p ${VM_BACKUPS}
    $ mkdir -p ${VM_ISO}

Далее создаем виртуальную машину

<br/>

### Понадобился hostonly интерфейс. Он сам автоматом не создался

    $ vboxmanage list hostonlyifs

пусто

    $ VBoxManage hostonlyif create

<br/>

    $ vboxmanage list hostonlyifs
    Name:            vboxnet0
    GUID:            786f6276-656e-4074-8000-0a0027000000
    DHCP:            Disabled
    IPAddress:       192.168.56.1
    NetworkMask:     255.255.255.0
    IPV6Address:
    IPV6NetworkMaskPrefixLength: 0
    HardwareAddress: 0a:00:27:00:00:00
    MediumType:      Ethernet
    Status:          Down
    VBoxNetworkName: HostInterfaceNetworking-vboxnet0

<br/>

    $ VBoxManage hostonlyif ipconfig vboxnet0 --ip 192.168.56.1

    $ sudo ifconfig vboxnet0 up

    $ ifconfig vboxnet0

<br/>

### Порядок виртуальных интерфейсов у меня д.б.

    1 интерфейс hostonly
    2 интерфейс nat
