---
layout: page
title: Инсталляция VirtualBox 6.X в командной строке в Ubuntu 20.04.1
description: Инсталляция VirtualBox 6.X в командной строке в Ubuntu 20.04.1
keywords: linux, virtual, ubuntu, virtualbox, installation, command line
permalink: /devops/linux/virtual/virtualbox/setup/ubuntu/
---

# Инсталляция VirtualBox 6.X в командной строке в Ubuntu 20.04.1

Делаю:  
09.01.2021

<br/>

    $ sudo su -

<br/>

    # cd /tmp

    # echo "deb [arch=amd64] http://download.virtualbox.org/virtualbox/debian $(lsb_release -cs) contrib" >> /etc/apt/sources.list.d/virtualbox.list

<br/>

    # wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -

<br/>

    # apt update -y

<br/>

    # apt-cache search virtualbox*

<br/>

**Последняя 6.1 ее и ставлю**

    # apt install -y virtualbox-6.1

<br/>

    # vboxmanage --version
    6.1.16r140961

<br/>

**Если ошибка:**

```
Some packages could not be installed. This may mean that you have
requested an impossible situation or if you are using the unstable
distribution that some required packages have not yet been created
or been moved out of Incoming.
The following information may help to resolve the situation:

The following packages have unmet dependencies:
 virtualbox-6.1 : Depends: libvpx5 (>= 1.6.0) but it is not installable
                  Recommends: libsdl-ttf2.0-0 but it is not going to be installed
E: Unable to correct problems, you have held broken packages.
```

<br/>

Скорее всего, вы (как и я) указали неправильную версию дистрибутива.

В файле /etc/apt/sources.list.d/virtualbox.list

```
Для 20.04 focal
Для 18.04 bionic
и т.д.
```

<br/>

### Обновить VirtualBox в Ubuntu

    $ sudo apt-get update
    $ sudo apt-cache search virtualbox
    $ sudo apt-get install -y virtualbox-6.0
    $ vboxmanage --version

<br/>

### Установка пакетов расширения (USB, Remote Console, etc)

Делаю:  
26.09.2019

Проприетарная, по идее, требует денег за использование в организациях.

Мне иногда нужна для удаленного доступа, поэтому обычно устанавливаю сразу вместе с virtualbox

<br/>

    -- если нужно удалить старый
    $ VBoxManage extpack uninstall  "Oracle VM VirtualBox Extension Pack"

<br/>

    $ VBoxManage list extpacks
    Extension Packs: 0

<br/>    
    
    $ cd /tmp/
    $ wget http://download.virtualbox.org/virtualbox/6.0.10/Oracle_VM_VirtualBox_Extension_Pack-6.0.10.vbox-extpack
    $ VBoxManage extpack install Oracle_VM_VirtualBox_Extension_Pack-6.0.10.vbox-extpack

<br/>

    $ VBoxManage list extpacks
    Extension Packs: 1
    Pack no. 0:   Oracle VM VirtualBox Extension Pack
    Version:      6.0.10
    Revision:     132072
    Edition:
    Description:  USB 2.0 and USB 3.0 Host Controller, Host Webcam, VirtualBox RDP, PXE ROM, Disk Encryption, NVMe.
    VRDE Module:  VBoxVRDP
    Usable:       true
    Why unusable:

<br/>

### Инсталляция Guest Additions в командной строке

Делаю:  
26.09.2019

**Нужно устанавливать в виртуальной машине!**

Я забыл об этом и долго тупил с ошибкой. **modprobe vboxguest failed**

<br/>

    # modprobe vboxguest
    modprobe: ERROR: could not insert 'vboxguest': No such device

<br/>

Обычно виртуалки использую без GUI.

Пакет Guest Additions как минимум нужен для того, чтобы мышка по экрану нормально перемещалась, работала copy+paste и может быть что-то еще. Нужно ли устанавливать guest additions, если предстоит работать только в командной строке, наверное нет.

Installation guide

http://www.virtualbox.org/manual/ch04.html#idp11277648

<br/>

**Пример в Ubuntu:**

    $ sudo su -

    # apt-get install -y wget
    # apt-get install -y gcc make perl
    # apt-get install -y p7zip-full

    # cd /tmp

    # wget http://download.virtualbox.org/virtualbox/6.0.12/VBoxGuestAdditions_6.0.12.iso


    # 7z x ./VBoxGuestAdditions_6.0.12.iso -o./VBoxGuestAdditions_6.0.12/

    # cd VBoxGuestAdditions_6.0.12/

    # chmod +x ./VBoxLinuxAdditions.run

    # ./VBoxLinuxAdditions.run

    # reboot

<br/>

### Дополнительные настройки

<br/>

    $ vi ~/.bashrc

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

```shell

### VirtualBox ################

export VM_HOME=$HOME/machines

###############################

```

<br/>

Применить новые параметры:

    $ source ~/.bash_profile

<!--

<br/>

### Работа с Plugin в Vagrant

    $ vagrant plugin list
    $ vagrant plugin update


<br/>

Словил ошибку:

**modprobe vboxguest failed**

 # apt-get install -y virtualbox-dkms
    # apt-get install -y virtualbox-guest-dkms
    # apt-get install -y linux-headers-virtual

Не помогло.

    # /sbin/rcvboxadd quicksetup all

 -->

<!-- <br/>




Еще возможные варианты:

    # modprobe vboxdrv
    # modprobe vboxvideo
    # modprobe vboxsf


    # ./VBoxLinuxAdditions.run -->

<!--


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


-->