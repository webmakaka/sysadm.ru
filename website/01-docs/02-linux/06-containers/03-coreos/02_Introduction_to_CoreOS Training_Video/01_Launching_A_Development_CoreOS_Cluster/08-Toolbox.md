---
layout: page
title: Toolbox
permalink: /linux/containers/coreos/Introduction_to_CoreOS/Launching_A_Development_CoreOS_Cluster/Toolbox/
---


# [O’Reilly Media / Infinite Skills] Introduction to CoreOS Training Video [2015, ENG] : Launching A Development CoreOS Cluster : Toolbox



### TOOLBOX

Fedora - по умолчанию

    $ toolbox
    # yum install -y htop
    # htop

<br/>

Если нужно debian

    $ vi .toolboxrc

    TOOLBOX_DOCKER_IMAGE=debian
    TOOLBOX_DOCKER_TAG=jessie
    TOOLBOX_DOCKER_USER=root

    $ toolbox

    # apt-get update && apt-get install -y htop
    # htop
