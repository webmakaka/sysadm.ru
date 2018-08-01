---
layout: page
title: Docker в Linux
permalink: /linux/servers/containers/docker/
---

# Docker в Linux

Offtopic:  
[Docker в Windows](/windows/servers/containers/docker/)


<br/>

### Offtopic:

Здесь собираются материалы по работе с docker, начиная с версии 1.1. Материалы постепенно обновляются по мере необходимости обращения к ним.

Некоторые вещи обновлять не успеваю, т.к. docker развивается достаточно быстро.

Если копаете, можете помочь с обновлением и добавлением своих знаний.


В качестве хостовой машины для docker контейнеров может быть интересен дистрибутив (уже от RedHat) - CoreOS. Впрочем, я считаю, что лучше Ubuntu и самому поставить тот же Docker и поднять доп сервисы, если они будут нужны.


<strong>P.S.!!! В контейнерах для Centos7 не работает из коробки systemd! По крайней мере на момент попытки запуска мной! Сейчас в основном использую контейнеры с Ubuntu / CoreOS</strong>

Еще стали предлагать для контейнеров использовать alpine linux. Вроде жрет меньше ресурсов и еще какие-то преимущества. Но я пока хз. 



<br/>

### Поехали

<br/>

[Последняя версия Docker Community Edition] - на апрель 2018 - 18.03.0-ce (2018-03-21)  
https://docs.docker.com/release-notes/docker-ce/

[Подготовленные image]  
https://hub.docker.com/explore/


[Docker Registry (Network Storage For Docker Images)] (облачный сервис для хранения контейнеров)  
https://hub.docker.com  
https://quay.io


На hub.docker.com можно делать автоматически генерируемые контейнеры. Для этого необходимо указать где сервису взять Dockerfile. Например на github или bitbucket. При изменении файла, собирается заново. Вот пример моего контейнера, https://hub.docker.com/r/marley/nodejs/builds/ - автоматически собирается при обновлении контейнера от официального поставщика контейнеров в node.js.

Можно создать свой Registry:  

https://docs.docker.com/registry/  
https://www.digitalocean.com/community/tutorials/how-to-set-up-a-private-docker-registry-on-ubuntu-14-04



<br/><br/>


### Инсталляция Docker

[Инсталляция Docker](/linux/servers/containers/docker/installation/)  

[Инсталляция Docker-Compose (для совместной работы контейнеров)](/linux/servers/containers/docker/tools/docker-compose/)  


<br/>

### Базовые вещи

[Имидж и контейнер, в чем собственно разница?](/linux/servers/containers/docker/basics/images-and-containers/)  

[Основные команды Docker](/linux/servers/containers/docker/basics/basic-commands/)


<br/>

### Запуск Docker контейнера

[Запуск Ubuntu Docker контейнера](/linux/servers/containers/docker/run/)  
[Пример запуска веб проекта в контейнерах Docker](https://github.com/marley-nodejs/Projects-in-Docker)  


<br/>

### Docker Tools

[Docker Machine (для запуска контейнеров в virtualbox, обычно в windows)](/linux/servers/containers/docker/docker-machine/)  


<br/>

### Docker NetWorking (Не особо и нужно. Но возможность такая есть (или по крайней мере была в версии 1.3). Не пользуюсь этой возможностью)

https://docs.docker.com/engine/userguide/networking/


[Настройка моста для работы с Docker в Ubuntu](/linux/servers/containers/docker/networking/ubuntu-bridge/)  
[Задание параметров сетевых интерфейсов docker в Ubuntu (IP, gateway, etc.)](/linux/servers/containers/docker/networking/ubuntu-bridge/bridge-my-version/)  


<br/>

### Docker Linking Containers


Лучше использовать <a href="/linux/servers/containers/docker/tools/docker-compose/">docker-compose</a> для линковки контейнеров.
Для работы с docker-compose нужные версии docker >= 1.8.


[Пример линковки контейнеров для их совместной работы](/linux/servers/containers/docker/linking-containers/manual-linking/)  



<br/>

### Docker Работа с image

[Скопировать Docker Images на другой Host](/linux/servers/containers/docker/basics/copying-images-to-other-hosts/)  
[Скопировать image на hub.docker.com и забрать image с него](/linux/servers/containers/docker/basics/push-and-pull-docker-image-to-hub/)  


<br/>

### Работа с официальными и не только контейнерами

[Lamp Server](/linux/servers/containers/docker/lamp/)
[Работа с официальным mysql Docker контейнером](/linux/servers/containers/docker/official/containers/mysql/)  



<br/>

### Информация о запущенных контейнерах

[Получить информацию о запущенных Docker контейнерах c помощью sysdig](/linux/servers/containers/docker/sysdig/)  


<br/>

### Docker практические задачи

[Переместить файлы Docker](/linux/servers/containers/docker/basics/move-docker-files/)  
<!-- [Перенос форума в контейнеры Docker (В работе)](/linux/servers/containers/docker/odba/) -->


<br/>

### Dockerfile - скрипт для создания контейнера автоматически

[здесь](/linux/servers/containers/docker/dockerfile/)  


<br/>

### Docker Clustering

[Docker Swarm](/linux/servers/containers/docker/clustering/swarm/)  

<br/>

### Примеры конфигов работы с Docker

[Docker for Web Developers (видеокурс)](https://bitbucket.org/sysadm-ru/docker-for-web-developers)


<br/>

### Хостовые операционные системы для docker контейнеров

[CoreOS (была куплена RedHat)](/linux/servers/containers/coreos/)

Rancher OS (Rancher Labs)

Atomic (RedHat)

___


<br/>

Еще вот здесь чувак пишет о s6-overlay. (Я хз пока что это такое):


    http://reangdblog.blogspot.com/2016/09/debian-ubuntu-docker.html


    Я использую s6-overlay, эта набор скриптов поверх s6, которые разрабатывались специально под docker. Я не буду пересказывать документацию, но вкратце опишу возможности: 

    - "Повесить" набор скриптов на старт и остановку контейнера.
    - Декларативно описать назначение прав на директории и файлы, вместо беспорядочных chmod и chown в разных местах.
    - Для каждого сервиса можно написать скрипт запуска под нужным пользователем, и скрипт, который будет выполняться при завершении контейнера.
    - Ну и конечно единообразное логирование каждого шага.


    https://github.com/just-containers/s6-overlay






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
