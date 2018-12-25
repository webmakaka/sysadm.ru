---
layout: page
title: Инсталляция и Upgrade Docker в Ubuntu 18.04.1 bionic64
permalink: /linux/servers/containers/docker/installation/ubuntu/
---


# Инсталляция / Upgrade Docker в Ubuntu 18.04.1 bionic64

Делаю:  

25.12.2018


<br/>

### Инсталляция Docker версии 18.x

    # apt install -y \
        apt-transport-https \
        ca-certificates \
        curl \
        software-properties-common

<br/>

    # curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

<!-- 
    # apt-key fingerprint 0EBFCD88 -->
    

    # add-apt-repository \
    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) \
    stable"


    # apt update && apt install -y docker-ce

    # docker -v
    Docker version 18.09.0, build 4d60db4

<br/>

**Подробнее:**  
https://docs.docker.com/install/linux/docker-ce/ubuntu/

<br/>
**Бинарники docker лежат здесь:**  
https://download.docker.com/linux/ubuntu/dists/bionic/stable/binary-amd64/

<br/>

## Настройка 

<br/>

### Предоставить пользователю права для работы с docker


    # usermod -aG docker <username>

в группе docker должен появиться этот пользователь  

    # cat /etc/group | grep docker
        docker:x:126:username

Перелогиниваемся, иначе не будет работать

    $ logout

Или даже можно сделать reboot.

<br/>

### Изменить каталог по умолчанию для хранения контейнеров и имиджей

    # mkdir -p /mnt/dsk1/docker
    # chown -R <username> /mnt/dsk1/docker

    # vi /etc/default/docker

    DOCKER_OPTS="-g /mnt/dsk1/docker"

<br/>

    # service docker restart

<br/>

    # ps auxwww | grep docker
    root      2476  0.0  0.1 274324 29896 ?        Ssl  10:10   0:00 /usr/bin/docker daemon -g /mnt/dsk1/docker
