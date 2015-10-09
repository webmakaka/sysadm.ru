---
layout: page
title: Отправить docker image на hub.docker.com
permalink: /linux/virtual/docker/basics/push-and-pull-docker-image-to-hub/
---


https://hub.docker.com  


Создали репо на сайте

Если нужно сделать из контейнера image, сначала нужно выполнить эту команду.

    docker commit <container_id> <image_tag>

    docker images

    docker tag <image_id> <your_docker_hub_login>/<image_name>

или

    docker tag <image_tag> <your_docker_hub_login>/<image_name>

или даже лучше с указанием версии

    docker tag <image_id> <your_docker_hub_login>/<image_name>:<image_version>

и сосбвенно отправка на docker-hub

    doceker push <your_docker_hub_login>/<image_name>

==========

Забрать image с ренее созданного репо.

    doceker pull <your_docker_hub_login>/<image_name>


У меня получился контейнер на 5.812 GB :))
