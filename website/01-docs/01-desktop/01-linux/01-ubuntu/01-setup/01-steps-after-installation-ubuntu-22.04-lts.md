---
layout: page
title: Шаги после инсталляции Ubuntu 22.04 LTS (для себя)
description: Шаги после инсталляции Ubuntu 22.04 LTS (для себя)
keywords: ubuntu, install
permalink: /desktop/linux/ubuntu/setup/steps-after-installation-ubuntu-22.04-lts/
---

# Шаги после инсталляции Ubuntu 22.04 LTS (для себя)

**Делаю:**  
14.05.2022

<br/>

### Обновление

```
$ sudo su -
# apt update && apt-get upgrade -y
# apt install -y vim curl git
```

<br/>

### Не спрашивать каждый раз пароль при комаде с sudo

[Не спрашивать каждый раз пароль при комаде с sudo](/desktop/linux/ubuntu/setup/do-not-ask-root-password/)


<br/>

### Установка VSCODE

<br/>

```
$ curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
$ sudo install -o root -g root -m 644 microsoft.gpg /etc/apt/trusted.gpg.d/
$ sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main" > /etc/apt/sources.list.d/vscode.list'
```

<br/>

```
$ sudo apt-get install apt-transport-https
$ sudo apt-get update
$ sudo apt-get install -y code
```

<br/>

Дока:  
https://code.visualstudio.com/docs/setup/linux

<br/>

### Запуск sysadm.ru в редакторе vscode

<br/>

```
$ mkdir ~/projects && cd ~/projects
$ git clone https://github.com/webmakaka/sysadm.ru.git
$ cd ~/projects/sysadm.ru
$ code .
```



<br/>

### Устанавливаем дополнительное ПО

<br/>

```
$ sudo ubuntu-drivers autoinstall
$ sudo apt install -y ubuntu-restricted-extras
```

<br/>

```
$ sudo apt install -y \
    make \
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
    gimp \
    usb-creator-gtk \
    gnome-tweaks
```

<!--
    grub-customizer \
-->


<br/>

### Gnome Panel

    $ sudo apt install -y gnome-panel

    $ sudo reboot

<br/>

Перезагружаемся, при старте выбираем - Gnome Flashback (Metacity)

<br/>

### Установить нормальный background

    $ gsettings set org.gnome.desktop.background picture-options 'none'

    $ gsettings set org.gnome.desktop.background primary-color '#548080'

    $ gsettings set org.gnome.desktop.interface color-scheme 'prefer-dark'


<br/>

### Смена раскладки клавиатуры по Alt + Shift

Вот надо что-нибудь да испортить! По умолчанию, нужно выбрать комбинацию из 3х клваиш, чтобы сменить раскладку.

    $ gnome-tweaks

<br/>

Keyboard & Mouse --> Additional Layout Options --> Switching to another layout --> Alt + Shift

<br/>

### Настройка цветовой темы

Appearance -> Theme

    Legacy Applications -> Yaru-dark
    Icons -> Ubuntu-mono-dark

<br/>

### Установить формат дат в консоли на английский

<br/>


```
$ gnome-control-center
```

    Region & Language --> Formats --> United Kingdom

<br/>

### ПО CTRL + ALT + DELETE показывать текущие процессы

    
    Kyeboard --> Keyboard Shortcuts --> View and Customize Shortcuts

    System --> Log out

    Убираем

<br/>

**Добавляем**

    Name: system-monitor
    Command: gnome-system-monitor

    CTRL + ALT + DELETE



<br/>

### Убрать автовыключение монитора

    Power --> Power Saving Options --> Blank screen --> Never

<br/>

## Настройка параметров терминала

**Отключить противный звук при ошибке в консоли**

Terminal --> Right Click --> Preferences

Unnamed -> Sound -> Terminal bell (disable)

![Отключить противный звук при ошибке в консоли](/img/desktop/linux/ubuntu/setup/disable-sound-when-error-in-the-console.png 'Отключить противный звук при ошибке в консоли'){: .center-image }

<br/>

**Шрифты, цвета и т.д.**

Unnamed --> Text --> Custom font: Ubuntu Mono Regular 22

Color --> Built-in schemes: Black on white

<br/>

## Заблокировать дерьмовые сайты с рекламой казино, ставок и т.д.

[Инфа здесь](/desktop/linux/ubuntu/browsers/block-junk-websites/)

<br/>

## Дополнительное ПО

[Chrome](/desktop/linux/ubuntu/browsers/chrome/)  
[Opera](/desktop/linux/ubuntu/browsers/opera/)

<br/>

**Telegram**

    $ sudo mv Telegram /opt/telegram

    $ gnome-tweaks

    Startup Applications

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

![Автозапуск telegram](/img/desktop/linux/ubuntu/setup/autostart-telegram.png "Автозапуск telegram"){: .center-image }

-->
