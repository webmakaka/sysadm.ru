---
layout: page
title: Инсталляция VirtualBox 5.X в командной строке в Ubuntu
permalink: /linux/servers/virtual/virtualbox/installation/ubuntu/14.04/
---


# Инсталляция VirtualBox 5.X в командной строке в Ubuntu


    $ sudo su -
    # cp /etc/apt/sources.list /etc/apt/sources.list.orig

<br/>

    # echo 'deb http://download.virtualbox.org/virtualbox/debian trusty contrib' >> /etc/apt/sources.list

<br/>

    # cd /tmp

<br/>

    # wget -q http://download.virtualbox.org/virtualbox/debian/oracle_vbox.asc -O- | sudo apt-key add -

<br/>

    # apt-get update -y

<br/>

    # apt-cache search virtualbox*

<br/>

Последняя 5.1 ее и ставлю

    # apt-get install -y virtualbox-5.1


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

{% highlight shell %}

###############################
# USER DEFINED
. ~/.bash_profile
###############################

{% endhighlight %}


Отредактируйте файл ~/.bash_profile

    $ vi ~/.bash_profile

<br/>

Добавьте


{% highlight shell %}

############################################
#### VirtualBox Parameters

    export VM_HOME=$HOME/machines

############################################

{% endhighlight %}




Применить новые параметры:

    $ source ~/.bash_profile  

<br/>

    $ vboxmanage --version
    5.1.26r117224


<br/>

### Установка пакетов расширения (USB, Remote Console, etc)

    $ VBoxManage list extpacks
    Extension Packs: 0

<br/>

    $ sudo su -

<br/>

    # cd /tmp/
    # wget http://download.virtualbox.org/virtualbox/5.1.26/Oracle_VM_VirtualBox_Extension_Pack-5.1.26.vbox-extpack

<br/>

    # VBoxManage extpack install Oracle_VM_VirtualBox_Extension_Pack-5.1.26.vbox-extpack

<br/>

    # VBoxManage list extpacks
    Extension Packs: 1
    Pack no. 0:   Oracle VM VirtualBox Extension Pack
    Version:      5.1.26
    Revision:     117224
    Edition:      
    Description:  USB 2.0 and USB 3.0 Host Controller, Host Webcam, VirtualBox RDP, PXE ROM, Disk Encryption, NVMe.
    VRDE Module:  VBoxVRDP
    Usable:       true
    Why unusable:
