---
layout: page
title: Шаги после инсталляции Ubuntu (для себя)
permalink: /linux/ubuntu/after-installation/
---



    $ sudo su -
    # apt-get update && apt-get upgrade -y

<!--
# apt-get upgrade -y
-->


// ДОП ПО

    # apt-get install -y ubuntu-restricted-extras
    # apt-get install -y vim openssh-server traceroute

<br/>

    # apt-get install -y gnome-panel

<!--

sudo add-apt-repository universe
sudo apt-get update
sudo apt-get install gnome-session-flashback


gsettings set org.gnome.desktop.wm.preferences button-layout ':minimize,maximize,close'
-->


    # reboot

Перезагружаемся в gnome (Metacity)

Кнопки:

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





    Applications --> System Tools --> System Settings --> Keyboard --> Text Entry

    + russian



    Region and Languages

    Input Source

    + russian


    + Switch


    Keyboard Layout
    Loayouts Russian
    Option

    ----------

    background:
    #548080 - Workstation
    #B8C195 - Ноутбук


    В терминале
    Edit --> Keyboard shorkut
    снять первые 2 галочки

    Edit --> Profile Preferences --> Colors

    снять галочку --> Use colors from system theme


    система параметры
    System tools - System Settings --> Brightness & Lock

    Turn screen off when inactive for: 1 hour
    Lock screen after: 3o min



    System Settings -> Keyboard --> Shortcuts

    system -- logout disabled

    Добавить в Custom shortcuts

    Name: system-monitor
    Command: gnome-system-monitor

    Ctrl + Alt + Delete