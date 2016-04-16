---
layout: page
title: Инсталляция MySQL в Centos 6.X
permalink: /linux/databases/mysql/installation/
---



## Инсталляция MySQL на Centos 6.5 из пакетов


Посмотреть, че там в репо лежит:

    # yum search mysql

<br/>

    // Install mysql mysql-server:
    # yum install -y mysql mysql-server

<br/>

    # mysql --version
    mysql  Ver 14.14 Distrib 5.5.38, for Linux (x86_64) using readline 5.1

<br/>

    # chkconfig --level 2345 mysqld on

<br/>

    // Start MySQL server daemon (mysqld)
    # service mysqld start


<br/>

### Инсталляция более поздних версий базы из пакетов (не из официального репо)

Если нужно установить версию 5.5, а ее нет в репозитории по умолчанию, можно сделать следующее:

    # rpm -Uvh http://mirror.webtatic.com/yum/el6/latest.rpm
    # yum install -y mysql55w mysql55w-server


Подробнее  
https://webtatic.com/packages/mysql55/
