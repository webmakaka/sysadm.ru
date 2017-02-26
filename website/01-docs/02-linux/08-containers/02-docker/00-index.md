---
layout: page
title: Docker в Linux
permalink: /linux/containers/docker/
---

# Docker в Linux

Offtopic:  
[Docker в Windows](/windows/containers/docker/)


<br/>

### Offtopic:

В качестве хостовой машины для docker контейнеров вполне подойдет CoreOS. Желающих прокачать скилы по работе с Docker, предлагаю присмотреться к этому linux. Желающие копать совместно, присоединяйтесь.


Здесь собираются материалы по работе с docker, начиная с версии 1.1. Материалы постепенно обновляются по мере необходимости обращения к ним.
Некоторые вещи обновлять не успеваю, т.к. docker развивается достаточно быстро.

Если копаете, можете помочь с обновлением и добавлением своих знаний.


<strong>P.S.!!! В контейнерах для Centos7 не работает из коробки systemd! По крайней мере на момент попытки запуска мной! Сейчас в основном использую Debian / Ubuntu</strong>


<br/>

P.S.: Скачал интересную и современную книгу "Docker Orchestration". Если кто захочет разобрать и поделиться результатами, пишите. Реально есть что изучать!



<br/><br/>

[Подготовленные image]  
https://hub.docker.com/explore/


[YouTube Playlist]  
http://www.youtube.com/playlist?list=PLkA60AVN3hh_6cAz8TUGtkYbJSL2bdZ4h


[Docker Tutorial]  
https://gist.github.com/sysadm-ru/bfc6e91fa891b4d457522212acaa8810


[Docker Registry (Network Storage For Docker Images)] (облачный сервис для хранения контейнеров)  
https://hub.docker.com  
https://quay.io

Можно создать свой Registry:  
https://docs.docker.com/registry/  
https://www.digitalocean.com/community/tutorials/how-to-set-up-a-private-docker-registry-on-ubuntu-14-04


<br/><br/>


### Docker

[Имидж и контейнер, в чем собственно разница?](/linux/containers/docker/basics/images-and-containers/)  

[Основные команды Docker](/linux/containers/docker/basics/basic-commands/)

<br/>

### Инсталляция Docker


[Инсталляция Docker в Ubuntu 14.04](/linux/containers/docker/installation/ubuntu/)  
[Инсталляция Docker в CentOS 7](/linux/containers/docker/installation/centos/)  

<br/>

[Docker-Compose (Инсталляция Docker-Compose в Ubuntu 14.04) ](/linux/containers/docker/toosl/docker-compose/installation/)  

<br/>

### Docker Update | Upgrade

[Upgrade Docker в Ubuntu](/linux/containers/docker/basics/upgrade-docker-on-ubuntu/)  


<br/>

### Docker Tools

Docker-Compose (для совместной работы контейнеров)  
[Docker Machine (для запуска контейнеров в virtualbox, обычно в windows)](/linux/containers/docker/docker-machine/)  


<br/>

### Docker NetWorking (Не особо и нужно. Но возможность такая есть)

https://docs.docker.com/engine/userguide/networking/


[Настройка моста для работы с Docker в Ubuntu](/linux/containers/docker/networking/ubuntu-bridge/)  
[Задание параметров сетевых интерфейсов docker в Ubuntu (IP, gateway, etc.)](/linux/containers/docker/networking/ubuntu-bridge/bridge-my-version/)  


<br/>

### Docker Linking Containers


Лучше использовать docker-compose для линковки контейнеров.
Для работы с docker-compose нужные версии docker >= 1.8.


[Пример линковки контейнеров для их совместной работы](/linux/containers/docker/linking-containers/manual-linking/)  

[Линковка Docker контейнеров с помощью Docker Compose](/linux/containers/docker/tools/docker-compose/)



<br/>

### Docker Работа с image

[Скопировать Docker Images на другой Host](/linux/containers/docker/basics/copying-images-to-other-hosts/)  
[Скопировать image на hub.docker.com и забрать image с него](/linux/containers/docker/basics/push-and-pull-docker-image-to-hub/)  


<br/>

### Работа с официальными и не только контейнерами

[Работа с официальным mysql Docker контейнером](/linux/containers/docker/official/containers/mysql/)  


<br/>

### Информация о запущенных контейнерах

[Получить информацию о запущенных Docker контейнерах c помощью sysdig](/linux/containers/docker/sysdig/)  


<br/>

### Docker практические задачи

[Переместить файлы Docker](/linux/containers/docker/basics/move-docker-files/)  
[Перенос форума в контейнеры Docker (В работе)](/linux/containers/docker/odba/)


<br/>

### Docker Clustering

[Docker Swarm](/linux/containers/docker/swarm/)  

**Alternatives:**

- Kubernetes
- Mesosphere
- Apache Mesos

<br/>

### Dockerfile

[здесь](/linux/containers/docker/dockerfile/)  


<br/>

### Примеры конфигов работы с Docker

[Docker for Web Developers (видеокурс)](https://github.com/sysadm-ru/Docker-for-Web-Developers)

___

<br/>

**Возможно, полезные статьи по docker:**


<ul>

    <li><a href="http://rus-linux.net/MyLDP/vm/docker/docker-tutorial.html" rel="nofollow">Docker – руководство для начинающих</a></li>

    <li><a href="http://dou.ua/lenta/articles/docker/" rel="nofollow">Виртуализация процесса разработки, часть 1: Docker</a></li>

    <li><a href="http://habrahabr.ru/post/253877/" rel="nofollow">Понимая Docker</a></li>
    <li><a href="http://habrahabr.ru/post/253999/" rel="nofollow">Docker и костыли в продакшене</a></li>
    <li><a href="http://habrahabr.ru/post/250469/" rel="nofollow">Как жить с Docker, или почему лучше с ним, чем без него?</a></li>
    <li><a href="http://habrahabr.ru/post/247969/" rel="nofollow">Docker в продакшене — чему мы научились, запустив более 300 миллионов контейнеров</a></li>
    <li><a href="http://habrahabr.ru/post/247903/" rel="nofollow">Docker: интересные особенности базовых образов</a></li>


    <li><a href="http://habrahabr.ru/post/247547/" rel="nofollow">Создание окружения для веб-разработки на основе Docker (для PHP)</a></li>
    <li><a href="http://habrahabr.ru/post/246933/" rel="nofollow">Docker, SkyDNS и SkyDock — быстро и удобно</a></li>
    <li><a href="http://habrahabr.ru/post/234829/" rel="nofollow">Оптимизация образов Docker</a></li>
    <li><a href="http://habrahabr.ru/company/infopulse/blog/237737/" rel="nofollow">Почему вам не нужен sshd в Docker-контейнере</a></li>
    <li><a href="http://habrahabr.ru/post/240509/" rel="nofollow">Docker: запуск графических приложений в контейнерах</a></li>
    <li><a href="http://habrahabr.ru/post/238069/" rel="nofollow">Деплой Rails-приложения с помощью Docker</a></li>
    <li><a href="http://habrahabr.ru/post/243953/" rel="nofollow">Docker в браузере, или как создать и «расшарить» среду разработки</a></li>
</ul>
