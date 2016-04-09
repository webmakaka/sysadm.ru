---
layout: page
title: Базовая настройка сети для Docker с использованием моста в Ubuntu
permalink: /linux/virtual/docker/networking/ubuntu-bridge/
---


    sudo apt-get install bridge-utils

<br/>

    sudo brctl show docke0

<br/>

    sudo service docker stop
    sudo ip link set dev docker0 down

<br/>

    sudo brctl delbr docke0
    sudo brctl addbr bridge0
    sudo ip addr add 192.168.0.1/24 dev brigde0
    sudo ip link set dev bridge0 up


<br/>

    vi /etc/default

<br/>

    DOCKER_OPTS=" -b=bridge0"

<br/>

    sudo service docker start
