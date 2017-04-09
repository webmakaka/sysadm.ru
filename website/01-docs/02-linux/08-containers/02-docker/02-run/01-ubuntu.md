---
layout: page
title: Запуск Ubuntu Docker контейнера
permalink: /linux/containers/docker/run/
---



# Запуск Ubuntu Docker контейнера


    $ docker run -i -t ubuntu:latest /bin/bash

Если это будет webserver, то команда может быть следующей, чтобы можно было при обращении к порту 80 локалхоста подключаться к порту 8080 контейнера

    $ docker run -i -t -p 80:8080 ubuntu:latest  /bin/bash

    # apt-get update


// Для работы лично мне нужно поставить

    # apt-get install -y vim git wget curl iputils-ping net-tools

# Если предстоит что-то компилить

    # apt-get install -y build-essential


<br/>

### Стартовать контейнер для разработки

// Разрабатывать на хостовой, запускать в контейнере

    $ docker run -i -t --name dev -v /home/marley/go:/project ubuntu:latest  /bin/bash
