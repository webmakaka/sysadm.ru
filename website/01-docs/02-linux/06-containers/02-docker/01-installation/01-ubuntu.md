---
layout: page
title: Инсталляция Docker в Ubuntu 14.04
permalink: /linux/containers/docker/installation/ubuntu/
---



# Инсталляция Docker в Ubuntu 14.04


**Вариант 1 устанавливаем docker из репо docker (рекоменую)**  

    $ sudo su -

    # apt-get update
    # apt-get install -y apt-transport-https ca-certificates
    # apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D

    # echo "deb https://apt.dockerproject.org/repo ubuntu-xenial main" | sudo tee /etc/apt/sources.list.d/docker.list

or

    # echo "deb https://apt.dockerproject.org/repo ubuntu-trusty main" | sudo tee /etc/apt/sources.list.d/docker.list

or

    # echo "deb https://apt.dockerproject.org/repo ubuntu-precise main" | sudo tee /etc/apt/sources.list.d/docker.list

<br/>

    # apt-get update
    # apt-cache policy docker-engine

If you are installing docker engine on Ubuntu Xenial, Wily or Trusty, it’s recommended to install the linux-image-extra- kernel packages to allow using aufs

    # apt-get install -y linux-image-extra-$(uname -r) linux-image-extra-virtual

    # apt-get install -y docker-engine

<br/>

    $ docker -v
    Docker version 1.12.3, build 6b644ec



<!-- <br/>

**Вариант 2: устанавливаем docker из репо docker (рекоменую)**

    # wget -qO- https://get.docker.com/gpg | apt-key add -
    # echo "deb http://get.docker.com/ubuntu docker main" >> /etc/apt/sources.list.d/docker.list

    # apt-get update
    # apt-get install -y lxc-docker

    # docker -v



<br/>

**Вариант 3: устанавливаем docker из репо ubuntu. (не рекомендую)**

    $ sudo su -

    # apt-get update
    # apt-get install -y docker.io
    # service docker.io status

    $ docker -v -->


<br/>

**Вариант 2: Из исходников, что лежат на github (я не разобрался и у меня не заработало!!!)**


    $ sudo apt-get install make git
    $ cd /tmp/
    $ git clone --depth=1 https://github.com/docker/docker.git

    $ cd docker/

    $ sudo make build
    $ sudo make binary


    $ ./bundles/1.14.0-dev/binary-client/docker --version
    Docker version 1.14.0-dev, build 001cbc4

    $ sudo ./bundles/1.14.0-dev/binary-daemon/dockerd --version
    Docker version 1.14.0-dev, build 001cbc4


    # rm /usr/bin/docker

    # ln -s  /tmp/docker/bundles/1.12.4-rc1/binary-client/docker-1.12.4-rc1 /usr/bin/docker
    # ln -s  /tmp/docker/bundles/1.12.4-rc1/binary-daemon/dockerd-1.12.4-rc1 /usr/bin/dockerd

    # docker daemon

    Ошибка!!!

Мне пока не горит. Нет желания тратить на исследование свое личное время. Если кто подскажет как сделать или я сам разберусь, напишу.



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

    # mkdir -p /mnt/dsk1/docker
    # chown -R <username> /mnt/dsk1/docker

    # vi /etc/default/docker

    DOCKER_OPTS="-g /mnt/dsk1/docker"

<br/>

    # service docker restart

<br/>

    # ps auxwww | grep docker
    root      2476  0.0  0.1 274324 29896 ?        Ssl  10:10   0:00 /usr/bin/docker daemon -g /mnt/dsk1/docker


<br/>
<br/>

https://docs.docker.com/engine/installation/linux/ubuntulinux/


Из исхдных кодов:  
http://tristan.lt/blog/docker-4-build-docker-from-sources/  
http://oyvindsk.com/writing/docker-build-from-source  
