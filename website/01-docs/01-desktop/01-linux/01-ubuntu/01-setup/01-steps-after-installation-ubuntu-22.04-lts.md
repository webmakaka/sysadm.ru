---
layout: page
title: Шаги после инсталляции Ubuntu 22.04 LTS (для себя)
description: Шаги после инсталляции Ubuntu 22.04 LTS (для себя)
keywords: desktop, linux, ubuntu, setup, steps aftr installation
permalink: /desktop/linux/ubuntu/setup/steps-after-installation-ubuntu-22.04-lts/
---

# Шаги после инсталляции Ubuntu 22.04 LTS (для себя)

**Делаю:**  
2024.03.10

<br/>

### Обновление

```
$ sudo su -
$ sudo apt update -y && sudo apt upgrade -y
$ sudo apt install -y vim curl git
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
$ sudo apt install apt-transport-https
$ sudo apt update
$ sudo apt install -y code
```

<br/>

Дока:  
https://code.visualstudio.com/docs/setup/linux

<br/>

### Запуск sysadm.ru в редакторе vscode

<br/>

```
$ mkdir ~/projects && cd ~/projects
// $ GIT_SSH_COMMAND='ssh -i ~/.ssh/webmakaka -o IdentitiesOnly=yes' git clone git@github.com:webmakaka/sysadm.ru.git
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
// Основные
$ sudo apt install -y \
    vim \
    openssh-server \
    net-tools \
    traceroute \
    iputils-ping \
    rar unrar-free \
    wakeonlan \
    python-is-python3 \
    make gcc perl \
    p7zip-full
```

<br/>

```
// Дополнительные
$ sudo apt install -y \
    vlc \
    mpv \
    transmission \
    ffmpegthumbnailer \
    whois \
    gimp \
    krita \
    usb-creator-gtk \
    gnome-tweaks \
    gnome-screenshot
```

<!--
    grub-customizer \
-->

<br/>

### Gnome Panel

```
$ sudo apt install -y gnome-panel

$ sudo reboot
```

<br/>

Перезагружаемся, при старте выбираем - Gnome Flashback (Metacity)

<br/>

### Установить нормальный background

```
$ gsettings set org.gnome.desktop.background picture-options 'none'

$ gsettings set org.gnome.desktop.background primary-color '#548080'

$ gsettings set org.gnome.desktop.interface color-scheme 'prefer-dark'
```

<br/>

### Добавление русского языка

```
$ gnome-control-center
```

    Keyboard -> Input Sources -> +Russian

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

### Установить формат дат в консоли на английский

    Region & Language --> Formats --> United Kingdom

<br/>

### Смена раскладки клавиатуры по Alt + Shift

Вот надо что-нибудь да испортить! По умолчанию, нужно выбрать комбинацию из 3х клваиш, чтобы сменить раскладку.

<br/>

    $ gnome-tweaks

<br/>

Keyboard & Mouse --> Additional Layout Options --> Switching to another layout --> Alt + Shift

<br/>

### Настройка цветовой темы

Appearance -> Themes

    Applications -> Yaru-dark
    Icons -> Ubuntu-mono-dark

<br/>

### Убрать автовыключение монитора

    Power --> Power Saving Options --> Screen Blank --> Never

<br/>

## Настройка параметров терминала

**Отключить противный звук при ошибке в консоли**

Terminal --> Значек 3 линии --> Preferences

Unnamed -> Sound -> Terminal bell (disable)

![Отключить противный звук при ошибке в консоли](/img/desktop/linux/ubuntu/setup/disable-sound-when-error-in-the-console.png 'Отключить противный звук при ошибке в консоли'){: .center-image }

<br/>

**Шрифты, цвета и т.д.**

Unnamed -->

Text --> Text Apperarance --> Custom font: Ubuntu Mono 22

Text --> Cursor --> Cursor shape: I-Beam

Colors --> Built-in schemes: Black on white

<br/>

## Заблокировать дерьмовые сайты с рекламой казино, ставок и т.д.

[Инфа здесь](/desktop/linux/ubuntu/browser/block-junk-websites/)

<br/>

## Дополнительное ПО

[Chrome](/desktop/linux/ubuntu/browser/chrome/)  
[Opera](/desktop/linux/ubuntu/browser/opera/)

<br/>

### Telegram

    $ sudo mv telegram /opt/telegram

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
Command: /opt/telegram/telegram -startintray

![Автозапуск telegram](/img/desktop/linux/ubuntu/setup/autostart-telegram.png "Автозапуск telegram"){: .center-image }

-->
