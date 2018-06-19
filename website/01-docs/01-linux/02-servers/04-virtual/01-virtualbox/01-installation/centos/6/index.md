---
layout: page
title: Инсталляция VirtualBox в командной строке в RedHat/Centos
permalink: /linux/servers/virtual/virtualbox/installation/centos/6/
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
