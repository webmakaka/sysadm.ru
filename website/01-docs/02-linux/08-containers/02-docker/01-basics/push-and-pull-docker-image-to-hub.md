---
layout: page
title: Отправить docker image на hub.docker.com
permalink: /linux/containers/docker/basics/push-and-pull-docker-image-to-hub/
---



# Отправить docker image на hub.docker.com

Создали репо на hub.docker.com

container_id и container_name как и для image в данном случае одно и тоже.


Если нужно сделать из контейнера image, сначала нужно выполнить эту команду.

    $ docker commit <container_name> <image_name>

Или даже лучше сразу:

    $ docker commit <container_name> <your_docker_hub_login>/<image_name>:<image_version>

// При необходимости, можно поменять название image

    $ docker tag <image_name> <your_docker_hub_login>/<image_name>:<image_version>


<br/>


### Отправка image на docker-hub

    $ docker login
    $ docker push <your_docker_hub_login>/<image_name>


<br/>

### Забрать image с ренее созданного репо.

    $ doceker pull <your_docker_hub_login>/<image_name>


<br/>

### Конкретный пример. Делал совсем недавно (Docker version 1.9.1)


    $ docker commit nginx_server marley/nginx_server:1

    nginx_server - имя моего контейнера
    marley/nginx_server:1 - создать image со следующим именем

    $ docker images
    REPOSITORY               TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
    marley/nginx_server      1                   5a6aaa885cf2        19 minutes ago      395.3 MB

    $ docker login
    $ docker push marley/nginx_server:1


<br/>

Забрать теперь можно командой:

    $ docker pull marley/nginx_server
