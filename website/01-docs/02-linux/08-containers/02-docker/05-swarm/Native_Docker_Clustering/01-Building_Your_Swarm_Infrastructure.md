---
layout: page
title: Native Docker Clustering
permalink: /linux/containers/docker/swarm/Native_Docker_Clustering/Building_Your_Swarm_Infrastructure/
---

# Docker Swarm: Native Docker Clustering [2016, ENG] > Module 4: Building your Swarm Infrastructure



**Файлы для старта виртуальных машин с coreos**

https://github.com/sysadm-ru/Native-Docker-Clustering



<br/>

    $ vagrant up


<br/>

**Инсталляция python 2 в coreos**

https://raw.githubusercontent.com/ziozzang/python-on-coreos/master/install-python-on-coreos.sh


<br/>

### CONSUL BUILD COMMANDS


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

    core@core-01 ~ $ docker exec -it consul1 bash

<br/>

    bash-4.3# consul members
    Node     Address        Status  Type    Build  Protocol  DC
    consul1  10.0.1.5:8301  alive   server  0.5.2  2         dc1
    consul2  10.0.2.5:8301  alive   server  0.5.2  2         dc1
    consul3  10.0.3.5:8301  alive   server  0.5.2  2         dc1



<br/>

### SWARM MANAGER BUILD COMMANDS


    **core 01**

    $ docker run --restart=unless-stopped -h mgr1 --name mgr1 -d -p 3375:2375 swarm manage --replication --advertise 10.0.1.5:3375 consul://10.0.1.5:8500/


    **core 02**

    $ docker run --restart=unless-stopped -h mgr2 --name mgr2 -d -p 3375:2375 swarm manage --replication --advertise 10.0.2.5:3375 consul://10.0.2.5:8500/


    **core 03**

    $ docker run --restart=unless-stopped -h mgr3 --name mgr3 -d -p 3375:2375 swarm manage --replication --advertise 10.0.3.5:3375 consul://10.0.3.5:8500/


<br/>

    **core 01**

    $ docker logs mgr1
    time="2017-01-31T21:01:29Z" level=info msg="Initializing discovery without TLS"
    time="2017-01-31T21:01:29Z" level=info msg="Listening for HTTP" addr=":2375" proto=tcp
    time="2017-01-31T21:01:29Z" level=info msg="Leader Election: Cluster leadership lost"
    time="2017-01-31T21:01:29Z" level=info msg="Leader Election: Cluster leadership acquired"


<br/>

### CONSUL CLIENT BUILDS ON NODES 1-3


    $ vagrant ssh core-04

    **core 04**

    $ docker run --restart=unless-stopped -d -h consul-agt1 --name consul-agt1 \
    -p 8300:8300 \
    -p 8301:8301 -p 8301:8301/udp \
    -p 8302:8302 -p 8302:8302/udp \
    -p 8400:8400 \
    -p 8500:8500 \
    -p 8600:8600/udp \
    progrium/consul -rejoin -advertise 10.0.4.5 -join 10.0.1.5


<br/>

    $ vagrant ssh core-05

    **core 05**

	$ docker run --restart=unless-stopped -d -h consul-agt2 --name consul-agt2 \
    -p 8300:8300 \
    -p 8301:8301 -p 8301:8301/udp \
    -p 8302:8302 -p 8302:8302/udp \
    -p 8400:8400 \
    -p 8500:8500 \
    -p 8600:8600/udp \
    progrium/consul -rejoin -advertise 10.0.5.5 -join 10.0.1.5


<br/>

    $ vagrant ssh core-06

    **core 06**

    $ docker run --restart=unless-stopped -d -h consul-agt3 --name consul-agt3 \
    -p 8300:8300 \
    -p 8301:8301 -p 8301:8301/udp \
    -p 8302:8302 -p 8302:8302/udp \
    -p 8400:8400 \
    -p 8500:8500 \
    -p 8600:8600/udp \
    progrium/consul -rejoin -advertise 10.0.6.5 -join 10.0.1.5


<br/>

### SWARM JOIN COMMANDS TO JOIN NODES TO THE CLUSTER


    **core 04**

    $ docker run -d swarm join --advertise=10.0.4.5:2375 consul://10.0.4.5:8500/


    **core 05**

    $ docker run -d swarm join --advertise=10.0.5.5:2375 consul://10.0.5.5:8500/


    **core 06**

    $ docker run -d swarm join --advertise=10.0.6.5:2375 consul://10.0.6.5:8500/

<br/>

    # consul members
    Node         Address        Status  Type    Build  Protocol  DC
    consul-agt3  10.0.6.5:8301  alive   client  0.5.2  2         dc1
    consul1      10.0.1.5:8301  alive   server  0.5.2  2         dc1
    consul3      10.0.3.5:8301  alive   server  0.5.2  2         dc1
    consul2      10.0.2.5:8301  failed  server  0.5.2  2         dc1
    consul-agt1  10.0.4.5:8301  alive   client  0.5.2  2         dc1
    consul-agt2  10.0.5.5:8301  alive   client  0.5.2  2         dc1



<br/>

    $ curl http://10.0.3.5:8500/v1/catalog/nodes | python -m json.tool


    [
        {
            "Address": "10.0.4.5",
            "Node": "consul-agt1"
        },
        {
            "Address": "10.0.5.5",
            "Node": "consul-agt2"
        },
        {
            "Address": "10.0.6.5",
            "Node": "consul-agt3"
        },
        {
            "Address": "10.0.1.5",
            "Node": "consul1"
        },
        {
            "Address": "10.0.2.5",
            "Node": "consul2"
        },
        {
            "Address": "10.0.3.5",
            "Node": "consul3"
        }
    ]


<br/>

    На всех:

    $ docker run -d --name registrator -h registrator -v /var/run/docker.sock:/tmp/docker.sock gliderlabs/registrator:latest consul://10.0.1.5:8500


<br/>

    $ curl http://10.0.3.5:8500/v1/catalog/services | python -m json.tool


    {
        "consul": [],
        "consul-53": [
            "udp"
        ],
        "consul-8300": [],
        "consul-8301": [
            "udp"
        ],
        "consul-8302": [
            "udp"
        ],
        "consul-8400": [],
        "consul-8500": [],
        "consul-8600": [
            "udp"
        ],
        "swarm": []
    }


<br/>

    $ curl http://10.0.3.5:8500/v1/catalog/service/swarm | python -m json.tool

    [
        {
            "Address": "10.0.1.5",
            "Node": "consul1",
            "ServiceAddress": "",
            "ServiceID": "registrator:mgr1:2375",
            "ServiceName": "swarm",
            "ServicePort": 3375,
            "ServiceTags": null
        },
        {
            "Address": "10.0.1.5",
            "Node": "consul1",
            "ServiceAddress": "",
            "ServiceID": "registrator:mgr2:2375",
            "ServiceName": "swarm",
            "ServicePort": 3375,
            "ServiceTags": null
        },
        {
            "Address": "10.0.1.5",
            "Node": "consul1",
            "ServiceAddress": "",
            "ServiceID": "registrator:mgr3:2375",
            "ServiceName": "swarm",
            "ServicePort": 3375,
            "ServiceTags": null
        }
    ]



<br/>

    core 04

    $ docker run -d --name web1 -p 80:80 nginx

<br/>

    $ curl http://10.0.3.5:8500/v1/catalog/services | python -m json.tool

        должен появиться nginx

    {
        "consul": [],
        "consul-53": [
            "udp"
        ],
        "consul-8300": [],
        "consul-8301": [
            "udp"
        ],
        "consul-8302": [
            "udp"
        ],
        "consul-8400": [],
        "consul-8500": [],
        "consul-8600": [
            "udp"
        ],
        "nginx-80": [],
        "swarm": []
    }


Пока хз что да как но работает.
