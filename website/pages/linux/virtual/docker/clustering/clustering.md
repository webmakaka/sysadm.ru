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

3. Get Swarm:

    go get -u github.com/docker/swarm
    
4. Update $PATH:
    
    export PATH=$PATH:~/go/bin
    
    
5. Virify install: 
    
    swarm --version
