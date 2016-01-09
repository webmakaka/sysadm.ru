---
layout: page
title: Native Docker Clustering with swarm
permalink: /linux/virtual/docker/clustering/ubuntu/
---


Official Repository:  
https://hub.docker.com/_/swarm/


По материлам видеокурса.


[pluralsight] First Look: Native Docker Clustering [2015, ENG]

Как-то я не до конца разобрал.



Нужна версия docker >= 1,4

1.install GO:

    sudo apt-get install -y golang

2.Set $GOPATH:

    export GOPATH=~/go  
    mkdir ~/go

3.Get Swarm:

    go get -u github.com/docker/swarm

4.Update $PATH:

    export PATH=$PATH:~/go/bin


5.Virify install:

    swarm --version


<br/>

## Building a Swarm Cluster

1.Create cluster (создется на одном хосте):

    swarm create


2.Join machines (на других хостах добавляем машины в пул):

   swarm join token://<token> --addr <ip>:<port>

Порт по умолчанию 2375

3.Virify joins:

    swarm list token://<token>

4.Start Swarm:

    swarm manage token://<token> -H <ip>:<port>

Для примера:

    swarm manage --help
    swarm manage token://<token> -H 0.0.0.0:4243 &

    ps -ef | grep swarm


5.Virify cluster:

    docker info


___



    export DOCKER_HOST=192.168.56.50:4243
    docker info
    docker run --name c1 -d nginx
    docker ps
    http://192.168.56.50:4242/v1.16/contaners/json
    http://192.168.56.50:2375/v1.16/contaners/json

___

**strategy random**

убили swarm процесс


    swarm manage token://<token> -H 0.0.0.0:4243 --strategy random &

стартуем 20 контейнеров. Они должны разместить произвольно на хостах.

    for i in {1..20}; do /bin/bash -c "docker run -d nginx"; done

удаляем

    docker rm -f $(docker ps -aq)


___

**strategy binpacking** (default) (в зависимости от памяти хост машины)

убили swarm процесс


    swarm manage token://<token> -H 0.0.0.0:4243 --strategy binpacking &


___

**Affinity filter**

    docker run -d --name c1 -e constraint:node==three nginx
    docker run -d --name c2 -e constraint:node==two nginx

    docker run -d --name c4 -e affinity:container==c1 nginx
    docker run -d --name c5 -e affinity:container!=c1 nginx


___

**Standard Constraints**


    docker -H two:2375 info

    docker run -d --name c1 -e constraint:operatingsystem==Deb* nginx
    docker run -d --name c2 -e constraint:operatingsystem==fedora* nginx (Ошибка!)


___

**Custom Constraints**

two:


    vi /etc/default/docker

    DOCKER_OPTS="-H 192.168.56.35:2375 -H unix:///var/run/docker.sock --label zone=dmz --label site=london"

    service docker restart
    docker -H two:2375 info
    (появились label)


three:

    vi /etc/default/docker

    DOCKER_OPTS="-H 192.168.56.185:2375 -H unix:///var/run/docker.sock --label zone=prod --label site=london"
    service docker restart
    docker -H three:2375 info


two:

    docker run -d --name londprod1 -e constraint:site==london -e constraint:zone==prod nginx
    docker ps

    docker run -d --name londprod2 -e constraint:site==london -e constraint:zone!=prod nginx
    docker ps


___

**Resourse Constraints**


    docker run -d -p 8080:80 nginx
    docker run -d -p 8080:80 nginx
    docker run -d -p 8080:80 nginx


на каждом из хостов создаст по контейнеру.
Если попытаться выполнить команду 4-й раз получим ошибку, т.к. ресурсов больше нет.
