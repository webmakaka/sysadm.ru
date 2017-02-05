---
layout: page
title: Docker Swarm первый взгляд
permalink: /linux/containers/docker/swarm/old/
---

# Docker Swarm первый взгляд


https://docs.docker.com/engine/swarm/swarm-tutorial/


**Файлы для старта виртуальных машин с coreos**

https://github.com/sysadm-ru/Native-Docker-Clustering

<br/>

    $ vagrant up


<br/>

    $ vagrant ssh core-01
    $ vagrant ssh core-02
    $ vagrant ssh core-03


Имеем 3 виртуальные машины с IP адресами:

    core-01 - manager1 (10.0.11.5)
    core-02 - worker1  (10.0.12.5)
    core-03 - worker2  (10.0.13.5)

На всех:

    $ docker pull swarm

    $ docker run swarm -v
    swarm version 1.2.6 (`git rev-parse --short HEAD`)


<br/>

### core-01

    $ docker swarm init --advertise-addr <MANAGER-IP>

    $ docker swarm init --advertise-addr 10.0.11.5     
    Swarm initialized: current node (93q948x7yyah5qs5msbfhab7m) is now a manager.

    To add a worker to this swarm, run the following command:

        docker swarm join \
        --token SWMTKN-1-0t0d1z90g4jhp3zyi7jm4a17hs01ffoph4pnv88mavd9ypvg96-awkthc92qq9as8v5slmweyy5h \
        10.0.11.5:2377

    To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.


<br/>

### core-02 core-03

    # docker swarm join \
    --token SWMTKN-1-0t0d1z90g4jhp3zyi7jm4a17hs01ffoph4pnv88mavd9ypvg96-awkthc92qq9as8v5slmweyy5h \
    10.0.11.5:2377


<br/>

### core-01

     $ docker node ls
     ID                           HOSTNAME  STATUS  AVAILABILITY  MANAGER STATUS
     1zzst7ij9wk1pn7d2xu9hg5rx    core-03   Ready   Active        
     8cx6w1u5pv6jrvfigswznbnw9    core-02   Ready   Active        
     93q948x7yyah5qs5msbfhab7m *  core-01   Ready   Active        Leader


<br/>

### Create a service

    $ docker service create --replicas 1 --name helloworld alpine ping docker.com
    cm8hegviha1ip8kp6wq4nqa9o

    $ docker service ls
    ID            NAME        REPLICAS  IMAGE   COMMAND
    cm8hegviha1i  helloworld  1/1       alpine  ping docker.com

    $ docker service inspect --pretty helloworld
    ID:		cm8hegviha1ip8kp6wq4nqa9o
    Name:		helloworld
    Mode:		Replicated
     Replicas:	1
    Placement:
    UpdateConfig:
     Parallelism:	1
     On failure:	pause
    ContainerSpec:
     Image:		alpine
     Args:		ping docker.com
    Resources:



    $ docker service ps helloworld
    ID                         NAME          IMAGE   NODE     DESIRED STATE  CURRENT STATE          ERROR
    04nksqarbn1vx3e5sqmfl5h1d  helloworld.1  alpine  core-01  Running        Running 2 minutes ago  



    $ docker service scale helloworld=5


    $ docker service ps helloworld
    ID                         NAME          IMAGE   NODE     DESIRED STATE  CURRENT STATE           ERROR
    04nksqarbn1vx3e5sqmfl5h1d  helloworld.1  alpine  core-01  Running        Running 3 minutes ago   
    btb03l7l0oz3owvc2fonvm1gl  helloworld.2  alpine  core-02  Running        Running 11 seconds ago  
    eneg4b14vzm9q7el33zuby0vo  helloworld.3  alpine  core-03  Running        Running 11 seconds ago  
    dsvlis2xqceebdagl6qx4fpkz  helloworld.4  alpine  core-03  Running        Running 11 seconds ago  
    47edjkqjj4x35boeeo2ycnrre  helloworld.5  alpine  core-01  Running        Running 13 seconds ago  
