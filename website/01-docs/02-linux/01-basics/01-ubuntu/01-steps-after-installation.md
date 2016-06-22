---
layout: page
title: Шаги после инсталляции Ubuntu (для себя)
permalink: /linux/basics/ubuntu/steps-after-installation/
---

### Шаги после инсталляции Ubuntu (для себя)

Я уже попробовал 16.04 и вернулся на 14.04 release 04.

В 16.04 у меня сбрасывались настройки для второго монитора, мышка ходила как-то странно, были проблемы с переключением клавиатуры. Индикатор раскладки при ее переключении не переключался. Отсутствовал Ubuntu Market (или как он там называется). Компьютер несколько раз выклюался.

Вообщем, пусть пока дорабатывают, может попробую когда появится release 2 или будет какая необходимость в 16.04.


### Обновление

    $ sudo su -
    # apt-get update && apt-get upgrade -y

<!--
# apt-get upgrade -y
-->


// ДОП ПО

    # apt-get install -y ubuntu-restricted-extras

<br/>

    # apt-get install -y
    vim \
    openssh-server \
    traceroute \
    git \
    vlc


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

### Отключение экрана


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

    Экран белый, шрифт черный.



### Дополнительное ПО

[atom](/linux/editors/)  
[chrome](/linux/basics/ubuntu/chrome/)
