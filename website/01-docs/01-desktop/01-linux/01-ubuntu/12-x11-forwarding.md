---
layout: page
title: X11-forwarding в Ubuntu
description: X11-forwarding в Ubuntu
keywords: desktop, linux, ubuntu, x11-forwarding
permalink: /desktop/linux/ubuntu/x11-forwarding/
---

# X11 forwarding

<br/>

**НЕ РАБОТАЕТ!!!**

Ненавижу всю эту X11 !!! Не понимаю как работает !!!!

<br/>

    // Разрешить всем
    $ xhost +

    // Или задать какой-то определенный порт
    $ xhost +192.168.1.9

    $ xclock

<br/>

Появились часы на локалхосте с локалхоста

<br/>

    $ echo $DISPLAY
    :1

Работает только с таким параметром.

<br/>

### Пытаюсь настроить для подключения с удаленного хоста. На клиенте настраиваю.

<br/>

Возможно, что нужно подключаться не root пользователем.

<br/>

    // Должно быть yes
    $ sudo vi /etc/ssh/sshd_config

<br/>

```
X11Forwarding yes
X11DisplayOffset 10
X11UseLocalhost yes
```

<br/>

Чтобы писать 10.0 а не 0.0

<br/>

    // Рестартуем при необходимости
    $ sudo service ssh restart

<br/>

    $ sudo apt install -y nmap nc

<br/>

    $ netstat -an | grep -F 6000

<br/>

    tcp        0      0 0.0.0.0:6000            0.0.0.0:*               LISTEN
    tcp6       0      0 :::6000                 :::*                    LISTEN

<br/>

```
192.168.1.9 - рабочая станция с ubuntu
192.168.1.11 - сервер
```

<br/>

    # nmap -p 6000 192.168.1.9

<br/>

    Starting Nmap 5.21 ( http://nmap.org ) at 2013-08-18 04:13 MSK
    Nmap scan report for 192.168.1.9
    Host is up (0.000044s latency).
    PORT     STATE SERVICE
    6000/tcp open  X11

<br/>

    $ nc -vv 192.168.1.9 6000
    Connection to 192.168.1.200 6000 port [tcp/x11] succeeded!=

<br/>

### Подключаемся к серверу

```
// option -X to enable X11 forwarding
$ ssh -X username@hostname

// option -X to enable trusted X11 forwarding
$ ssh -Y username@hostname

$ export DISPLAY='IP:0.0'
$ xclock
```

<br/>

https://aws.amazon.com/blogs/compute/how-to-enable-x11-forwarding-from-red-hat-enterprise-linux-rhel-amazon-linux-suse-linux-ubuntu-server-to-support-gui-based-installations-from-amazon-ec2/
