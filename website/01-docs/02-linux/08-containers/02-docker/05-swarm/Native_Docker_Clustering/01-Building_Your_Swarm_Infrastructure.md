---
layout: page
title: Native Docker Clustering
permalink: /linux/containers/docker/swarm/Native_Docker_Clustering/Building_Your_Swarm_Infrastructure/
---

# Docker Swarm: Native Docker Clustering [2016, ENG] > Module 4: Building your Swarm Infrastructure


<br/>

    $ vagrant up

<br/>

    $ vagrant ssh core-01

    $ docker run --restart=unless-stopped -d -h consul1 --name consul1 -v /mnt:/data \
    -p 10.0.1.5:8300:8300 \
    -p 10.0.1.5:8301:8301 \
    -p 10.0.1.5:8301:8301/udp \
    -p 10.0.1.5:8302:8302 \
    -p 10.0.1.5:8302:8302/udp \
    -p 10.0.1.5:8400:8400 \
    -p 10.0.1.5:8500:8500 \
    -p 172.17.0.1:53:53/udp \
    progrium/consul -server -advertise 10.0.1.5 -bootstrap-expect 3


<br/>


    $ vagrant ssh core-02

    $ docker run --restart=unless-stopped -d -h consul2 --name consul2 -v /mnt:/data  \
    -p 10.0.2.5:8300:8300 \
    -p 10.0.2.5:8301:8301 \
    -p 10.0.2.5:8301:8301/udp \
    -p 10.0.2.5:8302:8302 \
    -p 10.0.2.5:8302:8302/udp \
    -p 10.0.2.5:8400:8400 \
    -p 10.0.2.5:8500:8500 \
    -p 172.17.0.1:53:53/udp \
    progrium/consul -server -advertise 10.0.2.5 -join 10.0.1.5


<br/>


    $ vagrant ssh core-03

    $ docker run --restart=unless-stopped -d -h consul3 --name consul3 -v /mnt:/data  \
    -p 10.0.3.5:8300:8300 \
    -p 10.0.3.5:8301:8301 \
    -p 10.0.3.5:8301:8301/udp \
    -p 10.0.3.5:8302:8302 \
    -p 10.0.3.5:8302:8302/udp \
    -p 10.0.3.5:8400:8400 \
    -p 10.0.3.5:8500:8500 \
    -p 172.17.0.1:53:53/udp \
    progrium/consul -server -advertise 10.0.3.5 -join 10.0.1.5

<br/>
<br/>

    $ docker exec -it consul1 bash
    bash-4.3# consul members                                                       
    Node     Address         Status  Type    Build  Protocol  DC
    consul1  10.0.2.15:8301  alive   server  0.5.2  2         dc1


<br/>
<br/>


**Команды из прилагаемого архива:**


    ########################################################################################################################################################
    ####### The following commands are for Module 4: Building your Swarm Infrastructure
    #######
    ####### Remember to substitute hostnames and IPs etc etc for the appropriate values in your environment
    ########################################################################################################################################################

    ####### CONSUL BUILD COMMANDS

    NODE1
    	docker run --restart=unless-stopped -d -h consul1 --name consul1 -v /mnt:/data \
        -p 10.0.1.5:8300:8300 \
        -p 10.0.1.5:8301:8301 \
        -p 10.0.1.5:8301:8301/udp \
        -p 10.0.1.5:8302:8302 \
        -p 10.0.1.5:8302:8302/udp \
        -p 10.0.1.5:8400:8400 \
        -p 10.0.1.5:8500:8500 \
        -p 172.17.0.1:53:53/udp \
        progrium/consul -server -advertise 10.0.1.5 -bootstrap-expect 3

    NODE2
    	docker run --restart=unless-stopped -d -h consul2 --name consul2 -v /mnt:/data  \
        -p 10.0.2.5:8300:8300 \
        -p 10.0.2.5:8301:8301 \
        -p 10.0.2.5:8301:8301/udp \
        -p 10.0.2.5:8302:8302 \
        -p 10.0.2.5:8302:8302/udp \
        -p 10.0.2.5:8400:8400 \
        -p 10.0.2.5:8500:8500 \
        -p 172.17.0.1:53:53/udp \
        progrium/consul -server -advertise 10.0.2.5 -join 10.0.1.5

    NODE3
    	docker run --restart=unless-stopped -d -h consul3 --name consul3 -v /mnt:/data  \
        -p 10.0.3.5:8300:8300 \
        -p 10.0.3.5:8301:8301 \
        -p 10.0.3.5:8301:8301/udp \
        -p 10.0.3.5:8302:8302 \
        -p 10.0.3.5:8302:8302/udp \
        -p 10.0.3.5:8400:8400 \
        -p 10.0.3.5:8500:8500 \
        -p 172.17.0.1:53:53/udp \
        progrium/consul -server -advertise 10.0.3.5 -join 10.0.1.5

    ####### Perform the following from any of the consul server nodes to verify the status of Consul
    	docker exec -it consul<1/2/3> bash
    	consul members


    ####### CONSUL CLIENT BUILDS ON NODES 1-3

    NODE1
    	docker run --restart=unless-stopped -d -h consul-agt1 --name consul-agt1 \
    	-p 8300:8300 \
    	-p 8301:8301 -p 8301:8301/udp \
    	-p 8302:8302 -p 8302:8302/udp \
    	-p 8400:8400 \
    	-p 8500:8500 \
    	-p 8600:8600/udp \
    	progrium/consul -rejoin -advertise 10.0.4.5 -join 10.0.1.5

    NODE2
    	docker run --restart=unless-stopped -d -h consul-agt2 --name consul-agt2 \
    	-p 8300:8300 \
    	-p 8301:8301 -p 8301:8301/udp \
    	-p 8302:8302 -p 8302:8302/udp \
    	-p 8400:8400 \
    	-p 8500:8500 \
    	-p 8600:8600/udp \
    	progrium/consul -rejoin -advertise 10.0.4.5 -join 10.0.1.5

    NODE3
    	docker run --restart=unless-stopped -d -h consul-agt3 --name consul-agt3 \
    	-p 8300:8300 \
    	-p 8301:8301 -p 8301:8301/udp \
    	-p 8302:8302 -p 8302:8302/udp \
    	-p 8400:8400 \
    	-p 8500:8500 \
    	-p 8600:8600/udp \
    	progrium/consul -rejoin -advertise 10.0.4.5 -join 10.0.1.5		



    ####### SWARM MANAGER BUILD COMMANDS

    NODE1
    	docker run --restart=unless-stopped -h mgr1 --name mgr1 -d -p 3375:2375 swarm manage --replication --advertise 10.0.1.5:3375 consul://10.0.1.5:8500/

    NODE2
    	docker run --restart=unless-stopped -h mgr2 --name mgr2 -d -p 3375:2375 swarm manage --replication --advertise 10.0.2.5:3375 consul://10.0.2.5:8500/

    NODE3
    	docker run --restart=unless-stopped -h mgr3 --name mgr3 -d -p 3375:2375 swarm manage --replication --advertise 10.0.3.5:3375 consul://10.0.3.5:8500/


    ####### SWARM JOIN COMMANDS TO JOIN NODES TO THE CLUSTER

    NODE1
    	docker run -d swarm join --advertise=10.0.4.5:2375 consul://10.0.4.5:8500/

    NODE2
    	docker run -d swarm join --advertise=10.0.5.5:2375 consul://10.0.5.5:8500/

    NODE3
    	docker run -d swarm join --advertise=10.0.6.5:2375 consul://10.0.6.5:8500/


    ####### To launch a local registrator service you can use the following command on each node -
    	docker run -d --name registrator -h registrator -v /var/run/docker.sock:/tmp/docker.sock gliderlabs/registrator:latest consul://10.0.1.5:8500
