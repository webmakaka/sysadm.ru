---
layout: page
title: PostgreSQL инсталляция в Centos 6.X
permalink: /linux/databases/postgresql/centos/
---

### Для версии 9

1\. Configure your YUM repository

Для начала нужно указать в репозиториях [base] и [updates], что из-них следует исключить пакеты для postgresql:

    # vi /etc/yum.repos.d/CentOS-Base.repo

Для [base] и [updates]

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


### Настройка конфигов (расположение зависит от выбранной версии postgresql)

Config:

    # cp /var/lib/pgsql/data/postgresql.conf /var/lib/pgsql/data/postgresql.conf.backup

<br/>

    # vi /var/lib/pgsql/data/postgresql.conf

<br/>

    listen_addresses = '*'
    port = 5432

<br/>

    # cp /var/lib/pgsql/data/pg_hba.conf /var/lib/pgsql/data/pg_hba.conf.backup

<br/>

    # vi /var/lib/pgsql/data/pg_hba.conf

<br/>

    добавляю:  

    host          all           all           192.168.56.0      255.255.255.0         trust

    192.168.56.0 - подсеть из которой можно будет подключаться к серверу PostgreSQL

<br/>

    # service postgresql restart


### Создание базы и пользователя


    # su - postgres

    $ createdb mydatabase
    $ psql mydatabase

    CREATE USER scott WITH PASSWORD 'tiger';

    GRANT ALL PRIVILEGES ON DATABASE mydatabase to scott;


// Удалить базу (если нужно), можно командой

    $ dropdb mydatabase

// Поменять владельца

    ALTER DATABASE mydatabase OWNER TO scott;

// Проверка удаленного подключения

    $ psql -h 192.168.1.11 -p 5432 -U user -W mydatabase

___


Если нужно собрать из исходников более новую версию:
http://odba.ru/showthread.php?t=465  

Вроде неплохая статья:  
http://www.unixmen.com/postgresql-9-4-released-install-centos-7/


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


______

### Ошибка 1:

    pg_restore: [archiver] unsupported version (1.12) in file header

Проверяем:

    pg_restore --version
    pg_restore (PostgreSQL) 8.4.20

Нужна версия 9 и выше.

    $ pg_restore --version
    pg_restore (PostgreSQL) 9.4.1
___


### Ошибка 2:

[archiver (db)] could not execute query: ERROR:  language "plpgsql" does not exist

    $ psql mydatabase
    CREATE LANGUAGE plpgsql;


http://softlabpro.blogspot.ru/2011/05/postgresql-restore-9x-backup-in-8x.html




### Ошибка 3:


ERROR: could not execute query: ERROR:  must be owner of language plpgsql

    $ psql paymentgate
    paymentgate=# alter role <user_name> with superuser;
    ALTER ROLE


Убедиться, что owner выставлен правильно.

    postgres=# \l
                                       List of databases
        Name     |  Owner   | Encoding |   Collate   |    Ctype    |   Access privileges
    -------------+----------+----------+-------------+-------------+-----------------------
     paymentgate | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =Tc/postgres         +
                 |          |          |             |             | postgres=CTc/postgres+
                 |          |          |             |             | test=CTc/postgres
     postgres    | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 |
     template0   | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
                 |          |          |             |             | postgres=CTc/postgres
     template1   | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
                 |          |          |             |             | postgres=CTc/postgres
    (4 rows)


<!--

REAL
==========================


$ createdb paymentgate

$ psql

# ALTER DATABASE paymentgate OWNER TO test;

postgres=# \l

exit

psql paymentgate

paymentgate=# alter role test with superuser;
ALTER ROLE

-->
