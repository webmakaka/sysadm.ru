---
layout: page
title: Удаленный рабочий стол в Ubuntu 20.04
description: Удаленный рабочий стол в Ubuntu 20.04
keywords: linux, vncserver, vncviewer
permalink: /desktop/linux/ubuntu/vnc-server/
---

# Удаленный рабочий стол в Ubuntu 20.04

Последний раз делаю:  
01.05.2021

<br/>

### Рекомендация!

Посмотреть видео с ютбуба внизу.

<br/>

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

<br/>


```
$ sudo chmod +x ~/.vnc/xstartup

$ vncserver
```

<br/>

### На клиенте:

Можно подключиться с помощью reminna. Которая в моем дистрибутиве, похоже, что предустановлена.

<br/>

```
$ sudo apt-add-repository ppa:remmina-ppa-team/remmina-next ; sudo apt-get update ; sudo apt-get install -y remmina remmina-plugin-rdp
```

<br/>

    $ remmina

После запуска, следует создать новый профиль для подключения. Стартовое окно оно не для подключения а для фильтрации уже имеющися подключений.

<br/>

**Для linux хостов:**

Подключаемся к <ip_server>:5901

Protocol: Remmina VNC Plugin

<br/>

Блин, не помню откуда 5901.


<br/>

**Для windows хостов:**

RDP 

В общих настройках remmina выбрать

Preferences -> RDP -> Use client keyboard mapping


<br/>

В настройках подключения к хосту:

Remote Desktop Preference -> Advanced -> Quality -> Best (slowest)

<br/>

Альтернативно, можно использовать вот этот клиент (на данный момент бесплатный)  
https://www.realvnc.com/en/connect/download/viewer/linux/

<br/>

    $ vncviewer

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
