---
layout: page
title: Playing Around with Our First Docker Container
permalink: /docker/installing-and-updating-docker/playing-around-with-our-first-docker-dontainer/
---


## 04 Installing and Updating Docker

04 07 Playing Around with Our First Docker Container

ubuntu

    $ docker run -it centos /bin/bash

container

    # ps -elf
    # cat /etc/hosts

    # ip a
    # uname -a

    # car /etc/redhat-release

    # yum check-update
    # yum install -y vim

    # создали файл "/tmp/testfile.txt" с текстом "Yo! This is a test file!".
    # vim /tmp/testfile.txt

    # exit

ubuntu

    // показать активные контейнеры
    $ docker ps

    // показать все контейнеры в том числе остановленные
    $ docker ps -a

    $ ls -l /var/lib/docker/aufs/diff/

    Нашли ранее созданный контейнер и в нем каталог tmp с созданным ранее файлом в контейнере.
    $ cat /var/lib/docker/aufs/diff/<container_id>/tmp/testfile.txt


    docker start <container_id>
    docker attach <container_id><container_id>
