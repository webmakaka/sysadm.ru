---
layout: page
title: Configuring Docker to Communicate Over the Network Users
permalink: /docker/installing-and-updating-docker/configuring-docker-to-communicate-over-the-network/
---


## 04 Installing and Updating Docker

04 06 Configuring Docker to Communicate Over the Network Users

Ubuntu

    # netstat -tcp
    # service docker stop
    # docker -H 192.168.1.11:2375 -d &
    # netstat -tcp

Centos

(клиентом цепляемся к удаленному docker на ubuntu)

    export DOCKER_HOST="tcp://192.168.1.11:2375"
    docker version

(возвращаемся к локальному docker)

    export DOCKER_HOST=
    docker version
