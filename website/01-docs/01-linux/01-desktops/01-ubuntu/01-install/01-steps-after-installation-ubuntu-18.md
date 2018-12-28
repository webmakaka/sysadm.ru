---
layout: page
title: Шаги после инсталляции Ubuntu 18 (для себя)
permalink: /linux/desktops/ubuntu/install/steps-after-installation-ubuntu-18/
---

# Шаги после инсталляции Ubuntu 18 (для себя)


<br/>

### Обновление

    $ sudo su -
    # apt update && apt-get upgrade -y

// ДОП ПО

    # apt install -y ubuntu-restricted-extras

<br/>

    # apt install -y \
    vim \
    openssh-server \
    traceroute \
    git \
    vlc \
    net-tools


<br/>

### Gnome Panel

    # apt install -y gnome-panel

    # reboot

Перезагружаемся, при старте выбираем - gnome (Metacity)


<br/>

### Смена раскладки клавиатуры по Alt + Shift (При необходимости)

Вот надо что-нибудь да испортить! По умолчанию, нужно выбрать комбинацию из 3х клваиш, чтобы сменить раскладку.

    $ sudo apt-get install -y gnome-tweak-tool
    $ gnome-tweaks

<br/>

### System --> Settings --> Devices --> Keyboard

    Убрать logout по CTRL + ALT + DELETE

<br/>

Logout - убираем 

<br/>

Добавляем

    Name: system-monitor
    Command: gnome-system-monitor

    CTRL + ALT + DELETE

<br/>

### Отключить противный звук при ошибке в консоли


![Отключить противный звук при ошибке в консоли](/img/linux/desktops/ubuntu/install/disable-sound-when-error-in-the-console.png "Отключить противный звук при ошибке в консоли"){: .center-image }



<br/>

### Автозапуск telegram

System Tools --> Preferences --> Startup Applications


![Автозапуск telegram](/img/linux/desktops/ubuntu/install/autostart-telegram.png "Автозапуск telegram"){: .center-image }


<br/>

### Заблокировать дерьмовые сайты с рекламой казино, ставок и т.д.

https://github.com/michaeltrimm/hosts-blocking/blob/master/_hosts.txt

+

    0.0.0.0 rbc.ru
    0.0.0.0 lenta.ru
    0.0.0.0 betcity.ru
    0.0.0.0 blackhole.beeline.ru

    81.17.30.22 nnm-club.me



<br/>

### Дополнительное ПО

[chrome](/linux/desktops/ubuntu/chrome/)
[atom](/linux/desktops/code/editors/)  
