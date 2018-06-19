---
layout: page
title: Инсталляция Docker-Compose в Ubuntu 14.04 организовать совместную работу нескольких контейнеров)
permalink: /linux/servers/containers/docker/toosl/docker-compose/installation/
---

# Инсталляция Docker-Compose в Ubuntu 14.04 организовать совместную работу нескольких контейнеров)


Делаю:  
03.04.2018

Смотрю последний релиз (сегодня это 1.20.1):
https://github.com/docker/machine/releases/

    # curl -L https://github.com/docker/compose/releases/download/1.20.1/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose

    # chmod +x /usr/local/bin/docker-compose

    $ docker-compose --version
    docker-compose version 1.20.1, build 5d8c71b
