---
layout: page
title: Инсталляция и Upgrade Docker в Ubuntu 14.04
permalink: /linux/containers/docker/installation/ubuntu/
---


# Инсталляция / Upgrade Docker в Ubuntu 14.04


Похоже, они с каждым релизом меняют способ установки. 
Пятый раз переписываю!

Делаю:  

27.04.2018

на ubuntu/xenial64  
на ubuntu/bionic64 - пока не работает. Делаю на виртуалке.



<br/>

### Инсталляция Docker версии 18.03.0

    -- Удаляю текущую версию docker (если нужно)
    # apt-get remove -y docker docker-engine docker.io

    # apt-get update

    # apt-get install -y \
        linux-image-extra-$(uname -r) \
        linux-image-extra-virtual
        
    # apt-get install -y \
        apt-transport-https \
        ca-certificates \
        curl \
        software-properties-common
        
    # curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    
    # apt-key fingerprint 0EBFCD88
    
    # add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
   
   
   # apt-get update
   # apt-get install -y docker-ce

   # docker -v
   Docker version 18.03.1-ce, build 9ee9f40


Подробнее:  
https://docs.docker.com/install/linux/docker-ce/ubuntu/


<br/>

## Настройка 

<br/>

### Предоставить пользователю права для работы с docker

    $ sudo gpasswd -a <username> docker

в группе docker должен появиться этот пользователь  

    $ cat /etc/group
        docker:x:126:username

Перелогиниваемся, иначе не будет работать

    $ logout

Или даже можно сделать reboot.

<br/>

### Изменить каталог по умолчанию для хранения контейнеров и имиджей

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
