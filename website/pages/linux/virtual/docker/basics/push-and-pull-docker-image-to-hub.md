---
layout: page
title: Отправить docker image на hub.docker.com
permalink: /linux/virtual/docker/basics/push-and-pull-docker-image-to-hub/
---


https://hub.docker.com  


Создали репо на сайте  

    docker images
    docker tag <image_id> <your_login>/helloworld:1.0
    doceker push <your_login>/helloworld:1.0

==========

Забрать созданный image  

    doceker pull <your_login>/helloworld:1.0
