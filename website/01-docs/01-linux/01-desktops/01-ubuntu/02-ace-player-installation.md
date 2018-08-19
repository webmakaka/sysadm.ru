---
layout: page
title: Инсталляция ACE плеера в Ubuntu
permalink: /linux/desktops/ubuntu/ace-player-installation/
---

# Инсталляция ACE плеера в Ubuntu


<br/>

### Инсталляция ACE плеера в Ubuntu 16.04 / 18.04.1


Делаю!  
19.08.2018

    $ sudo snap install acestreamplayer
    $ acestreamplayer

На вопрос о возрасте, я указал, что мне меньше 13 лет и рекламу всякого УГ (Уткина Г...) мне не стали показывать.

<div align="center">
    <img src="http://img.ipev.ru/2018/08/19/Screenshot-from-2018-08-19-00-44-55.png" border="0" alt="Установка Ace плеера в Ubuntu 18.04.1">

</div>

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

<div align="center">
    <img src="//files.sysadm.ru/img/ace-01.png" border="0" alt="Установка Ace плеера в Ubuntu">

    <br/><br/>

    <img src="//files.sysadm.ru/img/ace-02.png" border="0" alt="Установка Ace плеера в Ubuntu">

    <br/><br/>

    <img src="//files.sysadm.ru/img/ace-03.png" border="0" alt="Установка Ace плеера в Ubuntu">    

</div>


<br/>

### Ссылки на трансляции футбола (Sopcast, Ace Stream):

- livesport(.)ws
- sopsport(.)org
- football-russia(.)tv
