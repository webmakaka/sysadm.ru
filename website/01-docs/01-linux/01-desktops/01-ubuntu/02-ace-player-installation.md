---
layout: page
title: Инсталляция ACE плеера в Ubuntu
permalink: /linux/desktops/ubuntu/ace-player-installation/
---

# Инсталляция ACE плеера в Ubuntu


Делаю!  
31.03.2018

В общем футбол по телевизору не показывают. На сайте матч тв тоже нет, а на спортбоксе просят денег.

На сайтах выпили ссылки на сопки, а если и остались, то соп плеер не коннектится. Остались ссылки на Ace плеер. Я зх почему, но сопку активно использую, а эйс плеер нет.


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

<br/><br/>

Я не ворую фотбол. Была в свое время такая акция.<br/>
Воруют, это когда чего-то становится меньше. А так, футбола у них меньше не стало.
Придумают же ...