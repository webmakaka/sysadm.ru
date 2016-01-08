---
layout: page
title: Инсталляция Vargant в Ubuntu 14.04
permalink: /linux/virtual/vagrant/installation/ubuntu-14-04/
---


<br/>

### Инсталляция Vagrant

    # cd /tmp
    # wget https://releases.hashicorp.com/vagrant/1.8.1/vagrant_1.8.1_x86_64.deb

    # dpkg -i vagrant_1.8.1_x86_64.deb

    $ vagrant -v
    Vagrant 1.8.1


<br/>

### Создание виртуальной машины с помощью Vagrant

https://atlas.hashicorp.com/boxes/search

    $ vagrant init bento/ubuntu-14.04

    $ vi Vagrantfile

    $ vagrant up

    $ vagrant ssh

    $ vagrant halt
