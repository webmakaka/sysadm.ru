---
layout: page
title: Docker в Linux
permalink: /linux/containers/docker/
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

[Последняя версия Docker Community Edition] - на августе 2019 - 19.03.1  
https://docs.docker.com/release-notes/docker-ce/

[Подготовленные image]  
https://hub.docker.com/explore/

[Docker Registry (Network Storage For Docker Images)] (облачный сервис для хранения контейнеров)  
https://hub.docker.com  
https://quay.io

На hub.docker.com можно делать автоматически собираемые image. Для этого необходимо указать сервису проект с Dockerfile. При внесении изменений в проект, image собирается заново.

**Можно также создать свой Registry**


<br/><br/>

### Инсталляция Docker

[Инсталляция Docker](/linux/containers/docker/install/)

[Инсталляция Docker-Compose (для совместной работы контейнеров)](/linux/containers/docker/tools/docker-compose/)

[Пример запуска прилоения в Docker одной командой](/linux/containers/docker/run/)

<br/>

### Базовые вещи

[Имидж и контейнер, в чем собственно разница?](/linux/containers/docker/basics/images-and-containers/)

[Основные команды Docker](/linux/containers/docker/basics/basic-commands/)

<br/>

### Docker Tools

[Docker Machine (для запуска контейнеров в virtualbox, обычно в windows или mac)](/linux/containers/docker/docker-machine/)

<br/>

### Docker NetWorking (Не особо и нужно. Но возможность такая есть (или по крайней мере была в версии 1.3). Не пользуюсь этой возможностью)

https://docs.docker.com/engine/userguide/networking/

[Настройка моста для работы с Docker в Ubuntu](/linux/containers/docker/networking/ubuntu-bridge/)  
[Задание параметров сетевых интерфейсов docker в Ubuntu (IP, gateway, etc.)](/linux/containers/docker/networking/ubuntu-bridge/bridge-my-version/)

<br/>

### Docker Linking Containers

Лучше использовать <a href="/linux/containers/docker/tools/docker-compose/">docker-compose</a> для линковки контейнеров.
Для работы с docker-compose нужные версии docker >= 1.8.

[Пример линковки контейнеров для их совместной работы](/linux/containers/docker/linking-containers/manual-linking/)

<br/>

### Docker Работа с image

[Скопировать Docker Images на другой Host](/linux/containers/docker/basics/copying-images-to-other-hosts/)  
[Скопировать image на hub.docker.com и забрать image с него](/linux/containers/docker/basics/push-and-pull-docker-image-to-hub/)

<br/>

### Работа с официальными и не только контейнерами

[Пример запуска веб проекта в контейнерах Docker](https://github.com/marley-nodejs/Projects-in-Docker)

[Lamp Server](/linux/containers/docker/lamp/)

[Работа с официальным mysql Docker контейнером](/linux/containers/docker/official/containers/mysql/)

[YouTube: Quick Wordpress Setup With Docker](https://www.youtube.com/watch?v=pYhLEV-sRpY)

[docker-django](https://github.com/ruddra/docker-django)

[MongoDB + импорт данных](https://github.com/g0t4/docker-mongo-sample-datasets/tree/docker-registry)

<br/>

### Информация о запущенных контейнерах

[Получить информацию о запущенных Docker контейнерах c помощью sysdig](/linux/containers/docker/sysdig/)

<br/>

### Docker практические задачи

[Переместить файлы Docker](/linux/containers/docker/basics/move-docker-files/)

<!-- [Перенос форума в контейнеры Docker (В работе)](/linux/containers/docker/odba/) -->

<br/>

### Dockerfile - скрипт для создания контейнера автоматически

[здесь](/linux/containers/docker/dockerfile/)


<br/>

### Self-hosted Registry (Свой dockerhub)

[Self-hosted Registry](/linux/containers/docker/registry/)


<br/>

### Docker Clustering

[Docker Swarm](/linux/containers/docker/clustering/swarm/)

<br/>

### Примеры конфигов работы с Docker

[Docker for Web Developers (видеокурс)](https://bitbucket.org/sysadm-ru/docker-for-web-developers)

<br/>

### Хостовые операционные системы для docker контейнеров

[CoreOS (была куплена RedHat/IBM)](/linux/containers/coreos/)

Rancher OS (Rancher Labs)

Atomic (RedHat)

---

<br/>

Еще вот здесь чувак пишет о s6-overlay. (Я хз пока что это такое):

    http://reangdblog.blogspot.com/2016/09/debian-ubuntu-docker.html


    Я использую s6-overlay, эта набор скриптов поверх s6, которые разрабатывались специально под docker. Я не буду пересказывать документацию, но вкратце опишу возможности:

    - "Повесить" набор скриптов на старт и остановку контейнера.
    - Декларативно описать назначение прав на директории и файлы, вместо беспорядочных chmod и chown в разных местах.
    - Для каждого сервиса можно написать скрипт запуска под нужным пользователем, и скрипт, который будет выполняться при завершении контейнера.
    - Ну и конечно единообразное логирование каждого шага.


    https://github.com/just-containers/s6-overlay
