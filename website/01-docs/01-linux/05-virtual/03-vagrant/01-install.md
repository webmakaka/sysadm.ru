---
layout: page
title: Инсталляция Vargant в Ubuntu 18.04
permalink: /linux/virtual/vagrant/install/ubuntu/
---

# Инсталляция Vargant в Ubuntu 18.04

Делаю  
03.11.2019

<br/>

DOWNLOAD VAGRANT:  
https://www.vagrantup.com/downloads.html

<br/>

### Инсталляция Vagrant

    $ sudo su -
    # cd /tmp
    # wget https://releases.hashicorp.com/vagrant/2.2.6/vagrant_2.2.6_x86_64.deb

    # dpkg -i vagrant_2.2.6_x86_64.deb

    # vagrant -v
    Vagrant 2.2.6

<br/>

### Работа с plugin в Vagrant

    $ vagrant plugin update
    $ vagrant plugin list

<br/>

### Удалить Vagrant

    $ sudo apt remove -y vagrant
