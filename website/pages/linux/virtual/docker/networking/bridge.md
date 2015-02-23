---
layout: page
title: Базовая настройка сети для Docker с использованием моста в Ubuntu
permalink: /linux/virtual/docker/networking/ubuntu-bridge/
---


    sudo apt-get install bridge-utils

    sudo service docker stop
    sudo ip link set dev docker0 down

    sudo brctl delbr docke0
    sudo brctl addbr bridge0
    sudo ip addr add 192.168.0.1/24 dev brigde0
    sudo ip link set dev bridge0 up


vi /etc/default

    DOCKER_OPTS=" -b=bridge0"

    sudo service docker start
