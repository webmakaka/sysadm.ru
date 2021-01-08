---
layout: page
title: Пакеты которые нужно установить после установки чистой операционнй системы для комфортной работы
description: Пакеты которые нужно установить после установки чистой операционнй системы для комфортной работы
keywords: Пакеты которые нужно установить после установки чистой операционнй системы для комфортной работы
permalink: /desktop/linux/install/centos/6/steps-after-installation-centos-6x/
---

# Пакеты которые нужно установить после установки чистой операционнй системы для комфортной работы

<br/><br/>

    # yum update -y
    # yum upgrade -y

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

<br/>

<!--

    # vi ~/.bash_profile
    alias vi="vim"

-->

    # yum install -y ntp
    # chkconfig --level 345 ntpd on
    # service ntpd start
    # ntpdate pool.ntp.org

<br/>

### Какие-то настройки для SSH

    # cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bkp

<br/>

    # vi /etc/ssh/sshd_config

    #UseDNS yes
    меняем на
    UseDNS no

    GSSAPIAuthentication yes
    меняем на
    GSSAPIAuthentication no

<br/>

    #  service sshd restart
