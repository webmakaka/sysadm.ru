---
layout: page
title: Инсталляция Zabbix 2.X в Ubuntu Linux 16.04 (Xenial)
permalink: /linux/servers/monitoring/zabbix/2.x/ubuntu/16.04/install/
---

# Инсталляция Zabbix 2.X в Ubuntu Linux 16.04 (Xenial)


    # apt-cache search zabbix
    grafana-zabbix - Zabbix datasource for Grafana
    zabbix-agent - network monitoring solution - agent
    zabbix-frontend-php - network monitoring solution - PHP front-end
    zabbix-java-gateway - network monitoring solution - Java gateway
    zabbix-proxy-mysql - network monitoring solution - proxy (using MySQL)
    zabbix-proxy-pgsql - network monitoring solution - proxy (using PostgreSQL)
    zabbix-proxy-sqlite3 - network monitoring solution - proxy (using SQLite3)
    zabbix-server-mysql - network monitoring solution - server (using MySQL)
    zabbix-server-pgsql - network monitoring solution - server (using PostgreSQL)


<br/>

    # apt-get install -y zabbix-server-mysql
    # service mysql restart

    # ps -C mysqld
      PID TTY          TIME CMD
     6087 ?        00:00:00 mysqld

<br/>

     # cat /usr/share/doc/zabbix-server-mysql/README.Debian


<br/>


    # mysql -p -e "create database zabbix character set utf8"
    # mysql -p -e "grant all on zabbix.* to 'zabbix'@'localhost' identified by 'SECRETPASSWORD'"


    # zcat /usr/share/zabbix-server-mysql/{schema,images,data}.sql.gz \
       | mysql -uzabbix -pSECRETPASSWORD zabbix


<br/>

### Проверка

    # mysql -u zabbix -pSECRETPASSWORD

    use zabbix;
    show tables;


<br/>



    # vi /etc/zabbix/zabbix_server.conf

    DBName=zabbix
    DBUser=zabbix
    DBPassword=SECRETPASSWORD


    # service zabbix-server restart

    # less /var/log/zabbix-server/zabbix_server.log

    # ps -C zabbix_server

<br/>

### Web Interface

    # apt-get install -y zabbix-frontend-php

    # apt-get install -y apache2 php php-fpm php-mysql libapache2-mod-php

    # vi /etc/php/7.0/apache2/php.ini

Изменяем:

    post_max_size = 16M
    max_execution_time = 300
    max_input_time = 300
    date.timezone = Europe/Moscow


    # cp /usr/share/doc/zabbix-frontend-php/examples/zabbix.conf /etc/apache2/conf-available/zabbix.conf

    # a2enconf zabbix.conf

    # service apache2 restart



    # service php7.0-fpm start

    # service zabbix-server restart

    # chown www-data /etc/zabbix



http://localhost/zabbix/

    После инсталляции:

    # chown root /etc/zabbix

login: Admin/zabbix

<br/>

### Изменение локали

    # locale -a
    # dpkg-reconfigure locales
    en_US.UTF-8 UTF-8

    Locales to be generated: 149
    Default locale for the system environment: 3

    # service apache2 restart


<br/>

Configuration --> Hosts --> Enable

<br/>

### Установка Zabbix агента

    # apt-get install -y zabbix-agent
    # vi /etc/zabbix/zabbix_agentd.conf


При необходимости поменять:

    Server=Zabbix.Server.IP.Address
    Hostname=Hostname_Of_Current_Machine

    # service zabbix-agent restart


<br/>

### Включение отправки сообщений на почту:

Сначала нужно настроить smtp или использовать уже настроенный.

    Administration --> Media Types --> Email
    Configuration --> Actions --> Enable
    Profile --> Media --> Add
