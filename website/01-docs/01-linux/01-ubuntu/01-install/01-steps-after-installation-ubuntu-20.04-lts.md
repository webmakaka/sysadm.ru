---
layout: page
title: Шаги после инсталляции Ubuntu 20.04 LTS (для себя)
description: Шаги после инсталляции Ubuntu 20.04 LTS (для себя)
keywords: ubuntu, install
permalink: /linux/ubuntu/install/steps-after-installation-ubuntu-20.04-lts/
---

# Шаги после инсталляции Ubuntu 20.04 LTS (для себя)

<br/>

### Обновление

    $ sudo su -
    # apt update && apt-get upgrade -y
    # apt install -y vim curl git


<br/>

### Установка VSCODE

    $ curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
    $ sudo install -o root -g root -m 644 microsoft.gpg /etc/apt/trusted.gpg.d/
    $ sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main" > /etc/apt/sources.list.d/vscode.list'

<br/>

    $ sudo apt-get install apt-transport-https
    $ sudo apt-get update
    $ sudo apt-get install -y code

<br/>

Дока:  
https://code.visualstudio.com/docs/setup/linux

<br/>

### Запуск sysadm.ru в редакторе vscode

    $ mkdir ~/projects && cd ~/projects
    $ git clone https://bitbucket.org/sysadm-ru/sysadm.ru
    $ cd sysadm.ru
    $ code .

<br/>

### Устанавливаем дополнительное ПО

    $ sudo apt install -y ubuntu-restricted-extras

<br/>

    $ sudo apt install -y \
        openssh-server \
        traceroute \
        vlc \
        mpv \
        transmission \
        ffmpegthumbnailer \
        net-tools \
        iputils-ping \
        rar unrar-free \
        wakeonlan \
        whois \
        gimp

<br/>

### Gnome Panel

    $ sudo apt install -y gnome-panel

    $ sudo reboot

Перезагружаемся, при старте выбираем - gnome (Metacity)


<br/>

### Не спрашивать каждый раз пароль при комаде с sudo

[Не спрашивать каждый раз пароль при комаде с sudo](/linux/ubuntu/small-improvements/)  


<br/>

### Установить нормальный background

    $ gsettings set org.gnome.desktop.background picture-options 'none'

    $ gsettings set org.gnome.desktop.background primary-color '#548080'

<br/>

### Смена раскладки клавиатуры по Alt + Shift

Вот надо что-нибудь да испортить! По умолчанию, нужно выбрать комбинацию из 3х клваиш, чтобы сменить раскладку.

    $ sudo apt-get install -y gnome-tweak-tool
    $ gnome-tweaks

<br/>

Keyboard & Mouse --> Additional Layout Options --> Switching to another layout --> Alt + Shift

Appearance --> Theme -- Yaru-dark


<br/>

### Установить формат дат в консоли на английский

<br/>

    Applications --> Preferences --> System
    
    Region & Language --> Formats --> United States

<br/>

### ПО CTRL + ALT + DELETE показывать текущие процессы

    Applications --> Preferences --> System
    
    Keyboard Shortcuts

    System --> Logout

    Убираем

<br/>

Добавляем

    Name: system-monitor
    Command: gnome-system-monitor

    CTRL + ALT + DELETE

<br/>

### Отключить противный звук при ошибке в консоли

Terminal --> Preferences

![Отключить противный звук при ошибке в консоли](/img/linux/ubuntu/install/disable-sound-when-error-in-the-console.png "Отключить противный звук при ошибке в консоли"){: .center-image }

Color --> Built-in schemes: Black on white

<br/>

### Убрать автовыключение монитора

    Applications --> Preferences --> System
    Power --> Power Saving --> Blank screen --> Never


<br/>

### Заблокировать дерьмовые сайты с рекламой казино, ставок и т.д.


$ sudo vi /etc/hosts


```
0.0.0.0 blackhole.beeline.ru
0.0.0.0 mailtrack.io
0.0.0.0 metrika.yandex.ru
0.0.0.0 informer.yandex.ru
0.0.0.0 naydex.net
0.0.0.0 *.naydex.net

0.0.0.0 rbc.ru
0.0.0.0 lenta.ru
0.0.0.0 betcity.ru

0.0.0.0 jivosite.ru
0.0.0.0 www.jivosite.ru
0.0.0.0 content.mql5.com

0.0.0.0 onlinefreecourse.net
0.0.0.0 downloadtutorials.net
0.0.0.0 ebookie.org
0.0.0.0 winline.ru
0.0.0.0 downloadtutorials.net

0.0.0.0 content.mql5.com



81.17.30.22 nnm-club.me

```

+

Отсюда добавим кучу сайтов:  
https://github.com/michaeltrimm/hosts-blocking/blob/master/_hosts.txt


<br/>

**Для этого:**

<br/>

    $ cd /tmp
    $ curl https://raw.githubusercontent.com/michaeltrimm/hosts-blocking/master/_hosts.txt --output badwebsites.txt

    $ sudo su -
    # cd /tmp

    # cat ./badwebsites.txt >> /etc/hosts


<br/>

### Дополнительное ПО

[Chrome](/linux/ubuntu/browsers/chrome/)  
[Opera](/linux/ubuntu/browsers/opera/)


<br/>

**Telegram**

Snap -> Telegram -> прописать автозапуск в gnome-tweaks

    $ sudo mv Telegram /opt/telegram

    $ gnome-tweaks


<!--

<br/>

### Автозапуск telegram


    $ sudo mkdir -p /opt/telegram
    $ sudo mv Telegram /opt/telegram/

<br/>


Applications -> System Tools -> Preferences -> Startup Applications

<br/>

Name: Telegram  
Command: /opt/telegram/Telegram -startintray

![Автозапуск telegram](/img/linux/ubuntu/install/autostart-telegram.png "Автозапуск telegram"){: .center-image }

-->






