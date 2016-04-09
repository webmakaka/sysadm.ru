---
layout: page
title: Docker в Linux
permalink: /linux/virtual/docker/
---

Вроде как, в качестве хостовой машины для docker контейнеров отлично подойдет CoreOS. Желающих прокачать скилы по работе с Docker, предлагаю присмотреться и к этому дистрибутиву. Желающие копать совместно, присоединяйтесь.


<br/><br/>
Здесь собираются материалы по работе с docker, начиная с версии 1.1. Материалы постепенно обновляются по мере необходимости обращения к ним.
<br/><br/>

<strong>P.S.!!! В контейнерах для Centos7 не работает из коробки systemd!</strong>

<br/><br/>

[Подготовленные image]  
https://hub.docker.com/explore/


[YouTube Playlist]  
http://www.youtube.com/playlist?list=PLkA60AVN3hh_6cAz8TUGtkYbJSL2bdZ4h


[Docker Tutorial]  
https://gist.github.com/sysadm-ru/bfc6e91fa891b4d457522212acaa8810

<br/><br/>


### Docker

[Имидж и контейнер, в чем собственно разница?](/linux/virtual/docker/basics/images-and-containers/)  

[Основные команды Docker](/linux/virtual/docker/basics/basic-commands/)  
<br/>

### Docker Installation


[Инсталляция Docker в Ubuntu 14.04](/linux/virtual/docker/basics/installing-docker-on-ubuntu/)  
[Инсталляция Docker в CentOS 7](/linux/virtual/docker/basics/installing-docker-on-centos/)  

[Инсталляция Docker-Compose в Ubuntu 14.04 (организовать работу нескольких контейнеров)](/linux/virtual/docker/basics/installing-docker-compose-on-ubuntu/)  



<br/>

### Docker Update | Upgrade

[Upgrade Docker в Ubuntu](/linux/virtual/docker/basics/upgrade-docker-on-ubuntu/)  


<br/>

### Переместить файлы Docker


[Переместить файлы Docker](/linux/virtual/docker/basics/move-docker-files/)  


<br/>

### Docker NetWorking (Не особо и нужно. Но возможность такая есть)

[Базовая настройка сети для Docker с использованием моста в Ubuntu (из видеокурса)](/linux/virtual/docker/networking/ubuntu-bridge/)  
[Задание параметров сетевых интерфейсов docker в Ubuntu (IP, gateway, etc.)](/linux/virtual/docker/networking/ubuntu-bridge/bridge-my-version/)  


<br/>

### Docker Linking Containers


Если я все правильно понимаю. То лучше использовать docker-compose для этих целей. (Когда начинал изучать, еще вроде не было никаких compose'ов). Для работы с docker-compose нужные версии docker 1.8 и выше (опять же вроде).


[Пример линковки контейнеров для их совместной работы](/linux/virtual/docker/linking-containers/manual-linking/)  

[С помощью Docker Compose](/linux/virtual/docker/linking-containers/docker-compose/)



<br/>

### Docker Работа с image

[Скопировать image на другой HOST](/linux/virtual/docker/basics/copying-images-to-other-hosts/)  
[Скопировать image на hub.docker.com и забрать image с него](/linux/virtual/docker/basics/push-and-pull-docker-image-to-hub/)  

<br/>

### Dockerfile

[Мой Dockerfile для разработки rails и node.js приложений в centos 6](/linux/virtual/docker/dockerfile/)  

<br/>

### Docker Clustering (нужно заново начинать разбираться)

[Clustering docker with swarm](/linux/virtual/docker/clustering/ubuntu/)  


<br/>

### Информация о запущенных контейнерах

[SYSDIG](/linux/virtual/docker/sysdig/)  


<br/>

### Docker Mashine

[Docker Mashine](/linux/virtual/docker/mashine/)  

<br/>

### Docker в Windows

[В 10 пока не работает](/windows/virtual/docker/)


<br/>

### Docker практические задачи

[Перенос форума в контейнеры Docker (В работе)](/linux/virtual/docker/odba/)


___

<br/>

**Возможно, полезные статьи по docker:**


<ul>

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
