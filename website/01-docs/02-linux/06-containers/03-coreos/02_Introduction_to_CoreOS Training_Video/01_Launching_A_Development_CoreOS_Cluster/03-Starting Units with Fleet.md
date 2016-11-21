---
layout: page
title: Starting Units with Fleet
permalink: /linux/containers/coreos/Introduction_to_CoreOS/Launching_A_Development_CoreOS_Cluster/Starting_Units_with_Fleet/
---


# [O’Reilly Media / Infinite Skills] Introduction to CoreOS Training Video [2015, ENG] : Launching A Development CoreOS Cluster : Starting Units with Fleet



### Statring Units with Fleet


    $ ssh-add ~/.vagrant.d/insecure_private_key

<br/>

    $ vagrant ssh core-01 -- -A

<br/>

    $ fleetctl list-unit-files

<br/>

    $ fleetctl list-units
    UNIT	HASH	DSTATE	STATE	TARGET


<br/>

    # vi ~/hellofleet.service

    [Unit]
    Description=Hello Fleet Service
    After=docker.service
    Requires=docker.service

    [Service]
    TimeoutStartSec=0
    ExecStartPre=-/usr/bin/docker kill hello-fleet
    ExecStartPre=-/usr/bin/docker rm hello-fleet
    ExecStartPre=/usr/bin/docker pull busybox
    ExecStart=/usr/bin/docker run --name hello-fleet busybox /bin/sh -c "while true; do echo Hello Fleet; sleep 1; done"
    ExecStop=-/usr/bin/docker rm -f hello-fleet


<br/>

    $ fleetctl submit hellofleet.service
    Unit hellofleet.service inactive

<br/>

    $ fleetctl list-unit-files          
    UNIT			HASH	DSTATE		STATE		TARGET
    hellofleet.service	9b0408f	inactive	inactive	-

<br/>

    $ fleetctl load hellofleet
    Unit hellofleet.service loaded on 48777333.../172.17.8.103


<br/>

    $ fleetctl list-units     
    UNIT			MACHINE				ACTIVE		SUB
    hellofleet.service	48777333.../172.17.8.103	inactive	dead

<br/>


    $ fleetctl start hellofleet
    Unit hellofleet.service launched on 48777333.../172.17.8.103

<br/>

    $ fleetctl list-units
    UNIT			MACHINE				ACTIVE	SUB
    hellofleet.service	48777333.../172.17.8.103	active	running

<br/>

    $ fleetctl status hellofleet
    ● hellofleet.service - Hello Fleet Service
       Loaded: loaded (/run/fleet/units/hellofleet.service; linked-runtime; vendor preset: disabled)
       Active: active (running) since Thu 2016-01-07 23:03:26 UTC; 1min 16s ago
      Process: 1357 ExecStartPre=/usr/bin/docker pull busybox (code=exited, status=0/SUCCESS)
      Process: 1350 ExecStartPre=/usr/bin/docker rm hello-fleet (code=exited, status=1/FAILURE)
      Process: 1295 ExecStartPre=/usr/bin/docker kill hello-fleet (code=exited, status=1/FAILURE)
     Main PID: 1382 (docker)
       Memory: 11.3M
          CPU: 155ms
       CGroup: /system.slice/hellofleet.service
               └─1382 /usr/bin/docker run --name hello-fleet busybox /bin/sh -c while true; do echo Hello Fleet; sleep 1; done

    Jan 07 23:04:33 core-03 docker[1382]: Hello Fleet
    Jan 07 23:04:34 core-03 docker[1382]: Hello Fleet
    Jan 07 23:04:35 core-03 docker[1382]: Hello Fleet
    Jan 07 23:04:36 core-03 docker[1382]: Hello Fleet
    Jan 07 23:04:37 core-03 docker[1382]: Hello Fleet
    Jan 07 23:04:38 core-03 docker[1382]: Hello Fleet
    Jan 07 23:04:39 core-03 docker[1382]: Hello Fleet
    Jan 07 23:04:40 core-03 docker[1382]: Hello Fleet
    Jan 07 23:04:41 core-03 docker[1382]: Hello Fleet
    Jan 07 23:04:42 core-03 docker[1382]: Hello Fleet



    $ fleetctl stop hellofleet.service



    $ fleetctl list-units     
    UNIT			MACHINE				ACTIVE	SUB
    hellofleet.service	48777333.../172.17.8.103	failed	failed

    $ fleetctl destroy hellofleet.service

<br/>

### Global Service

https://github.com/rosskukulinski/Introduction_To_CoreOS/blob/master/Chapter%204/global_units/global.service


<br/>

### Dockerized Node.js Application


https://github.com/rosskukulinski/Introduction_To_CoreOS/tree/master/Chapter%204/Dockerized_App


    $ docker run --rm -ti -p 3000:3000 -e INSTANCE=instance1 rosskukulinski/nodeapp1


    $ docker inspect --format='' containerId

    $ curl 172.17.0.2:3000
    Hello from instance1 running on c2108d6d0bf6


    $ fleetctl start nodeapp@.service
    $ fleetctl start nodeapp-v2@{1..2}


<br/>

### Interacting with ETCD

    $ etcdctl mkdir /my_data
    $ etcdctl ls / --recursive

    $ etcdctl mk /my_data/key myvalue
myvalue

    $ etcdctl get /my_data/key       
    myvalue

    $ etcdctl update /my_data/key myNewValue

    $ etcdctl set /my_data/key2 mykey2

    $ etcdctl set /my_data/expiring_key byebye --ttl 5


<br/>

### TOOLBOX

    $ vi .toolboxrc

    TOOLBOX_DOCKER_IMAGE=debian
    TOOLBOX_DOCKER_TAG=jessie
    TOOLBOX_DOCKER_USER=root

    $ toolbox
