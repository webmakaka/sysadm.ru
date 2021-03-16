---
layout: page
title: PostgreSQL инсталляция в Ubuntu
description: PostgreSQL
keywords: databases, linux, postgresql ubuntu, инсталляция
permalink: /databases/postgresql/setup/ubuntu/
---

# PostgreSQL инсталляция в Ubuntu

**Делаю:**  
19.08.2018

```
$ sudo su -

$ PG_VERSION=9.6

-- bionic
# echo 'deb http://apt.postgresql.org/pub/repos/apt/ bionic-pgdg main' > /etc/apt/sources.list.d/pgdg.list

-- xenial
# echo 'deb http://apt.postgresql.org/pub/repos/apt/ xenial-pgdg main' > /etc/apt/sources.list.d/pgdg.list

-- trusty
# echo 'deb http://apt.postgresql.org/pub/repos/apt/ trusty-pgdg main' > /etc/apt/sources.list.d/pgdg.list

# wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add -

# apt-get update
# apt-get install -y postgresql-$PG_VERSION postgresql-server-dev-$PG_VERSION

# service postgresql restart
# service postgresql status
# systemctl enable postgresql

```

<br/>

Если нужно установить только psql клиент

```
$ sudo apt-get install -y postgresql-client
$ psql --version
PostgreSQL) 10.12 (Ubuntu 10.12-0ubuntu0.18.04.1)
```

<br/>

### Подключение к базе

```
# su - postgres

\$ psql

postgres-# \dx
List of installed extensions
Name | Version | Schema | Description
---------+---------+------------+------------------------------
plpgsql | 1.0 | pg_catalog | PL/pgSQL procedural language
(1 row)
```

<br/>

### Добавление расширения jsonbx-1.0.0

```
# apt install -y gcc make
# cd /home/marley/Desktop/jsonbx-1.0.0
# make

# pg_config --pkglibdir
/usr/lib/postgresql/9.6/lib

# cp jsonbx.o /usr/lib/postgresql/9.6/lib
# cp jsonbx.so /usr/lib/postgresql/9.6/lib

# cp jsonbx.control /usr/share/postgresql/9.6/extension/
# cp jsonbx--1.0.sql /usr/share/postgresql/9.6/extension/


$ psql
psql (9.6.9)
Type "help" for help.

postgres=# CREATE EXTENSION IF NOT EXISTS jsonbx;
CREATE EXTENSION
postgres=#
postgres=#
postgres=# \dx
                    List of installed extensions
    Name   | Version |   Schema   |         Description
---------+---------+------------+------------------------------
    jsonbx  | 1.0     | public     | Jsonb extension
    plpgsql | 1.0     | pg_catalog | PL/pgSQL procedural language
(2 rows)
```

<br/>

### Импорт базы

Разные способы имеются

```shell

// Создание бекапа
$ pg_dump database_name > database_name_20160527.sql

// Восстановление

$ su - postgres
$ psql

-- Если база не создана
$ CREATE DATABASE <db_name>;

exit

$ psql database_name < database_name_20160527.sql
```

<br/>

https://www.netguru.co/tips/how-to-dump-and-restore-postgresql-database

<br/>

ЕЩе есть такой вариант:

```shell

$ cd <db_dump_dir>

-- Данные будут перезаписаны
$ pg_restore -d <db_name> myDump.dump -j 4 -c
```

<!--
<br/>

```

vi /etc/postgresql/9.6/main/pg_hba.conf
local   all             postgres                                peer

here change peer to trust

restart, sudo service postgresql restart

now try, psql -U postgres


```

<br/>


Было полезным:

https://wiki.postgresql.org/wiki/Apt -->
