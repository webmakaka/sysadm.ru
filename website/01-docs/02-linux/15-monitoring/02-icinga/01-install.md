---
layout: page
title: Инсталляция Icinga в Ubuntu Linux (Xenial)
permalink: /linux/monitoring/icinga/install/
---

### Инсталляция Icinga в Ubuntu Linux (Xenial)

<br/>

https://docs.icinga.com/icinga2/latest/doc/module/icinga2/chapter/getting-started#!/icinga2/latest/doc/module/icinga2/chapter/getting-started#setting-up-icinga2

https://github.com/Icinga/icingaweb2/blob/master/doc/02-Installation.md

<br/>


    # addgroup --system icingaweb2
    # usermod -a -G icingaweb2 www-data

<br/>

    # apt-get install -y software-properties-common

    # add-apt-repository -y ppa:formorer/icinga

    # apt-get update

    # apt-get install -y icinga2

<br/>

    # cd /tmp/
    # wget https://www.monitoring-plugins.org/download/monitoring-plugins-2.2.tar.gz
    # tar -zxvf monitoring-plugins-2.2.tar.gz
    # cd monitoring-plugins-2.2

    # ./configure
    # make && make install


<br/>

### MySQL

    # apt-get install -y mysql-server mysql-client
    # service mysql restart

    # mysql -u root -p

    CREATE DATABASE icinga2;

    GRANT SELECT, INSERT, UPDATE, DELETE, DROP, CREATE VIEW, INDEX, EXECUTE ON icinga2.* TO 'icinga2'@'localhost' IDENTIFIED BY 'icinga2';


<br/>

    CREATE DATABASE icingaweb2;

    GRANT SELECT, INSERT, UPDATE, DELETE, DROP, CREATE VIEW, INDEX, EXECUTE ON icingaweb2.* TO 'icingaweb2'@'localhost' IDENTIFIED BY 'icingaweb2';

    quit

<!-- <br/>

    mysql -p icingaweb2

    INSERT INTO icingaweb_user (name, active, password_hash) VALUES ('icingaadmin', 1, '$1$EzxLOFDr$giVx3bGhVm4lDUAw6srGX1');
    quit -->


<br/>

    # vi /etc/icingaweb2/resources.ini


{% highlight text %}

[icingaweb2]
type                = "db"
db                  = "mysql"
host                = "localhost"
port                = "3306"
dbname              = "icingaweb2"
username            = "icingaweb2"
password            = "icingaweb2"


[icinga2]
type                = "db"
db                  = "mysql"
host                = "localhost"
port                = "3306"
dbname              = "icinga2"
username            = "icinga2"
password            = "icinga2"

{% endhighlight %}

    <!-- # mysql -u root -p icinga </usr/share/icinga2-ido-mysql/schema/mysql.sql -->
    # mysql -p icinga2 < /usr/share/icinga2-ido-mysql/schema/mysql.sql


Возможно, что она должна здесь быть:

    /usr/share/icingaweb2/etc/schema/mysql.schema.sql

    # mysql -p icingaweb2 < /usr/share/doc/icingaweb2/schema/mysql.schema.sql


icingaweb_group

    https://raw.githubusercontent.com/Icinga/icingaweb2/master/etc/schema/mysql.schema.sql


<br/>

### Apache, PHP

    # apt-get install -y apache2 php libapache2-mod-php
    # apt-get install -y php-gd php-imagick

    # phpenmod gd
    # phpenmod imagick

    # vi /etc/php/7.0/apache2/php.ini

    date.timezone = Europe/Moscow

<br/>

    # service apache2 restart


<br/>

### Настройка icinga

    # apt-get install -y icingaweb2
    # apt-get install -y icinga2-ido-mysql
    Enable Icinga 2's ido-mysql feature? [yes/no] no
    Configure database for icinga2-ido-mysql with dbconfig-common? [yes/no] no

    <!-- # icingacli setup config webserver apache --document-root /usr/share/icingaweb2/public/ -->
    # icingacli setup config webserver apache --document-root /usr/share/icingaweb2/public/ > /etc/apache2/sites-enabled/001-icinga.conf

    # icinga2 feature enable ido-mysql
    # icinga2 feature enable command

    # icingacli setup config directory

    # icingacli setup token create    
    The newly generated setup token is: 7e07c7f0d3ffd2e2

    # service icinga2 restart


<br/>

### Подключение браузером

    localhost/icingaweb2/setup
    вставляю token

<br/>
<br/>
<br/>
<br/>


There is currently no icinga instance writing to the IDO. Make sure that a icinga instance is configured and able to write to the IDO.

http://serverfault.com/questions/743426/there-is-currently-no-icinga-instance-writing-to-the-ido
