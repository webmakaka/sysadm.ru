---
layout: page
title: Installing check_mk monitoring service in Linux
permalink: /devops/linux/monitoring/check-mk/install/
---

# Installing check_mk monitoring service in Linux

Делаю  
01.05.2019

По материалам видео индуса:  
https://www.youtube.com/watch?v=fixkbancVc4&list=PL34sAs7_26wODGPEgaHrhRiXkax0q7_v5

<br/>

Предполагается что уже установлен <a href="/devops/linux/virtual/virtualbox/install/">VirtualBox</a>, <a href="/devops/linux/virtual/vagrant/install/ubuntu/">Vagrant</a>.

<br/>

    // Также предполагается, что установлен vagrant-hostmanager
    $ vagrant plugin install vagrant-hostmanager

<br/>

    $ cd ~
    $ git clone https://bitbucket.org/sysadm-ru/omd-monitoring.git
    $ cd omd-monitoring/vagrant-provisioning/

<br/>

https://mathias-kettner.com/download_version.php?&version=1.5.0p16&edition=cre

<br/>

### Инсталляция check_mk в Centos 7

    $ vagrant up centosvm01
    $ vagrant ssh centosvm01

<br/>

    $ sudo yum install -y epel-release
    $ sudo yum install -y wget

<br/>

    $ wget https://mathias-kettner.de/support/1.5.0p16/check-mk-raw-1.5.0p16-el7-38.x86_64.rpm

    $ sudo yum install -y check-mk-raw-1.5.0p16-el7-38.x86_64.rpm

<br/>

    $ omd version
    OMD - Open Monitoring Distribution Version 1.5.0p16.cre

<br/>

    // Можно просто отключить SELINUX, но мы делаем следующее
    $ sudo setsebool -P httpd_can_network_connect 1
    $ sudo getsebool httpd_can_network_connect
    httpd_can_network_connect --> on

<br/>

    $ sudo systemctl start firewalld

<br/>

    $ sudo firewall-cmd --list-all
    public (active)
    target: default
    icmp-block-inversion: no
    interfaces: eth0 eth1
    sources:
    services: ssh dhcpv6-client
    ports:
    protocols:
    masquerade: no
    forward-ports:
    source-ports:
    icmp-blocks:
    rich rules:

<br/>

    $ sudo firewall-cmd --permanent --add-service http
    $ sudo firewall-cmd --reload

<br/>

### Инсталляция check_mk в Ubuntu 18

    $ vagrant up ubuntuvm01
    $ vagrant ssh ubuntuvm01

<br/>

    $ sudo apt-get update
    $ sudo apt-get install -y gdebi-core

    $ wget https://mathias-kettner.de/support/1.5.0p16/check-mk-raw-1.5.0p16_0.bionic_amd64.deb

    $ sudo gdebi check-mk-raw-1.5.0p16_0.bionic_amd64.deb

    $ omd version
    OMD - Open Monitoring Distribution Version 1.5.0p16.cre

