---
layout: page
title: Инсталляция Vargant в Ubuntu 14.04
permalink: /linux/virtual/vagrant/installation/ubuntu-14-04/
---


<br/>

### Инсталляция Vagrant

    # cd /tmp
    # wget https://releases.hashicorp.com/vagrant/1.8.1/vagrant_1.8.1_x86_64.deb

    # dpkg -i vagrant_1.8.1_x86_64.deb

    $ vagrant -v
    Vagrant 1.8.1


<br/>

### Создание виртуальной машины с помощью Vagrant

https://atlas.hashicorp.com/boxes/search

    $ vagrant init bento/ubuntu-14.04

    $ vi Vagrantfile

    $ vagrant up

    $ vagrant ssh

    $ vagrant halt

<br/>


### Vagrant и CoreOS



Смотрю видео курс
[O'Reilly Media / Infinite Skills] Introduction to CoreOS Training Video [2015, ENG]

https://github.com/rosskukulinski/Introduction_To_CoreOS/


VirtualBox и Git должны быть установлены.


https://github.com/coreos/coreos-vagrant/



    $ git clone https://github.com/coreos/coreos-vagrant/

    $ cd coreos-vagrant/

    $ mv config.rb.sample config.rb

    $ atom .

<br/>

    config.rb

    # $num_instances=1
    $num_instances=3

    #$update_channel='alpha'
    $update_channel='stable'


<br/>

    $ vagrant up

<br/>

    $ vagrant status
    Current machine states:

    core-01                   running (virtualbox)
    core-02                   running (virtualbox)
    core-03                   running (virtualbox)


<br/>

    $ vagrant ssh core-01 -- -A

<br/>

    $ core@core-01 ~ $ fleetctl list-machines
    MACHINE		IP		METADATA
    48777333...	172.17.8.103	-
    5127495f...	172.17.8.102	-
    e20b10bc...	172.17.8.101	-


<br/>


    $ vagrant destroy


<br/>

### systemd

    $ sudo su
    core-01 core #

    # vi /etc/systemd/system/hellworld.service

<br/>

    [Unit]
    Description=Hello World Service
    After=docker.service
    Requires=docker.service

    [Service]
    TimeoutStartSec=0
    ExecStartPre=-/usr/bin/docker kill hello-world
    ExecStartPre=-/usr/bin/docker rm hello-world
    ExecStartPre=/usr/bin/docker pull busybox
    ExecStart=/usr/bin/docker run --name hello-world busybox /bin/sh -c "while true; do echo Hello World; sleep 1; done"
    ExecStop=-/usr/bin/docker rm -f hello-world


    [Install]
    WantedBy=multi-user.target

<br/>

    # systemctl enable helloworld.service
    Created symlink from /etc/systemd/system/multi-user.target.wants/helloworld.service to /etc/systemd/system/helloworld.service.

<br/>

    # systemctl status helloworld.service
    ● helloworld.service - Hello World Service
       Loaded: loaded (/etc/systemd/system/helloworld.service; enabled; vendor preset: disabled)
       Active: inactive (dead)


<br/>

    # systemctl start helloworld.service

<br/>

    # systemctl status helloworld.service
    ● helloworld.service - Hello World Service
      Loaded: loaded (/etc/systemd/system/helloworld.service; enabled; vendor preset: disabled)
      Active: active (running) since Thu 2016-01-07 22:36:17 UTC; 10s ago
     Process: 1430 ExecStartPre=/usr/bin/docker pull busybox (code=exited, status=0/SUCCESS)
     Process: 1423 ExecStartPre=/usr/bin/docker rm hello-world (code=exited, status=1/FAILURE)
     Process: 1368 ExecStartPre=/usr/bin/docker kill hello-world (code=exited, status=1/FAILURE)
    Main PID: 1449 (docker)
      Memory: 6.4M
         CPU: 142ms
      CGroup: /system.slice/helloworld.service
              └─1449 /usr/bin/docker run --name hello-world busybox /bin/sh -c w...

    Jan 07 22:36:19 core-01 docker[1449]: Hello World
    Jan 07 22:36:20 core-01 docker[1449]: Hello World
    Jan 07 22:36:21 core-01 docker[1449]: Hello World
    Jan 07 22:36:22 core-01 docker[1449]: Hello World
    Jan 07 22:36:23 core-01 docker[1449]: Hello World
    Jan 07 22:36:24 core-01 docker[1449]: Hello World
    Jan 07 22:36:25 core-01 docker[1449]: Hello World
    Jan 07 22:36:26 core-01 docker[1449]: Hello World
    Jan 07 22:36:27 core-01 docker[1449]: Hello World
    Jan 07 22:36:28 core-01 docker[1449]: Hello World

<br/>

    # journalctl -f -u helloworld.service
    -- Logs begin at Thu 2016-01-07 22:17:07 UTC. --
    Jan 07 22:37:20 core-01 docker[1449]: Hello World
    Jan 07 22:37:21 core-01 docker[1449]: Hello World
    Jan 07 22:37:22 core-01 docker[1449]: Hello World
    Jan 07 22:37:23 core-01 docker[1449]: Hello World
    Jan 07 22:37:24 core-01 docker[1449]: Hello World
    Jan 07 22:37:25 core-01 docker[1449]: Hello World



<br/>

    # systemctl stop helloworld.service



 <br/>
 <br/>

    # vi /etc/systemd/system/helloworld2@.service


    [Unit]
    Description=%n Service
    After=docker.service
    Requires=docker.service

    [Service]
    TimeoutStartSec=0
    ExecStartPre=-/usr/bin/docker kill %p-%i
    ExecStartPre=-/usr/bin/docker rm %p-%i
    ExecStartPre=/usr/bin/docker pull busybox
    ExecStart=/usr/bin/docker run --name %p-%i busybox /bin/sh -c "while true; do echo Hello from %p-%i running on %H; sleep 1; done"
    ExecStop=-/usr/bin/docker rm -f %p-%i

    [Install]
    WantedBy=multi-user.target


<br/>

    # systemctl start helloworld2@1.service

<br/>

   # journalctl -f -u helloworld2@1.service
   -- Logs begin at Thu 2016-01-07 22:17:07 UTC. --
   Jan 07 22:46:30 core-01 docker[1756]: Hello from helloworld2-1 running on core-01
   Jan 07 22:46:31 core-01 docker[1756]: Hello from helloworld2-1 running on core-01
   Jan 07 22:46:32 core-01 docker[1756]: Hello from helloworld2-1 running on core-01
   Jan 07 22:46:33 core-01 docker[1756]: Hello from helloworld2-1 running on core-0

<br/>

    # systemctl start helloworld2@{2..4}.service

<br/>

    # docker ps
    CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
    db6e56bea8d7        busybox             "/bin/sh -c 'while tr"   20 seconds ago      Up 18 seconds                           helloworld2-4
    0ba8d0a176f2        busybox             "/bin/sh -c 'while tr"   20 seconds ago      Up 18 seconds                           helloworld2-3
    f9bf85cbb10e        busybox             "/bin/sh -c 'while tr"   20 seconds ago      Up 19 seconds                           helloworld2-2
    1feb9f7b8cfe        busybox             "/bin/sh -c 'while tr"   2 minutes ago       Up 2 minutes                            helloworld2-1


<br/>

    # systemctl stop helloworld2@{2..4}.service


<br/>
<br/>

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
