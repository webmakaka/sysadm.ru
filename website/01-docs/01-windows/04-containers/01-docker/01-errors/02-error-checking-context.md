---
layout: page
title: Ошибки при работе с Docker в Windows - Error checking context
permalink: /windows/containers/docker/errors/error-checking-context/
---

# Ошибки при работе с Docker в Windows -  Error checking context: 'can't stat '\\?\C:\Users\UserName\AppData\Local\Application Data''.

Создаю контейнер из Dockerfile с помощью windows консоли docker - Docker Quick Start Terminal.


    $ pwd
    /c/Users/UserName

**Ошибка:**

    $ docker build -t <container_name> .
    Error checking context: 'can't stat '\\?\C:\Users\UserName\AppData\Local\Application Data''.

**Решение:**

    $ mkdir 1
    $ mv Dockerfile ./1
    $ cd 1/
    $ docker build -t <container_name> .

<br/>

    $ docker build -t ubuntu .
    Sending build context to Docker daemon  2.56 kB
    Step 1 : FROM ubuntu:14.04
     ---> 90d5884b1ee0
    Step 2 : MAINTAINER marley (www.marley.org)
     ---> Running in 064967d2ca2d
     ---> 95b0b43a9f95
    Removing intermediate container 064967d2ca2d
    Step 3 : RUN apt-get update
     ---> Running in 73eb1d009e7a
     *****
