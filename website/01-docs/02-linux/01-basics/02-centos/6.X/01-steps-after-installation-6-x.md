---
layout: page
title: Пакеты которые нужно установить после установки чистой операционнй системы для комфортной работы
permalink: /linux/basics/centos/6/steps-after-installation/
---


### Пакеты которые нужно установить после установки чистой операционнй системы для комфортной работы

<br/><br/>

    # yum update -y
    # yum upgrade -y

    # yum repolist

    # yum update -y

    # yum install -y \
    vim \
    wget  \
    make \
    gcc \
    kernel-devel \
    openssh-server \
    openssh-clients \
    perl \
    bind-utils \
    traceroute \
    tcpdump \
    screen \
    telnet \
    nc \
    lsof


    В bind-utils лежит nslookup


    # vi ~/.bash_profile
    alias vi="vim"

    # yum install -y ntp
    # chkconfig --level 345 ntpd on
    # service ntpd start
    # ntpdate pool.ntp.org


<br/>

### Какие-то настройки для SSH


    # cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bkp

    # vi /etc/ssh/sshd_config

    #UseDNS yes
    меняем на
    UseDNS no

    GSSAPIAuthentication yes
    меняем на
    GSSAPIAuthentication no


    #  service sshd restart
