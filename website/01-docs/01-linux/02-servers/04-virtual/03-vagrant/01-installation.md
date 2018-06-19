---
layout: page
title: Инсталляция Vargant в Ubuntu 14.04
permalink: /linux/servers/virtual/vagrant/installation/ubuntu-14-04/
---


# Инсталляция Vargant в Ubuntu 14.04

Делаю 
02.04.2018

<br/>

DOWNLOAD VAGRANT:  
https://www.vagrantup.com/downloads.html


<br/>

У меня уже был установлен. Чего-то старый какой-то. Надо бы его обновить!

    # vagrant version
    Installed Version: 1.9.1
    Latest Version: 2.0.3

<br/>

### Удаляю Vagrant

    # apt-get remove -y vagrant


<br/>

### Инсталляция Vagrant

    # cd /tmp
    # wget https://releases.hashicorp.com/vagrant/2.0.3/vagrant_2.0.3_x86_64.deb

    # dpkg -i vagrant_2.0.3_x86_64.deb

    # vagrant -v
    Vagrant 2.0.3
