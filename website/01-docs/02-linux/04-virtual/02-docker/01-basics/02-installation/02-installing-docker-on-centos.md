---
layout: page
title: Инсталляция Docker в CentOS 7.X
permalink: /linux/virtual/docker/basics/installing-docker-on-centos/
---


### Вариант (рекомендуется)

    $ sudo wget https://get.docker.com/builds/Linux/x86_64/docker-latest -O /usr/bin/docker

    $ sudo chmod +x /usr/bin/docker


<br/>

### Вариант (не рекомендуется, т.к. установятся старые версии)

    # yum install -y docker
    # systemctl status docker.service

    # systemctl start docker.service
    # docker -v
    # docker version
    # docker info
