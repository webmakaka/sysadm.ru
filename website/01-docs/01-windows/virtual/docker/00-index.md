---
layout: page
title: Docker в Windows
permalink: /windows/virtual/docker/
---

Offtopic:  
[Docker в Linux](/linux/virtual/docker/)


Не вижу (пока) каких-либо причин использовать docker в windows, разве, что как у меня на работе компьютер с windows. Да еще и 10.

Я нашел видеокурс, который показывает, как работать с Docker в том числе и с Windows. Он называется.
[PLURALSIGHT.COM, DAN WAHLIN] DOCKER FOR WEB DEVELOPERS [2016, ENG]
Ищите на трекерах. Например, я брал на даркосе.


<br/>

### Поехали ставить Docker в Windows

Get Started with Docker for Windows  
https://docs.docker.com/windows/


1) Устанавливаю Docker Toolbox
https://www.docker.com/products/docker-toolbox

2) Запускаю через консоль Docker QuickStart Terminal

3) Проверяю (от нечего делать):


    $ docker pull hello-world
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


<br/>

### Ну и практический пример, чтобы не было совсем сучно.

Пусть это будет контейнер от проекта по обучению Angularjs 2.

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


К сожалению, лично у меня пока не заработало. (А все почему? потому, что не стал читаь мануалы!) Имею следующий output.

    npm info it worked if it ends with ok
    npm info using npm@3.8.3
    npm info using node@v5.10.1
    npm info lifecycle angular2-quickstart@1.0.0~prestart: angular2-quickstart@1.0.0
    npm info lifecycle angular2-quickstart@1.0.0~start: angular2-quickstart@1.0.0

    > angular2-quickstart@1.0.0 start /quickstart
    > tsc && concurrently "tsc -w" "lite-server"

    [1] Did not detect a `bs-config.json` or `bs-config.js` override file. Using lite-server defaults...
    [1] ** browser-sync config **
    [1] { injectChanges: false,
    [1]   files: [ './**/*.{html,htm,css,js}' ],
    [1]   watchOptions: { ignored: 'node_modules' },
    [1]   server: { baseDir: './', middleware: [ [Function], [Function] ] } }
    [1] [BS] Access URLs:
    [1]  -----------------------------------
    [1]        Local: http://localhost:3000
    [1]     External: http://172.17.0.2:3000
    [1]  -----------------------------------
    [1]           UI: http://localhost:3001
    [1]  UI External: http://172.17.0.2:3001
    [1]  -----------------------------------
    [1] [BS] Serving files from: ./
    [1] [BS] Watching files...
    [0] 9:51:33 AM - Compilation complete. Watching for file changes.


При попытке подключиться по указанному адресу, ничего не происходит.
Впрочем я зашел в сам контейнер, стартовал ноду, и подключился с помощью curl к вебсерверу.

Так, что какая-то проблема на стороне Winwos 10 или в скрипте явно прописаны параметры доступа к приложению.
Это у меня пока такие мысли.
