---
layout: page
title: Настройка виртуальных хостов
permalink: /linux/webservers/apache/virtual-hosts/
---

# Настройка виртуальных хостов

    $ cp /etc/httpd/conf/httpd.conf /etc/httpd/conf/httpd.conf.backup.$(date +%Y-%m-%d)

<br/>


    $ mkdir /etc/httpd/conf/websites/_IT/
    $ vi /etc/httpd/conf/httpd.conf

<br/>

    ###########################################################
    ######### VIRTUAL HOSTS #################################
    ###########################################################

    ### IT

    include conf/websites/_IT/sysadm.ru.conf

<br/>

    $ vi /opt/httpd/2.4.3/conf/websites/_IT/sysadm.ru.conf

<br/>

    <VirtualHost *:80>
        ServerName sysadm.ru
        ServerAlias www.sysadm.ru
        DocumentRoot /u01/webProjects/_IT/sysadm.ru
        ErrorLog /u01/logs/_IT/sysadm.ru/sysadm.ru-error.log
        CustomLog /u01/logs/_IT/sysadm.ru/sysadm.ru-access.log combined

        <Directory "/u01/webProjects/_IT/sysadm.ru">
            Options All
            AllowOverride All
            Require all granted
        </Directory>

    </VirtualHost>


При ошибке:

    configuration error:  couldn't perform authentication. AuthType not set

Можно закомментировать

    # Require all granted
