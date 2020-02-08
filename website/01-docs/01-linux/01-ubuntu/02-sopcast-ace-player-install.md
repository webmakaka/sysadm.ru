---
layout: page
title: Инсталляция SopCast и ACE плеера в Ubuntu
permalink: /linux/ubuntu/sopcast-ace-player-install/
---

# Инсталляция SopCast и ACE плеера в Ubuntu

<br/>

### Инсталляция SopCast плеера в Ubuntu 18.04.1 (Чего-то к трансляциям этим плеером не особо подключается в последнее время)

Делаю:  
16.04.2019

    $ sudo add-apt-repository ppa:linuxthebest.net/sopcast
    $ sudo apt-get update
    $ sudo apt-get install -y sopcast-player
    $ sopcast-player

Пока писал эти 4 команды, Месси забил 2 мяча МЮ в ЛЧ.

<br/>

### Инсталляция ACE плеера в Ubuntu 16.04 / 18.04.1

Делаю:  
13.02.2019

    $ sudo snap install acestreamplayer
    $ acestreamplayer

На вопрос о возрасте, я указал, что мне меньше 13 лет и рекламу всякого УГ мне вроде стали показывать меньше.

<br/>

![Установка Ace плеера в Ubuntu 18.04.1](/img/linux/ubuntu/ace-player-installation/ace-18-04.png "Установка Ace плеера в Ubuntu 18.04.1"){: .center-image }

<br/>

https://snapcraft.io/acestreamplayer

<br/>

Проверить работу Ace плеера:  
http://info.acestream.org/#/test

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

![Установка Ace плеера в Ubuntu](/img/linux/ubuntu/ace-player-installation/ace-01.png "Установка Ace плеера в Ubuntu"){: .center-image }

![Установка Ace плеера в Ubuntu](/img/linux/ubuntu/ace-player-installation/ace-02.png "Установка Ace плеера в Ubuntu"){: .center-image }

![Установка Ace плеера в Ubuntu](/img/linux/ubuntu/ace-player-installation/ace-03.png "Установка Ace плеера в Ubuntu"){: .center-image }

<br/>

### Ссылки на трансляции футбола (Sopcast, Ace Stream):

-   pimpletv(.)ru
-   livesport(.)ws (На них наехали, за РПФЛ обещали посадить на бутылку, а потом еще и РКН напал. В общем, спасибо РКН за блокировку сайта. Благодаря ей, трансляции возобновились)
-   football-russia(.)tv (Похоже, здесь уже нечего ловить)
