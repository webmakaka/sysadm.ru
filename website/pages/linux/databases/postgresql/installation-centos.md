---
layout: page
title: PostgreSQL инсталляция в Centos 6.X
permalink: /linux/databases/postgresql/centos/
---

### Для версии 9

1\. Configure your YUM repository

To the section(s) identified above, you need to append a line (otherwise dependencies might resolve to the postgresql supplied by the base repository):

    # vi /etc/yum.repos.d/CentOS-Base.repo

[base] and [updates]

    exclude=postgresql*

2\. Install PGDG RPM file

    # yum localinstall -y  http://yum.postgresql.org/9.4/redhat/rhel-6-x86_64/pgdg-centos94-9.4-1.noarch.rpm


3\. Install PostgreSQL

    # yum list postgres*
    # yum install -y postgresql94-server


4\. Initialize

    # service postgresql-9.4 initdb

5\. Startup

    # chkconfig postgresql-9.4 on

6\. Control service

    # service postgresql-9.4 start


https://wiki.postgresql.org/wiki/YUM_Installation



### Для версии 8


    # yum install -y postgresql-server
    # chkconfig --levels 35 postgresql on

    # service postgresql initdb


### Настройка конфигов

Config:

    # cp /var/lib/pgsql/data/postgresql.conf /var/lib/pgsql/data/postgresql.conf.orig

<br/>

    # vi /var/lib/pgsql/data/postgresql.conf

<br/>

    listen_addresses = '*'
    port = 5432

<br/>

    # cp /var/lib/pgsql/data/pg_hba.conf /var/lib/pgsql/data/pg_hba.conf.orig

<br/>

    # vi /var/lib/pgsql/data/pg_hba.conf

<br/>

    добавляю:  

    host          all           all           172.17.42.0      255.255.255.0         trust

    172.17.42.0 - подсеть из которой можно будет подключаться к серверу PostgreSQL

<br/>

    # service postgresql restart


### Создание базы и пользователя


    # su - postgres

    $ createdb mydatabase
    $ psql mydatabase

    CREATE USER scott WITH PASSWORD 'tiger';

    GRANT ALL PRIVILEGES ON DATABASE mydatabase to scott;


___


Если нужно собрать из исходников более новую версию:
http://odba.ru/showthread.php?t=465  

Вроде неплохая статья:  
http://www.unixmen.com/postgresql-9-4-released-install-centos-7/

______

Ошибка:

    pg_restore: [archiver] unsupported version (1.12) in file header

Проверяем:

    pg_restore --version
    pg_restore (PostgreSQL) 8.4.20

Нужна версия 9 и выше.

    $ pg_restore --version
    pg_restore (PostgreSQL) 9.4.1


______

### Импорт / Экспорт

Dump (Нужно уточнить параметры)

    pg_dump -f nvdls.db -F p -U nvdladmin nvdlstats

Restore

    pg_restore --verbose --clean --no-acl --no-owner -h myhost -U myuser -d mydb latest.dump


______


Dump Можно преобразовать в sql (Нужно уточнить параметры)

    pg_restore -Fc dump.backup -f dump.sql

И импортировать как последовательность SQL команд.

    $ psql database -f dump.sql


_______

Ошибка:
[archiver (db)] could not execute query: ERROR:  language "plpgsql" does not exist

    $ psql mydatabase
    CREATE LANGUAGE plpgsql;


http://softlabpro.blogspot.ru/2011/05/postgresql-restore-9x-backup-in-8x.html
