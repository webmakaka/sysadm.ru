---
layout: page
title: Installing Docker on Ubuntu
permalink: /docker/basics/installing-docker-on-ubuntu/
---


## Инсталляция Docker в Ubuntu


Вариант 1: устанавливаем docker из репо ubuntu.
___

    $ sudo su

    # apt-get update
    # apt-get install -y docker.io
    # service docker.io status

    $ docker -v

___

Вариант 2: устанавливаем docker из репо docker.

    (Пока просто пишу без реального ввода команд в консоли)
    (Если кто проверит и исправит или подтвердит, что все ок, будет супер.)
    # wget -qO- https//get.docker.com/gpg | apt-key add -
    # echo deb http://get.docker.com/ubuntu docker main > /etc/apt/sources.list.d/docker.list

    # apt-get update
    # apt-get install lxc-docker

    # docker version

===

Предоставить пользователю возможность работы с docker

    $ sudo gpasswd -a <username> docker

// в группе должен появиться этот пользователь  

    $ cat /etc/group
        docker:x:126:username

// Перелогиниваемся  
    $logout
