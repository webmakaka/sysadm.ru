---
layout: page
title: PostgreSQL инсталляция в Ubuntu
description: PostgreSQL
keywords: databases, linux, postgresql ubuntu, инсталляция
permalink: /adm/databases/postgresql/setup/ubuntu/
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

### Если нужно установить только psql клиент

```
$ sudo apt-get install -y postgresql-client
$ psql --version
psql (PostgreSQL) 12.2 (Ubuntu 12.2-4)
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
