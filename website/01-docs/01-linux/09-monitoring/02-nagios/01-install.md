---
layout: page
title: Инсталляция Nagios в Ubuntu Linux (Xenial)
permalink: /linux/monitoring/nagios/ubuntu/16.04/install/
---

# Инсталляция Nagios в Ubuntu Linux (Xenial)

<br/>

Взято из курса [OReilly Media  Infinite Skills  David Josephsen] Modern Nagios Training Video [2016, ENG].

Мной подредактировалось, а то конфиги немного протухли и из коробки работать отказались.


**Nagios Update Check**  
https://www.nagios.org/checkforupdates/?version=4.3.1&product=nagioscore


<br/>

### Пакеты для компиляции

    # apt-get install -y build-essential


<br/>

### pre-requisites

    # apt-get install -y \
    libgd2-xpm-dev \
    php-fpm \
    spawn-fcgi \
    fcgiwrap \
    unzip \
    nginx


<br/>

### Get nagios core and the tarballs

    $ cd /tmp/

    $ wget 'https://assets.nagios.com/downloads/nagioscore/releases/nagios-4.3.1.tar.gz'
    $ wget 'http://www.nagios-plugins.org/download/nagios-plugins-2.2.0.tar.gz'


<br/>

### add users and groups

    # adduser --system --no-create-home --disabled-login --group nagios
    # groupadd nagcmd
    # usermod -G nagcmd,www-data nagios


<br/>

### build and install nagios

    <!-- # tar -zxvf nagios-4.1.1.tar.gz -->

    # tar -zxvf nagios-4.3.1.tar.gz


    <!-- # cd nagios-4.1.1 -->

    # cd nagios-4.3.1
    # ./configure --prefix=/usr/local/nagios-4.3.1 --with-command-group=nagcmd

     <!-- #--sysconfdir=/etc/nagios -->


    # make all install
    # make install-init
    # make install-config
    # make install-commandmode

<br/>

### (optional) link nagios-version to /usr/local/nagios

    # ln -s /usr/local/nagios-4.3.1 /usr/local/nagios
    # ln -s /usr/local/nagios/etc /etc/nagios


<br/>

### (optional) fix var

    # mkdir /var/log/nagios
    # chown nagios:nagios /var/log/nagios
    # vi /etc/nagios/nagios.cfg

    log_file=/var/log/nagios/nagios.log

<br/>

### add a contact

    # vi /etc/nagios/objects/contacts.cfg
    :%s/nagiosadmin/katops/g


<br/>

### Build and install the plugins

    # cd /tmp
    # tar -zxvf nagios-plugins-2.2.0.tar.gz
    # cd nagios-plugins-2.2.0
    # ./configure --with-nagios-user=nagios --with-nagios-group=nagios
    # make install


<br/>

### test the configuration

    /usr/local/nagios/bin/nagios -v /etc/nagios/nagios.cfg


<br/>

### edit the nginx config

    # vi /etc/nginx/sites-available/nagios.busycorp.com.conf

{% highlight text %}

server {
    server_name nagios.busycorp.com;
    access_log /var/log/nginx/nagios.busycorp.com-access.log;
    error_log /var/log/nginx/nagios.busycorp.com-error.log;

    auth_basic "Private";
    auth_basic_user_file /etc/nagios/htpasswd.users;

    root /var/www/virtualhosts/nagios.busycorp.com;
    index index.php index.html;

    location / {
        try_files $uri $uri/ index.php /nagios;
    }

    location /nagios {
        alias /usr/local/nagios/share;
    }

    location ~ ^/nagios/(.*\.php)$ {
        alias /usr/local/nagios/share/$1;
        include /etc/nginx/fastcgi.conf;
        fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
    }

    location ~ \.cgi$ {
            root /usr/local/nagios/sbin/;
            rewrite ^/nagios/cgi-bin/(.*)\.cgi /$1.cgi break;
            fastcgi_param AUTH_USER $remote_user;
            fastcgi_param REMOTE_USER $remote_user;
            include /etc/nginx/fastcgi.conf;
            fastcgi_pass unix:/var/run/fcgiwrap.socket;
      }

    location ~ \.php$ {
            include /etc/nginx/fastcgi.conf;
            fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
      }
}

{% endhighlight %}


    # rm /etc/nginx/sites-enabled/default
    # ln -s /etc/nginx/sites-available/nagios.busycorp.com.conf /etc/nginx/sites-enabled


<br/>

### create a password for the nagios web ui

    # P=$(openssl passwd -crypt)
    # echo $P
    # echo "katops:${P}" >> /etc/nagios/htpasswd.users


<br/>

### grant cgi permissions to the UI

    # vi /etc/nagios/cgi.cfg
    :%s/nagiosadmin/katops/g


<br/>

### start nagios core

    # /etc/init.d/nagios start


<br/>

### start the cgi server

    # service fcgiwrap start
    # service php7.0-fpm start


### bring up the nagios core UI

    # service nginx start


<br/>

### Если что-то пошло не так

    # ps auxwww | grep php

### status

    # service nginx status
    # service php7.0-fpm status
    # service fcgiwrap status
    # service nagios status

### restart

    # service fcgiwrap restart
    # service php7.0-fpm restart
    # service nagios restart
    # service nginx restart


<br/>

### logs

    # less /var/log/nginx/error.log

    # less /var/log/nagios/nagios.log

    # less /var/log/php7.0-fpm.log

    # less /var/log/nginx/nagios.busycorp.com-access.log
    # less /var/log/nginx/nagios.busycorp.com-error.log


<br/>

### Browser

http://localhost/nagios/

    login: katops
    pass: your_pass
