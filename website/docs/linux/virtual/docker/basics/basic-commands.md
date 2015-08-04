---
layout: page
title: Основные команды
permalink: /linux/virtual/docker/basics/basic-commands/
---


Подготовленные image  
https://registry.hub.docker.com/  

Создать свой репо для контейнеров (1 приватный бесплатно + нет ограничений для публичных контейнеров)  
https://hub.docker.com  

___

$ docker -v  

    Docker version 1.4.1, build 5bc2ff8

$ docker version  

    Client version: 1.4.1
    Client API version: 1.16
    Go version (client): go1.3.3
    Git commit (client): 5bc2ff8
    OS/Arch (client): linux/amd64
    Server version: 1.4.1
    Server API version: 1.16
    Go version (server): go1.3.3
    Git commit (server): 5bc2ff8


$ docker info

    Containers: 1
    Images: 125
    Storage Driver: aufs
     Root Dir: /var/lib/docker/aufs
     Dirs: 127
    Execution Driver: native-0.2
    Kernel Version: 3.13.0-45-generic
    Operating System: Ubuntu 14.04.1 LTS
    CPUs: 8
    Total Memory: 23.54 GiB
    Name: workstation
    ID: 7OQQ:XF7B:TC2S:4UWQ:R4JB:VC3K:Q6TA:5ROP:SUAC:WLJ5:Y7YA:GFAG
    WARNING: No swap limit support

___



// взять из репо последнюю версию федоры  

    docker pull fedora

// взять все версии федоры  

    docker pull -a fedora

// получить список скачанных images  

    docker images
    docker images --tree
    docker images fedora


// Запустить shell в контейнере  

    docker run -it centos:centos6 /bin/bash


// Контенеры и имиджи хранятся здесь  

cat /var/lib/docker/aufs/diff/<container_id>

ls -l /var/lib/docker/containers  
ls -l /var/lib/docker/containers | wc -l

___

// показать активные контейнеры

    $ docker ps


// показать все контейнеры в том числе остановленные  

    $ docker ps -a


// Последний стартовавший контейнер.  

    docker ps -l


// Старт стоп

    docker start <container_id>
    docker stop <container_id>
    docker restart <container_id>

// Отключиться от контейнера docker без его остановки:
    CTRL + P + Q

// Подключиться  

    docker attach <container_id>

// Подключиться еще одной сессией к контейнеру

    docker exec -it <container_id> bash

___


    docker top <container_id>
    docker inspect <container_id>
    docker logs <container_id>

---

// Показать какие порты локальной машины соответствуют портам контейнера

    $ docker port <container_id>

Пример

    $ docker port my_container  

    1337/tcp -> 0.0.0.0:1337
    3000/tcp -> 0.0.0.0:3000
    8080/tcp -> 0.0.0.0:80
    9000/tcp -> 0.0.0.0:9000



// узнать IP Контейнера Docker

    docker inspect --format='{{.NetworkSettings.IPAddress}}' containerId


<br/>

### Остановка и удаление

// Удалить контейнер

    docker rm  <container_id>
    docker rm -f <container_id>


// stop all Docker containers:  

    # docker stop $(docker ps -a -q)

// remove all Docker containers:  

    # docker rm $(docker ps -a -q)

// remove all Docker images:  

    # docker rmi $(docker images -q)
