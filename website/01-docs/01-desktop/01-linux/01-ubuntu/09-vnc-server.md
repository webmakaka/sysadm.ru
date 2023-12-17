---
layout: page
title: Удаленный рабочий стол в Ubuntu 22.04
description: Удаленный рабочий стол в Ubuntu 22.04
keywords: linux, vncserver, vncviewer
permalink: /desktop/linux/ubuntu/vnc-server/
---

# Удаленный рабочий стол в Ubuntu 22.04

<br/>

**Последний раз делаю:**  
2023.11.04

<br/>

По этой статье:  
https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-vnc-on-ubuntu-22-04

<br/>

Для компьюетера во внутренней сети.

<br/>

```
$ sudo apt install -y xfce4 xfce4-goodies
$ sudo apt install -y tightvncserver
```

<br/>

```
$ mkdir ~/.vnc/
$ vi ~/.vnc/xstartup
```

<br/>

```bash
#!/bin/bash
xrdb $HOME/.Xresources
startxfce4 &
```

<br/>

```
$ chmod +x ~/.vnc/xstartup
```

<br/>

```
$ vncserver
```

<br/>

```
$ sudo vi /etc/systemd/system/vncserver@.service
```

<br/>

Нужно marley заменить на своего пользователя.

<br/>

```bash
[Unit]
Description=Start TightVNC server at startup
After=syslog.target network.target

[Service]
Type=forking
User=marley
Group=marley
WorkingDirectory=/home/marley

PIDFile=/home/marley/.vnc/%H:%i.pid
ExecStartPre=-/usr/bin/vncserver -kill :%i > /dev/null 2>&1
ExecStart=/usr/bin/vncserver -depth 24 -geometry 1920x1080 :%i
ExecStop=/usr/bin/vncserver -kill :%i

[Install]
WantedBy=multi-user.target
```

<br/>

```
$ sudo systemctl daemon-reload
$ sudo systemctl enable vncserver@1.service
$ sudo systemctl start vncserver@1
```

<br/>

```
$ systemctl status vncserver@1.service
$ sudo systemctl restart vncserver@1.service
```

<br/>

### Удаленное подключение с клиента с помощью remmina

<br/>

```
$ remmina
```

<br/>

После запуска, следует создать новый профиль для подключения. Стартовое окно оно не для подключения а для фильтрации уже имеющися подключений.

<br/>

**Для linux хостов:**

```
Name: home-download

Protocol: Remmina VNC Plugin

Server: <ip_server>:5901
```

<!-- <br/>

**Для windows хостов:**

RDP

В общих настройках remmina выбрать

Preferences -> RDP -> Use client keyboard mapping -->

<br/>

В настройках подключения к хосту:

Remote Desktop Preference -> Advanced -> Quality -> Best (slowest)

<br/>

### tigervnc

Альтернативно, можно использовать как вариант.

<br/>

```
sudo apt install tigervnc-viewer
```

<br/>

```
$ vncviewer 192.168.1.8:5901
```
