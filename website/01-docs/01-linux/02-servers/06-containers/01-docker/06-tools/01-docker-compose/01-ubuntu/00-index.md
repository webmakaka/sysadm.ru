---
layout: page
title: Инсталляция Docker-Compose в Ubuntu 18.04.1
permalink: /linux/servers/containers/docker/tools/docker-compose/install/ubuntu/
---

# Инсталляция Docker-Compose в Ubuntu 18.04.1

Docker-Compose нужен, чтобы организовать совместную работу нескольких контейнеров)

**Делаю:**  
10.05.2019

Смотрю последний релиз (сегодня это 1.24.0):
https://github.com/docker/compose/releases

    # curl -L https://github.com/docker/compose/releases/download/1.24.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose

    # chmod +x /usr/local/bin/docker-compose

    $ docker-compose --version
    docker-compose version 1.24.0, build 0aa59064
