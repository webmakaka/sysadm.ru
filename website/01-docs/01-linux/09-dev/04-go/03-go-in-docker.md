---
layout: page
title: Запуск GO программ в контейнере Docker
permalink: /linux/dev/go/go-in-docker/
---

# Запуск GO программ в контейнере Docker



    $ cd /tmp/
    $ vi Dockerfile
    
    $ git clone https://bitbucket.org/marley-golang/hw1_tree/
    $ cd hw1_tree/

    $ docker build -t mailgo_hw1 .
    
<br/>
    
Можно заменить команду в Dockerfile на:
    
<br/>
    
    $ go run main.go .
