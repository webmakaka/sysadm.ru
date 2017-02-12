---
layout: page
title: Инсталляция Vargant в Ubuntu 14.04
permalink: /linux/virtual/vagrant/installation/ubuntu-14-04/
---


# Инсталляция Vargant в Ubuntu 14.04


DOWNLOAD VAGRANT:  
https://www.vagrantup.com/downloads.html

<br/>

### Инсталляция Vagrant

    # cd /tmp
    # wget https://releases.hashicorp.com/vagrant/1.9.1/vagrant_1.9.1_x86_64.deb

    # dpkg -i vagrant_1.9.1_x86_64.deb

    # vagrant -v
    Vagrant 1.9.1

    # vagrant version
    Installed Version: 1.9.1
    Latest Version: 1.9.1

    You're running an up-to-date version of Vagrant!




<br/>


### Удаление Vagrant

    # apt-get remove -y vagrant
