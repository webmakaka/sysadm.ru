---
layout: page
title: Инсталляция Apache Httpd сервер на Centos 6.6 из пакетов
permalink: /linux/webservers/apache/install/
---

# Инсталляция Apache Httpd сервер на Centos 6.6 из пакетов

    # vi /etc/hosts
    192.168.1.202 webserv.local webserv

    # vi /etc/sysconfig/network

    NETWORKING=yes
    NETWORKING_IPV6=no
    HOSTNAME=webserv


    # yum list httpd*

    # yum install -y \
    httpd.x86_64


    # rpm -qa | grep -i httpd
    httpd-tools-2.2.15-30.el6.centos.x86_64
    httpd-2.2.15-30.el6.centos.x86_64

    # chkconfig httpd on
    # service httpd start

FIREWALL

    # cp /etc/sysconfig/iptables /etc/sysconfig/iptables.bkp

    # iptables -A INPUT -s 192.168.1.0/24  -m state --state NEW -p tcp --dport 80 -j ACCEPT
    # service iptables save
    # service iptables restart
