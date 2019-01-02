---
layout: page
title: Инсталляция Vargant в Ubuntu 18.04
permalink: /linux/servers/virtual/vagrant/installation/ubuntu/
---


# Инсталляция Vargant в Ubuntu 18.04

Делаю 
23.11.2018

<br/>

DOWNLOAD VAGRANT:  
https://www.vagrantup.com/downloads.html


<br/>

У меня уже был установлен. Чего-то старый какой-то. Надо бы его обновить!

    # vagrant version
    Installed Version: 2.0.3
    Latest Version: 2.2.1

<br/>

### Удаляю Vagrant

    # apt-get remove -y vagrant

<br/>

### Инсталляция Vagrant

    # cd /tmp
    # wget https://releases.hashicorp.com/vagrant/2.2.1/vagrant_2.2.1_x86_64.deb

    # dpkg -i vagrant_2.2.1_x86_64.deb

    # vagrant -v
    Vagrant 2.2.1

    $ vagrant plugin update

