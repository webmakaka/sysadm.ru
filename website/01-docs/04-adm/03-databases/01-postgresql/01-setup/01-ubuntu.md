---
layout: page
title: PostgreSQL инсталляция в Ubuntu
description: PostgreSQL
keywords: databases, linux, postgresql ubuntu, инсталляция
permalink: /adm/databases/postgresql/setup/ubuntu/
---

# PostgreSQL инсталляция в Ubuntu

<br/>

https://www.postgresql.org/download/linux/ubuntu/

<br/>

**Делаю:**  
19.12.2022

```
$ PG_VERSION=9.6

# Create the file repository configuration:
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'

# Import the repository signing key:
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -

# Update the package lists:
sudo apt-get update

# Install the latest version of PostgreSQL.
# If you want a specific version, use 'postgresql-12' or similar instead of 'postgresql':
sudo apt-get -y install postgresql
```

<br/>

```
# apt-get install -y postgresql-$PG_VERSION postgresql-server-dev-$PG_VERSION

# service postgresql restart
# service postgresql status
# systemctl enable postgresql

```

<br/>

### Если нужно установить только psql клиент

```
$ sudo apt-get -y install postgresql-client-15
$ psql --version
psql (PostgreSQL) 15.1 (Ubuntu 15.1-1.pgdg20.04+1)
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
