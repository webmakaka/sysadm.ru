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

1. Create cluster:

    swarm create
    
2. Join machines: 

   swarm join token://<token> --addr <ip>:<port>

3. Virify joins: 

    swarm list token://<token>
    
4. Start Swarm: swarm manage

    token://<token> -H <ip>:<port>

5. Virify cluster:

    docker info
    
    

