---
layout: page
title: Инсталляция Docker в CentOS
permalink: /linux/containers/docker/installation/centos/
---


# Инсталляция Docker в CentOS


### Инсталляция с помощью repo

    # yum update -y

<br/>

    # vi /etc/yum.repos.d/docker.repo

 <br/>

    [dockerrepo]
    name=Docker Repository
    baseurl=https://yum.dockerproject.org/repo/main/centos/$releasever/
    enabled=1
    gpgcheck=1
    gpgkey=https://yum.dockerproject.org/gpg


<br/>

    # yum install -y docker-engine

<br/>

    # chkconfig docker on

<br/>

    # service docker start

<br/>

    # groupadd docker

    # usermod -aG docker your_username

<br/>

    Log out and log back in.


<br/>

    $ docker run hello-world

<br/>


https://docs.docker.com/engine/installation/linux/centos/

<br/>
<br/>

### Вариант установки скриптом (рекомендуется)

    # wget https://get.docker.com/builds/Linux/x86_64/docker-latest -O /usr/bin/docker

    $ sudo chmod +x /usr/bin/docker


<br/>

### Вариант (не рекомендуется, т.к. установятся старые версии)

    # yum install -y docker
    # systemctl status docker.service

    # systemctl start docker.service
    # docker -v
    # docker version
    # docker info
