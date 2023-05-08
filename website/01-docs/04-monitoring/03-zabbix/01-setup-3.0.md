---
layout: page
title: Инсталляция Zabbix 3.X в Ubuntu Linux 16.04 (Xenial)
description: Инсталляция Zabbix 3.X в Ubuntu Linux 16.04 (Xenial)
keywords: adm, monitoring, zabbix, ubuntu, setup
permalink: /monitoring/zabbix/3.x/ubuntu/16.04/setup/
---

# Инсталляция Zabbix 3.X в Ubuntu Linux 16.04 (Xenial)

<br/>

https://www.zabbix.com/documentation/3.2/manual/installation/install_from_packages/repository_installation

    # cd /tmp
    # wget http://repo.zabbix.com/zabbix/3.2/ubuntu/pool/main/z/zabbix-release/zabbix-release_3.2-1+xenial_all.deb
    # dpkg -i zabbix-release_3.2-1+xenial_all.deb
    # apt-get update

<br/>

### Installing packages

    # apt-get install -y zabbix-server-mysql zabbix-frontend-php

    # service mysql restart

<br/>

    shell> mysql -uroot
    mysql> create database zabbix character set utf8 collate utf8_bin;
    mysql> grant all privileges on zabbix.* to zabbix@localhost identified by 'P@SSW0RD';
    mysql> quit;

<br/>

    # zcat /usr/share/doc/zabbix-server-mysql/create.sql.gz | mysql -uzabbix -p zabbix

<br/>

    # vi /etc/zabbix/zabbix_server.conf
    DBHost=localhost
    DBName=zabbix
    DBUser=zabbix
    DBPassword=P@SSW0RD

<br/>

    # service zabbix-server start
    # update-rc.d zabbix-server enable

<!-- <br/>

    # vi /etc/zabbix/apache.conf

    (может быть еще в этом нужно указать # vi /etc/php/7.0/cli/php.ini)

    php_value date.timezone Europe/Moscow -->

<br/>

    # vi /etc/php/7.0/apache2/php.ini

    date.timezone = Europe/Moscow

<br/>

    # apt-get install -y php-bcmath php-mbstring php-xmlwriter php-xmlreader

<br/>

// Чтобы можно было обращаться к серверу по url без префикса zabbix

    # vi /etc/apache2/sites-enabled/000-default.conf

    # DocumentRoot /var/www/html
    DocumentRoot /usr/share/zabbix

<br/>

    # service apache2 restart

<br/>

### Installing frontend

http://localhost/zabbix/

<br/>

После инсталляции:

login: Admin/zabbix

<br/>

### Установка Zabbix агента

    # apt-get install -y zabbix-agent
    # vi /etc/zabbix/zabbix_agentd.conf

При необходимости поменять:

    Server=Zabbix.Server.IP.Address
    Hostname=Hostname_Of_Current_Machine

    # service zabbix-agent restart

<br/>

### Настройка Zabbix на мониторинг сайтов:

https://www.youtube.com/watch?v=EWV8A29wvlk

Configuration --> Hosts --> Zabbix server --> Enable

Configuration --> Hosts --> Applications --> Create Application --> WebSites

// Повторять при добавлении нового хоста:

Configuration --> Hosts --> Zabbix server --> Web Scenarios --> Create Web Scenario

Name: Hostname
Application: WebSites

Steps --> add -->

Name: index
url: hostname
require status codes: 200

Monitoring --> Latest data --> zabbix server --> latest data --> Websites

Monitoring --> Web
