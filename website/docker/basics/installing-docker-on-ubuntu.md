---
layout: page
title: Installing Docker on Ubuntu
permalink: /docker/basics/installing-docker-on-ubuntu/
---


## 04 Installing and Updating Docker

Installing Docker on Ubuntu

Весия простая, устанавливается docker из пепо ubuntu.
___

    $ uname -a
Linux workstation 3.13.0-45-generic #74-Ubuntu SMP Tue Jan 13 19:36:28 UTC 2015 x86_64 x86_64 x86_64 GNU/Linux


    $ sudo su

    # apt-get update
    # apt-get install -y docker.io
    # service docker.io status

___

Весия простая, устанавливается docker из пепо docker.

    (Пока просто пишу без реального ввода команд в консоли)
    (Если кто проверит и исправит или подтвердит, что все ок, будет супер.)
    # wget -qO- https//get.docker.com/gpg | apt-key add -
    # echo deb http://get.docker.com/ubuntu docker main > /etc/apt/sources.list.d/docker.list

    # apt-get update
    # apt-get install lxc-docker

    # docker version



Я ранее поставил версию поновее, старую версию ставить не стал.

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


____


04 05 Granting Docker Control to Non-root Users


    $ sudo gpasswd -a <username> docker
    $ cat /etc/group
        docker:x:126:marley


    // Перелогиниваемся
    $logout

    $ docker run -it ubuntu /bin/bash
