---
layout: page
title: Копируем Images на другой Host
permalink: /linux/virtual/docker/basics/copying-images-to-other-hosts/
---


## Копируем Images на другой Host


Работаем на Ubuntu:

    docker run ubuntu /bin/bash -c "echo 'cool content' > /tmp/cool-file"

    docker ps -a

    docker commit <container_id> fridge

    docker images
    docker images --tree

    dcoker history fridge

    docker save -o /tmp/fridge.tar fridge

    ls -lh /tmp/fridge.tar


скопировали на centos

    tar -tf /tmp/fridge.tar

    docker load -i /tmp/fridge.tar

    docker images

    docker run -it fridge /bin/bash

    cat /tmp/cool-file
