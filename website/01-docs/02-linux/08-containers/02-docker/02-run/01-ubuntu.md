---
layout: page
title: Запуск Ubuntu Docker контейнера
permalink: /linux/containers/docker/run/
---



# Запуск Ubuntu Docker контейнера


    $ docker run -i -t ubuntu:latest /bin/bash
    # apt-get update


// Для работы нужно что-то поставить

    # apt-get install -y git wget curl iputils-ping


<br/>

### Стартовать контейнер для разработки

// Разрабатывать на хостовой, запускать в контейнере

    $ docker run -i -t --name dev -v /home/marley/go:/project ubuntu:latest  /bin/bash
