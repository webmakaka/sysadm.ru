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

<strong>P.S.!!! В контейнерах для Centos7 не работает из коробки systemd! По крайней мере на момент попытки запуска мной! Сейчас в основном использую контейнеры с Debian / Ubuntu</strong>



<br/><br/>

[Последняя версия Docker Community Edition] - на апрель 2018 - 18.03.0-ce (2018-03-21)  
https://docs.docker.com/release-notes/docker-ce/

[Подготовленные image]  
https://hub.docker.com/explore/


[Docker Registry (Network Storage For Docker Images)] (облачный сервис для хранения контейнеров)  
https://hub.docker.com  
https://quay.io


На hub.docker.com можно делать автоматически генерируемые контейнеры. Для этого необходимо указать где сервису взять Dockerfile. Например на github или bitbucket. При изменении файла, собирается заново. Вот пример моего контейнера, https://hub.docker.com/r/marley/nodejs/builds/ - автоматически собирается при обновлении контейнера от официального поставщика контейнеров в node.js.

Можно создать свой Registry (мне пока не был нужен):  

https://docs.docker.com/registry/  
https://www.digitalocean.com/community/tutorials/how-to-set-up-a-private-docker-registry-on-ubuntu-14-04



<br/><br/>


### Docker

[Имидж и контейнер, в чем собственно разница?](/linux/containers/docker/basics/images-and-containers/)  

[Основные команды Docker](/linux/containers/docker/basics/basic-commands/)

<br/>

### Инсталляция Docker

[Инсталляция / Upgrade Docker в Ubuntu](/linux/containers/docker/installation/ubuntu/)  
[Инсталляция Docker в CentOS 7](/linux/containers/docker/installation/centos/7/)  


<br/>

### Запуск Docker контейнера

[Запуск Ubuntu Docker контейнера](/linux/containers/docker/run/)  


<br/>

### Docker Tools

<br/>

[Docker-Compose (для совместной работы контейнеров)](/linux/containers/docker/toosl/docker-compose/)  

[Docker Machine (для запуска контейнеров в virtualbox, обычно в windows)](/linux/containers/docker/docker-machine/)  


<br/>

### Docker NetWorking (Не особо и нужно. Но возможность такая есть (или по крайней мере была в версии 1.3))

https://docs.docker.com/engine/userguide/networking/


[Настройка моста для работы с Docker в Ubuntu](/linux/containers/docker/networking/ubuntu-bridge/)  
[Задание параметров сетевых интерфейсов docker в Ubuntu (IP, gateway, etc.)](/linux/containers/docker/networking/ubuntu-bridge/bridge-my-version/)  


<br/>

### Docker Linking Containers


Лучше использовать <a href="/linux/containers/docker/toosl/docker-compose/">docker-compose</a> для линковки контейнеров.
Для работы с docker-compose нужные версии docker >= 1.8.


[Пример линковки контейнеров для их совместной работы](/linux/containers/docker/linking-containers/manual-linking/)  



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

[Docker Swarm](/linux/containers/docker/clustering/swarm/)  

<br/>

### Dockerfile

[здесь](/linux/containers/docker/dockerfile/)  


<br/>

### Примеры конфигов работы с Docker

[Docker for Web Developers (видеокурс)](https://bitbucket.org/sysadm-ru/docker-for-web-developers)

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
