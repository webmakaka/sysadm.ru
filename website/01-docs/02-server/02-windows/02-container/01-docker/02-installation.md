---
layout: page
title: Инсталляция Docker в Windows
description: Инсталляция Docker в Windows
keywords: Инсталляция Docker в Windows
permalink: /server/windows/container/docker/installation/
---

# Инсталляция Docker в Windows

Get Started with Docker for Windows  
https://docs.docker.com/windows/

1. Устанавливаю Docker Toolbox  
   https://www.docker.com/products/docker-toolbox

2. Запускаю через консоль для поиска: Docker QuickStart Terminal

3. Проверяю (от нечего делать):

   \$ docker pull hello-world
   Using default tag: latest
   latest: Pulling from library/hello-world
   03f4658f8b78: Pull complete
   a3ed95caeb02: Pull complete
   Digest: sha256:8be990ef2aeb16dbcb9271ddfe2610fa6658d13f6dfb8bc72074cc1ca36966a7
   Status: Downloaded newer image for hello-world:latest

<br/>

    $ docker run hello-world

    Hello from Docker.
    This message shows that your installation appears to be working correctly.

    To generate this message, Docker took the following steps:
     1. The Docker client contacted the Docker daemon.
     2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
     3. The Docker daemon created a new container from that image which runs the
        executable that produces the output you are currently reading.
     4. The Docker daemon streamed that output to the Docker client, which sent it
        to your terminal.

    To try something more ambitious, you can run an Ubuntu container with:
     $ docker run -it ubuntu bash

    Share images, automate workflows, and more with a free Docker Hub account:
     https://hub.docker.com

    For more examples and ideas, visit:
     https://docs.docker.com/userguide/
