---
layout: page
title: Инсталляция Docker в Ubuntu 14.04
permalink: /linux/virtual/docker/installation/ubuntu/
---



### Инсталляция

**Вариант 1: устанавливаем docker из репо docker (рекоменую)**

    # wget -qO- https://get.docker.com/gpg | apt-key add -
    # echo "deb http://get.docker.com/ubuntu docker main" >> /etc/apt/sources.list.d/docker.list

    # apt-get update
    # apt-get install -y lxc-docker

    # docker -v


**Вариант 2: устанавливаем docker из репо ubuntu. (не рекомендую)**

    $ sudo su -

    # apt-get update
    # apt-get install -y docker.io
    # service docker.io status

    $ docker -v

<br/>

### Настройка

**Предоставить пользователю права для работы с docker**

    $ sudo gpasswd -a <username> docker

в группе docker должен появиться этот пользователь  

    $ cat /etc/group
        docker:x:126:username

Перелогиниваемся, иначе не будет работать

    $ logout

Или даже можно сделать reboot.

<br/>


### Определяю каталог для хранения контейнеров и имиджей.

(Просто не хочу, хранить редко используемые docker файлы на системном, да еще и SSD диске)

    # mkdir -p /mnt/dsk0/docker
    # chown -R <username> /mnt/dsk0/docker

    # vi /etc/default/docker

    DOCKER_OPTS="-g /mnt/dsk0/docker"

<br/>

    # service docker restart

<br/>

    # ps auxwww | grep docker
    root      2476  0.0  0.1 274324 29896 ?        Ssl  10:10   0:00 /usr/bin/docker daemon -g /mnt/dsk0/docker
