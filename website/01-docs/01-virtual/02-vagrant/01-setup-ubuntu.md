---
layout: page
title: Инсталляция Vargant в Ubuntu 20.04.1
description: Инсталляция Vargant в Ubuntu 20.04.1
keywords: linux, ubuntu, vagrant, install
permalink: /virtual/vagrant/setup/ubuntu/
---

# Инсталляция Vargant в Ubuntu 20.04.1

Делаю  
21.09.2021

<br/>

DOWNLOAD VAGRANT:  
https://www.vagrantup.com/downloads.html

<br/>

### Инсталляция Vagrant

    $ cd /tmp
    $ wget https://releases.hashicorp.com/vagrant/2.2.18/vagrant_2.2.18_x86_64.deb

    $ sudo dpkg -i vagrant_2.2.18_x86_64.deb

    $ vagrant -v
    Vagrant 2.2.18

<br/>

### Работа с plugin в Vagrant

    $ vagrant plugin update
    $ vagrant plugin list

<br/>

### Удалить Vagrant

    $ sudo apt remove -y vagrant
