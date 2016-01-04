---
layout: page
title: Upgrade Docker в Ubuntu
permalink: /linux/virtual/docker/basics/upgrade-docker-on-ubuntu/
---


    # docker -v
    Docker version 1.4.1, build 5bc2ff8


<br/>


    # apt-cache search docker
    ***
    lxc-docker-1.7.1 - Linux container runtime

<br/>

    # apt-get update

Удаляем

    # apt-get purge -y lxc-docker*



<!-- $ apt-get install docker-engine -->

Ставим заново

    # apt-get install -y lxc-docker

<br/>

    # docker -v
    Docker version 1.7.1, build 786b29d
