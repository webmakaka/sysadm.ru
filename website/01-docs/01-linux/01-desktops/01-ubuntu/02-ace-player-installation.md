---
layout: page
title: Инсталляция ACE плеера в Ubuntu
permalink: /linux/desktops/ubuntu/ace-player-installation/
---

# Инсталляция ACE плеера в Ubuntu

<br/>

### Инсталляция ACE плеера в Ubuntu 16.04 / 18.04.1

Делаю:  
13.02.2019

    $ sudo snap install acestreamplayer
    $ acestreamplayer

На вопрос о возрасте, я указал, что мне меньше 13 лет и рекламу всякого УГ мне вроде стали показывать меньше.

![Установка Ace плеера в Ubuntu 18.04.1](/img/linux/desktops/ubuntu/ace-player-installation/ace-18-04.png "Установка Ace плеера в Ubuntu 18.04.1"){: .center-image }

<br/>

https://snapcraft.io/acestreamplayer

<br/>

### Инсталляция ACE плеера в Ubuntu 14.04

<br/>

    $ sudo su -

    # echo "deb http://repo.acestream.org/ubuntu/ trusty main" >> /etc/apt/sources.list.d/ace_stream.list

<br/>

    # wget -O - http://repo.acestream.org/keys/acestream.public.key | sudo apt-key add -

<br/>

    # apt-get update
    # apt-get install -y acestream-full

<br/>

Далее, запустить плеер можете в списке программ "Музыка и видео", или в командной строке:

    $ acestreamplayer

<br/>

Пока писал, какой голешник забили!

<br/>

![Установка Ace плеера в Ubuntu](/img/linux/desktops/ubuntu/ace-player-installation/ace-01.png "Установка Ace плеера в Ubuntu"){: .center-image }

![Установка Ace плеера в Ubuntu](/img/linux/desktops/ubuntu/ace-player-installation/ace-02.png "Установка Ace плеера в Ubuntu"){: .center-image }

![Установка Ace плеера в Ubuntu](/img/linux/desktops/ubuntu/ace-player-installation/ace-03.png "Установка Ace плеера в Ubuntu"){: .center-image }

<br/>

### Ссылки на трансляции футбола (Sopcast, Ace Stream):

-   livesport(.)ws
-   sopsport(.)org
-   football-russia(.)tv
