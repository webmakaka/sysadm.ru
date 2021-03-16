---
layout: page
title: Инсталляция MySQL на Centos 7 из пакетов
description: Инсталляция MySQL на Centos 7 из пакетов
keywords: Инсталляция MySQL на Centos 7 из пакетов
permalink: /databases/mysql/setup/centos7/
---

# Инсталляция MySQL на Centos 7 из пакетов

    # cd /tmp/
    # wget http://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm

    # rpm -ihv mysql57-community-release-el7-8.noarch.rpm
    # yum install -y mysql-community-server.x86_64

    # mysql --version
    mysql  Ver 14.14 Distrib 5.7.13, for Linux (x86_64) using  EditLine wrapper

    # systemctl start mysqld

    # ps -ef |grep mysql
    mysql     9438     1  4 21:21 ?        00:00:00 /usr/sbin/mysqld --daemonize --pid-file=/var/run/mysqld/mysqld.pid
    root      9467  9260  0 21:22 pts/0    00:00:00 grep --color=auto mysql

<br/>

    # mysql -u root -p
    Enter password:
    ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: NO)

<br/>

Приплыли, блять. Еще ничего не сдалал, а рутом уже не могу подключиться.

<br/>

    # systemctl stop mysqld

<br/>

    # vi /etc/my.cnf

<br/>

В блок

    [mysqld]

Добавляю

    skip-grant-tables

<br/>

    # systemctl start mysqld

<br/>

    # mysql -u root

<br/>

    flush privileges;
    SET PASSWORD FOR 'root'@'localhost' = PASSWORD('root');

<br/>

Повторяю теже действия, чтобы удалить из конфига skip-grant-tables.

<br/>

    # mysql -u root -p
    Enter password:
    Welcome to the MySQL monitor.  Commands end with ; or \g.
    Your MySQL connection id is 3
    Server version: 5.7.13 MySQL Community Server (GPL)

    Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

    Oracle is a registered trademark of Oracle Corporation and/or its
    affiliates. Other names may be trademarks of their respective
    owners.

    Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

<br/><br/>

Может есть вариант попроще? раньше же такого делать не приходилось.

<br/><br/>

    mysql> SHOW VARIABLES LIKE 'validate_password%';
    +--------------------------------------+--------+
    | Variable_name                        | Value  |
    +--------------------------------------+--------+
    | validate_password_dictionary_file    |        |
    | validate_password_length             | 8      |
    | validate_password_mixed_case_count   | 1      |
    | validate_password_number_count       | 1      |
    | validate_password_policy             | MEDIUM |
    | validate_password_special_char_count | 1      |
    +--------------------------------------+--------+
    6 rows in set (0.00 sec)

<br/><br/>

    # SET GLOBAL validate_password_length = 4;
    # SET GLOBAL validate_password_number_count = 0;
    # SET GLOBAL validate_password_mixed_case_count = 0;
    # SET GLOBAL validate_password_special_char_count = 0;
