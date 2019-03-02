---
layout: page
title: Инсталляция Vargant в Ubuntu 18.04
permalink: /linux/servers/virtual/vagrant/installation/ubuntu/
---

# Инсталляция Vargant в Ubuntu 18.04

Делаю
02.03.2019

<br/>

DOWNLOAD VAGRANT:  
https://www.vagrantup.com/downloads.html

<br/>

### Инсталляция Vagrant

    # cd /tmp
    # wget https://releases.hashicorp.com/vagrant/2.2.4/vagrant_2.2.4_x86_64.deb

    # dpkg -i vagrant_2.2.4_x86_64.deb

    # vagrant -v
    Vagrant 2.2.4

    $ vagrant plugin update

<br/>

### Удалить Vagrant

    # apt remove -y vagrant
