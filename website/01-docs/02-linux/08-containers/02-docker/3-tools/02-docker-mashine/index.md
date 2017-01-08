---
layout: page
title: Docker Machine
permalink: /linux/containers/docker/docker-machine/
---

# Docker Machine

https://docs.docker.com/machine/install-machine/


Я пока до конца не разобрался, для чего нужена Docker Machine.
Если все правильно понимаю, то для запуска virtualbox виртуалок. Docker выступает в качестве прослойки.

Наверное это актуально для WIndows а не для Linux пользователей.


    $ curl -L https://github.com/docker/machine/releases/download/v0.6.0/docker-machine-`uname -s`-`uname -m` > /usr/local/bin/docker-machine && \
    chmod +x /usr/local/bin/docker-machine


<br/>

    # docker-machine -v
    docker-machine version 0.6.0, build e27fb87


<br/>

    #  docker-machine create --driver virtualbox dev1

<br/>

    # docker-machine ls
    NAME   ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER    ERRORS
    dev1   -        virtualbox   Running   tcp://192.168.99.100:2376           v1.10.3   

<br/>

    # docker-machine env dev1
    export DOCKER_TLS_VERIFY="1"
    export DOCKER_HOST="tcp://192.168.99.100:2376"
    export DOCKER_CERT_PATH="/root/.docker/machine/machines/dev1"
    export DOCKER_MACHINE_NAME="dev1"
    # Run this command to configure your shell:
    # eval $(docker-machine env dev1)


<br/>

    # eval $(docker-machine env dev1)

<br/>

    # docker-machine ls
    NAME   ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER    ERRORS
    dev1   *        virtualbox   Running   tcp://192.168.99.100:2376           v1.10.3   

<br/>

    # docker run busybox echo hello world

<br/>

    # docker-machine stop dev1
