---
layout: page
title: Инсталляция Guest Additions в Ubuntu Linux в командной строке
permalink: /linux/virtual/virtualbox/guest-additions-installation-in-command-line/
---

###  Guest Additions


Пакет как минимум нужен, для того, чтобы мышка по экрану нормально перемещалась, работала copy+paste и может быть что-то еще. Нужно ли устанавливать guest additions, если предстоит работать только в командной строке, не скажу, не знаю.

Installation guide

http://www.virtualbox.org/manual/ch04.html#idp11277648

Downloads

http://download.virtualbox.org/virtualbox/4.3.30/VBoxGuestAdditions_4.3.30.iso

Пример в Ubuntu:

    $ sudo su -

    # apt-get install -y wget

    # cd /tmp

    # wget http://download.virtualbox.org/virtualbox/5.0.10/VBoxGuestAdditions_5.0.10.iso


<br/>

### Ubuntu

    # apt-get install p7zip-full

    # 7z x ./VBoxGuestAdditions_5.0.10.iso -o./VBoxGuestAdditions_5.0.10/

<br/>

### Centos x64

    # wget http://pkgs.repoforge.org/rpmforge-release/rpmforge-release-0.5.3-1.el6.rf.x86_64.rpm

    # rpm -ivh rpmforge-release-0.5.3-1.el6.rf.x86_64.rpm

    # yum install -y p7zip

    # 7za x ./VBoxGuestAdditions_5.0.10.iso -o./VBoxGuestAdditions_5.0.10/

<br/>

### Продолжаем


    # cd VBoxGuestAdditions_5.0.10/

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
