---
layout: page
title: Инсталляция и Upgrade Docker в Ubuntu 20.04.1
description: Инсталляция и Upgrade Docker в Ubuntu 20.04.1
keywords: devops, docker, docker-compose, инсталляция, linux, ubuntu, bash скрипт
permalink: /devops/containers/docker/setup/ubuntu/
---

# Инсталляция / Upgrade Docker в Ubuntu 20.04.1

Делаю:  
16.01.2021

<br/>

### Инсталляция Docker версии 19.x

```
$ mkdir ~/tmp
$ cd ~/tmp

<br/>

$ vi install-docker-and-docker-compose.sh

```

<br/>

```
# Install Docker

sudo apt-get update
sudo apt-get install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
sudo apt-get update
sudo apt-get install -y docker-ce


# Install Docker-Compose

LATEST_VERSION=$(curl --silent "https://api.github.com/repos/docker/compose/releases/latest" | grep '"tag_name"' | sed -E 's/.*"([^"]+)".*/\1/')

sudo curl -L "https://github.com/docker/compose/releases/download/${LATEST_VERSION}/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

<br/>

    $ chmod +x ./install-docker-and-docker-compose.sh

    $ sudo ./install-docker-and-docker-compose.sh

<br/>

```
$ docker -v
Docker version 20.10.2, build 2291f61

$ docker-compose --version
docker-compose version 1.27.4, build 40524192
```

<br/>

### Предоставить пользователю права для работы с docker

    $ sudo usermod -aG docker <username>

в группе docker должен появиться этот пользователь

    $ cat /etc/group | grep docker
        docker:x:126:username

Перелогиниваемся, иначе не будет работать

    $ logout

Или даже можно сделать reboot.

<br/>

### (При необходимости!) Изменить каталог по умолчанию для хранения контейнеров и имиджей

<br/>

    # mkdir -p /mnt/dsk1/docker
    # chown -R <username> /mnt/dsk1/docker

    # vi /etc/default/docker

    DOCKER_OPTS="-g /mnt/dsk1/docker"

<br/>

    # service docker restart

<br/>

    # ps auxwww | grep docker
    root      2476  0.0  0.1 274324 29896 ?        Ssl  10:10   0:00 /usr/bin/docker daemon -g /mnt/dsk1/docker
