---
layout: page
title: Шаги после инсталляции Ubuntu 18 (для себя)
permalink: /linux/desktops/install/ubuntu/steps-after-installation-ubuntu-18/
---

# Шаги после инсталляции Ubuntu 18 (для себя)


<br/>

### Обновление

    $ sudo su -
    # apt-get update && apt-get upgrade -y


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

    # reboot

Перезагружаемся, при старте выбираем - gnome (Metacity)



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

[atom](/linux/desktops/code/editors/)  
[chrome](/linux/desktops/ubuntu/chrome/)
