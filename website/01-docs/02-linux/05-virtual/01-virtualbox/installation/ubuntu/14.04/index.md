---
layout: page
title: Инсталляция VirtualBox 5.X в командной строке в Ubuntu
permalink: /linux/virtual/virtualbox/installation/ubuntu/14.04/
---

<br/>


    $ sudo su -
    # cp /etc/apt/sources.list /etc/apt/sources.list.bkp

<br/>

    # echo 'deb http://download.virtualbox.org/virtualbox/debian trusty contrib' >> /etc/apt/sources.list

<br/>

    # cd /tmp

<br/>

    # wget -q http://download.virtualbox.org/virtualbox/debian/oracle_vbox.asc -O- | sudo apt-key add -

<br/>

    # apt-get update

<br/>

    # apt-cache search virtualbox*

<br/>

    # apt-get install -y virtualbox-5.0


Создаем группу администраторов виртуальных машин:

    # groupadd -g 1005 vmadmins


Создаем пользователя для работы с виртуальными машинами

    # useradd \
    -g vmadmins \
    -d /home/vmadm \
    -s /bin/bash \
    -m vmadm


Задать пароль созданному пользователю

    # passwd vmadm

<br/>

    # su - vmadm

<br/>

    $  vi ~/.bashrc

<br/>

Добавляю в конец файла (чтобы читался .bash_profile как в redhat)

<br/>

    #
    . ~/.bash_profile


Отредактируйте файл ~/.bash_profile

    $ vi ~/.bash_profile

<br/>

Добавьте

    ############################################
    #### VirtualBox Parameters

        export VM_HOME=$HOME/machines
        export VM_BACKUPS=${VM_HOME}/backups

    ############################################


Применить новые параметры:

    $ source ~/.bash_profile  

<br/>

    $ vboxmanage --version
    5.0.12r104815


<br/>

### Установка пакетов расширения (USB, Remote Console, etc)

    $ VBoxManage list extpacks
    Extension Packs: 0

<br/>

    $ sudo su -

<br/>

    # cd /tmp/
    # wget http://download.virtualbox.org/virtualbox/5.0.12/Oracle_VM_VirtualBox_Extension_Pack-5.0.12.vbox-extpack

<br/>

    # VBoxManage extpack install Oracle_VM_VirtualBox_Extension_Pack-5.0.12.vbox-extpack

<br/>

    # VBoxManage list extpacks
    Extension Packs: 1
    Pack no. 0:   Oracle VM VirtualBox Extension Pack
    Version:      5.0.12
    Revision:     104815
    Edition:      
    Description:  USB 2.0 and USB 3.0 Host Controller, Host Webcam, VirtualBox RDP, PXE ROM, Disk Encryption.
    VRDE Module:  VBoxVRDP
    Usable:       true
    Why unusable:
