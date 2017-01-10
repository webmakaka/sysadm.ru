---
layout: page
title: Практический пример запуска приложения в docker под Windows
permalink: /windows/containers/docker/run-container/
---

# Практический пример запуска приложения в docker под Windows

Пусть это будет контейнер от проекта по обучению Angularjs 2 от поискового гиганта.


### !!! Чуть ли не 1GB скачается из интернета на диск.

По крайней мере, я вижу следующее:

    REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
    ng2-quickstart      latest              dd42c5d97b57        About an hour ago   934.4 MB


<br/>

     $ git clone https://github.com/angular/quickstart myApp
     Cloning into 'myApp'...
     remote: Counting objects: 1192, done.
     remote: Compressing objects: 100% (22/22), done.
     Receiving objects:  96% (1145/1192), 796.01 KiB | 677.0remote: Total 1192 (delta 8), reused 0 (delta 0), pack-reused 117Receiving objects: 100% (1192/1192), 1.63 MiB | 38.00 KiB/s, done.

     Resolving deltas: 100% (669/669), done.
     Checking connectivity... done.

<br/>

    $ cd myApp/

 <br/>

    $ docker build -t ng2-quickstart .
    $ docker run -it --rm -p 3000:3000 -p 3001:3001 ng2-quickstart


Коннектиться нужно не к localhost а к виртуальной машине virtualbox.


    $ docker-machine env default
    export DOCKER_TLS_VERIFY="1"
    export DOCKER_HOST="tcp://192.168.99.100:2376"
    export DOCKER_CERT_PATH="C:\Users\<Username>\.docker\machine\machines\default"
    export DOCKER_MACHINE_NAME="default"
    # Run this command to configure your shell:
    # eval $("C:\Program Files\Docker Toolbox\docker-machine.exe" env default)

<br/>

    $ docker-machine ip
    192.168.99.100

<!-- <br/>

    $ docker-machine start default
    $ docker-machine stop default

-->

<br/>

    $ docker-machine ls
    NAME      ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER    ERRORS
    default   -        virtualbox   Running   tcp://192.168.99.100:2376           v1.11.0

<br/>

    $ docker-machine status default
    Running

<br/>

<!--



<br/>

-->


Подключаюсь браузером:  
http://192.168.99.100:3000/

Все ОК.

PS. у меня с первого раза не заработало. ХЗ почему.

Выполнял после этого команду из env

    $ eval $("C:\Program Files\Docker Toolbox\docker-machine.exe" env default)

И перестартовывал контейнеры.
