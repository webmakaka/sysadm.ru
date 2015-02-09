---
layout: page
title: Docker
permalink: /docker/installing-docker-on-ubuntu/
---


## 04 Installing and Updating Docker

04 02 Installing Docker on Ubuntu


    $ uname -a
Linux workstation 3.13.0-45-generic #74-Ubuntu SMP Tue Jan 13 19:36:28 UTC 2015 x86_64 x86_64 x86_64 GNU/Linux


    $ sudo su

    # apt-get update
    # apt-get install -y docker.io
    # service docker.io status


Я поставил версию поновее, старую версию ставить не стал.

    $ docker -v
        Docker version 1.4.1, build 5bc2ff8

    $ docker version
        Client version: 1.4.1
        Client API version: 1.16
        Go version (client): go1.3.3
        Git commit (client): 5bc2ff8
        OS/Arch (client): linux/amd64
        Server version: 1.4.1
        Server API version: 1.16
        Go version (server): go1.3.3
        Git commit (server): 5bc2ff8


    $ docker info
        Containers: 1
        Images: 125
        Storage Driver: aufs
         Root Dir: /var/lib/docker/aufs
         Dirs: 127
        Execution Driver: native-0.2
        Kernel Version: 3.13.0-45-generic
        Operating System: Ubuntu 14.04.1 LTS
        CPUs: 8
        Total Memory: 23.54 GiB
        Name: workstation
        ID: 7OQQ:XF7B:TC2S:4UWQ:R4JB:VC3K:Q6TA:5ROP:SUAC:WLJ5:Y7YA:GFAG
        WARNING: No swap limit support
