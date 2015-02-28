---
layout: page
title: Native Docker Clustering with swarm
permalink: /linux/virtual/docker/clustering/ubuntu/
---

Нужна версия docker >= 1,4

1. install GO:

    sudo apt-get install -y golang
    
2. Set $GOPATH:

    export GOPATH=~/go  
    mkdir ~/go

3. Get Swarm:

    go get -u github.com/docker/swarm
    
4. Update $PATH:
    
    export PATH=$PATH:~/go/bin
    
    
5. Virify install: 
    
    swarm --version

___

## Building a Swarm Cluster

1. Create cluster (создется на одном хосте):

    swarm create

    
2. Join machines (на других хостах добавляем машины в пул): 

   swarm join token://<token> --addr <ip>:<port>
   
Порт по умолчанию 2375

3. Virify joins: 

    swarm list token://<token>
    
4. Start Swarm:

    swarm manage token://<token> -H <ip>:<port>

Для примера:

    swarm manage --help
    swarm manage token://<token> -H 0.0.0.0:4243 &
   
    ps -ef | grep swarm


5. Virify cluster:

    docker info
    
    

