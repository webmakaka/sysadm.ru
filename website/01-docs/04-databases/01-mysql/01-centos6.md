---
layout: page
title: Инсталляция MySQL в Centos 6.X
permalink: /databases/mysql/installation/
---

# [Вариант 1] Инсталляция MySQL на Centos 6.5 из пакетов (Вариант с установкой более новой версии видится предпочтительнее)

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


<br/><br/>

### [Вариант 2] Из репо MySQL:

Первый раз делаю этим способом!
Сначала поставил версию из репо по умолчанию. Потом программа, которая должна работать с базой, сказала, что с таким старьем она встречаться не хочет, ей бы чего помоложе.

https://repo.mysql.com/yum/

    # rpm -Uvh https://repo.mysql.com/yum/mysql-5.7-community/el/6/x86_64/mysql57-community-release-el6-7.noarch.rpm

    # yum install -y mysql mysql-server

<br/>

    # service mysqld start

<br/>

    Так у меня ошибка

<br/>

    # less /var/log/mysqld.log

<br/>

    [ERROR] InnoDB: The Auto-extending innodb_system data file './ibdata1' is of a different size 640 pages (rounded down to MB) than specified in the .cnf file: initial 768 pages, max 0 (relevant if non-zero) pages!
    2016-06-20T11:13:04.445520Z 0 [ERROR] InnoDB: Plugin initialization aborted with error Generic error
    2016-06-20T11:13:05.050047Z 0 [ERROR] Plugin 'InnoDB' init function returned error.
    2016-06-20T11:13:05.050108Z 0 [ERROR] Plugin 'InnoDB' registration as a STORAGE ENGINE failed.

<br/>

    # vi /etc/my.cnf

<br/>

     [mysqld] block

    добавляю:

    innodb_data_file_path = ibdata1:10M:autoextend

<br/>

    # cat /dev/null > /var/log/mysqld.log

<br/>

    # service mysqld restart

<br/>

хуяк

    Fatal error: mysql.user table is damaged. Please run mysql_upgrade.

Ну нах....

    # yum remove -y mysql mysql-server
    # rm -rf /var/lib/mysql/
    # yum install -y mysql mysql-server
    # service mysqld start

<br/>

Узнать сгенерированный пароль.

    # grep 'temporary password' /var/log/mysqld.log
    2016-06-20T11:48:16.167483Z 1 [Note] A temporary password is generated for root@localhost: 6SS6G:ila35!

Поменять пароль можно командой (тулза настолько назойлива, что не закрывается ни по ctrl + C, ни по ctrl + D). Но я бы так делать не рекомендовал:

    # mysql_secure_installation


Логинюсь с этим хреновым паролем.

    # mysql -u root -p

<br/>

    # SET GLOBAL validate_password_length = 4;
    # SET GLOBAL validate_password_number_count = 0;
    # SET GLOBAL validate_password_mixed_case_count = 0;
    # SET GLOBAL validate_password_special_char_count = 0;

<br/>

    flush privileges;
    SET PASSWORD FOR 'root'@'localhost' = PASSWORD('root');

Ура, я могу подкючаться к mysql как root/root.
Вышли на победный!



<br/>

### Инсталляция более поздних версий базы из пакетов (не из официального репо) (Устарело как гавно мамонта. Кому сейчас нужен mysql 5.5 ???)

Если нужно установить версию 5.5, а ее нет в репозитории по умолчанию, можно сделать следующее:

    # rpm -Uvh http://mirror.webtatic.com/yum/el6/latest.rpm
    # yum install -y mysql55w mysql55w-server


Удалить если что:

    # rm -rf /etc/yum.repos.d/webtatic.repo

Подробнее  
https://webtatic.com/packages/mysql55/
