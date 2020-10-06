---
layout: page
title: Шаги после инсталляции Ubuntu 14 (для себя)
description: Шаги после инсталляции Ubuntu 14 (для себя)
keywords: Шаги после инсталляции Ubuntu 14 (для себя)
permalink: /linux/ubuntu/install/steps-after-installation-ubuntu-14/
---

# Шаги после инсталляции Ubuntu 14 (для себя)

<br/>

### Обновление

    $ sudo su -
    # apt-get update && apt-get upgrade -y

<!--
# apt-get upgrade -y
-->

// ДОП ПО

    # apt-get install -y ubuntu-restricted-extras

<br/>

    # apt-get install -y \
    vim \
    openssh-server \
    traceroute \
    git \
    vlc \
    net-tools

<br/>

### Gnome Panel

    # apt-get install -y gnome-panel

<!--

sudo add-apt-repository universe
sudo apt-get update
sudo apt-get install gnome-session-flashback


gsettings set org.gnome.desktop.wm.preferences button-layout ':minimize,maximize,close'
-->

    # reboot

Перезагружаемся, при старте выбираем - gnome (Metacity)

<br/>

Кнопки (я люблю когда они справа):

    right :
    $ gsettings set org.gnome.desktop.wm.preferences button-layout ':minimize,maximize,close'

    left :
    $ gsettings set org.gnome.desktop.wm.preferences button-layout 'close,maximize,minimize:'

<!--


Если не получится, то поставить dconf из центра загрузки


$ gconf-editor

/Apps->Metacity->general


двойной клик по button_layout


close,minimize,maximize:

на
menu:minimize,maximize,close

-->

<br/>

### Раскладка клавиатуры и язык

    Applications --> System Tools --> System Settings --> Keyboard --> Text Entry

    + russian
    + Swithc to next source: Alt+ShiftL

<br/>

    Applications --> System Tools --> System Settings --> Language Support --> Region Formats

<br/>

![Ubuntu Region Formats](/img/linux/ubuntu/install/regional-formats.png 'Ubuntu Region Formats'){: .center-image }

<br/>

### Первый день недели - понедельник

    $ cd ~
    $ cp .pam_environment .pam_environment.orig
    $ vi .pam_environment

добавить:

    #Change first day of week to Monday
    export LC_TIME=en_GB.UTF-8
    #Change to metric system
    export LC_MEASUREMENT=en_GB.UTF-8

<br/>

### Shortcuts

    System Settings -> Keyboard --> Shortcuts

    System --> logout --> disabled


    Добавить в Custom shortcuts

    Name: system-monitor
    Command: gnome-system-monitor

    ПО: Ctrl + Alt + Delete

<!-- <br/>

    Не нашел в 16.04


    Region and Languages

    Input Source

    + russian

    + Switch


    Keyboard Layout
    Loayouts Russian
    Option

-->

<br/>

### Параметры отключения экрана

System tools - System Settings --> Brightness & Lock

Turn screen off when inactive for: 1 hour
Lock screen after: 3o min

<br/>

### Фон

    background:

    #548080 - Workstation
    #B8C195 - Ноутбук

<br/>

### Terminal

Edit --> Keyboard shortcuts

Отключаю:

    Enable menu access keys
    Enable the menu shortcut key

<br/>

    Edit --> Profile Preferences --> Colors

    снять галочку --> Use colors from system theme

    Buil-in schemes: Black on white

<br/>

### Отключаю противный звук при ошибке в консоли

    $ gsettings set org.gnome.desktop.sound event-sounds false

<br/>

### Чтобы при нажатии правой кнопкой мышки по папке было поле меню "Открыть в терминале"

    $ sudo apt-get install nautilus-open-terminal
    $ nautilus -q

<br/>

### Заблокировать дерьмовые сайты с рекламой казино, ставок и т.д.

https://sysadm.ru/linux/ubuntu/install/steps-after-installation-ubuntu-20.04-lts/

<br/>

### Дополнительное ПО

[atom](/linux/code/editors/)  
[chrome](/linux/ubuntu/browsers/chrome/)
