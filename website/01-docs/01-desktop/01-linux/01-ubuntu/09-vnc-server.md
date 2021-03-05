---
layout: page
title: Удаленный рабочий стол в Ubuntu 20.04
description: Удаленный рабочий стол в Ubuntu 20.04
keywords: linux, vncserver, vncviewer
permalink: /desktop/linux/ubuntu/vnc-server/
---

# Рекомендация!

Посмотреть видео с ютбуба внизу.

# Удаленный рабочий стол в Ubuntu 20.04

Последний раз делаю:  
05.03.2021

В общем у меня изначально было серое окно с курсором. И я ничего не мог с этим поделать.

Нашел рабочее решение с использованием xfce4

<br/>

### Решение с использованием xfce4

    $ sudo apt install -y xfce4 xfce4-goodies
    $ sudo apt install -y tightvncserver

<br/>

    $ vncserver

    $ vncserver -kill :1

    $ cp ~/.vnc/xstartup ~/.vnc/xstartup.orig

    $ vi ~/.vnc/xstartup

<br/>

После строки: “xrdb $HOME/.Xresources” добавляю строку

```
startxfce4 &
```

    $ sudo chmod +x ~/.vnc/xstartup

    $ vncserver

<br/>

На клиенте:

// UPD Наверное, лучше вот этот использовать  
https://www.realvnc.com/en/connect/download/viewer/linux/

<br/>

    $ vncviewer

Подключаемся к <ip_server>:5901

<br/>

**По материалам отсюда:**

https://www.linuxbuzz.com/setup-vnc-server-ubuntu-18-04-debian-9/

<br/>

### Вариант с Gnome Desktop (Заработал так себе. Серый экран на background'е)

Если знаете как победить, напишите.  
Но менюшки и программы рабочие.

```
$ sudo apt-get install -y ubuntu-gnome-desktop

// Хз нужны или нет
$ sudo apt-get install -y xfonts-100dpi xfonts-100dpi-transcoded xfonts-75dpi xfonts-75dpi-transcoded xfonts-base

$ sudo apt install -y tightvncserver
```

Далее, тоже самое, что и выше, только добавить вместо startxfce4 нужно:

```
gnome-session &
gnome-panel &
gnome-settings-daemon &
metacity &
nautilus &
gnome-terminal &
```

Может здесь что лишнее, или чего-то не хватает.
Пока буду юзать первый вариант. Но и второй победить бы тоже хотелось.

<br/>

**Наверное полезная статья:**

https://www.digitalocean.com/community/tutorials/vnc-ubuntu-16-04-ru

<br/>

### Ubuntu VNC Server

https://www.youtube.com/watch?v=3K1hUwxxYek
