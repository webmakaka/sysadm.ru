---
layout: page
title: Nagios
permalink: /linux/monitoring/nagios/
---

# Nagios

<br/>

### Инсталляция Nagios в Ubuntu Linux


Взято из курса [OReilly Media  Infinite Skills  David Josephsen] Modern Nagios Training Video [2016, ENG].

В должной мере пока не тестировалось.


    # apt-get install build-essential


<br/>

### pre-requisites

    # apt-get install -y \
    libgd2-xpm-dev \
    php5-fpm \
    spawn-fcgi \
    fcgiwrap \
    unzip \
    nginx


<br/>

### Get nagios core and the tarballs

    $ cd /tmp/
    $ wget 'https://assets.nagios.com/downloads/nagioscore/releases/nagios-4.1.1.tar.gz'
    $ wget 'http://www.nagios-plugins.org/download/nagios-plugins-2.1.1.tar.gz'


<br/>

### add users and groups

    $ sudo adduser --system --no-create-home --disabled-login --group nagios
    $ sudo groupadd nagcmd
    $ sudo usermod -G nagcmd,www-data nagios


<br/>

### build and install nagios

    tar -zxvf nagios-4.1.1.tar.gz
    cd nagios-4.1.1
    ./configure -h
    ./configure --prefix=/usr/local/nagios-4.1.1 --with-command-group=nagcmd

     #--sysconfdir=/etc/nagios


    sudo make all install
    sudo make install-init
    sudo make install-config
    sudo make install-commandmode

<br/>

### (optional) link nagios-version to /usr/local/nagios

    sudo ln -s /usr/local/nagios-4.1.1 /usr/local/nagios
    sudo ln -s /usr/local/nagios/etc /etc/nagios


<br/>

### (optional) fix var

    sudo mkdir /var/log/nagios
    sudo chown nagios:nagios /var/log/nagios
    sudo vi /etc/nagios/nagios.cfg
    sudo log_file=/var/log/nagios/nagios.log

<br/>

### add a contact

    sudo vi /etc/nagios/objects/contacts.cfg
    :%s/nagiosadmin/katops/g


<br/>

### Build and install the plugins

    tar -zxvf nagios-plugins-2.1.1.tar.gz
    cd nagios-plugins-2.1.1
    ./configure --with-nagios-user=nagios --with-nagios-group=nagios
    sudo make install


<br/>

### test the configuration

    /usr/local/nagios/bin/nagios -v /etc/nagios/nagios.cfg


<br/>

### start nagios core

    sudo /etc/init.d/nagios start

<br/>

### start the cgi server

    service fcgiwrap restart
    service php5-fpm restart

<br/>

### edit the nginx config

    sudo vi /etc/nginx/sites-available/nagios.busycorp.com.conf
    sudo rm /etc/nginx/sites-enabled/default
    sudo ln -s /etc/nginx/sites-available/nagios.busycorp.com.conf /etc/nginx/sites-enabled


<br/>

### create a password for the nagios web ui

    P=$(openssl passwd -crypt)
    sudo echo "katops:${P}" >> /etc/nagios/htpasswd.users

<br/>

### bring up the nagios core UI

    sudo service nginx reload


<br/>

### grant cgi permissions to the UI

    sudo vi /etc/nagios/cgi.cfg
    :%s/nagiosadmin/katops/g
