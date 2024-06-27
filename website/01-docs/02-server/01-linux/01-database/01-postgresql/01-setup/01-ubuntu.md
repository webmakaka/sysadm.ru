---
layout: page
title: PostgreSQL инсталляция в Ubuntu
description: PostgreSQL
keywords: server, linux, database, postgresql, setup, ubuntu
permalink: /server/linux/database/postgresql/setup/ubuntu/
---

# PostgreSQL инсталляция в Ubuntu

<br/>

https://www.postgresql.org/download/linux/ubuntu/

<br/>

```
// Нужно будет подумать, как улучшить перед следующей установкой когда-нибудь в будущем
W: https://apt.postgresql.org/pub/repos/apt/dists/jammy-pgdg/InRelease: Key is stored in legacy trusted.gpg keyring (/etc/apt/trusted.gpg), see the DEPRECATION section in apt-key(8) for details.
N: Skipping acquire of configured file 'main/binary-i386/Packages' as repository 'https://apt.postgresql.org/pub/repos/apt jammy-pgdg InRelease' doesn't support architecture 'i386'
```

<br/>

**Делаю:**  
2024.06.12

```
// Create the file repository configuration:
$ sudo sh -c 'echo "deb https://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'

// Import the repository signing key:
$ wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -

// Update the package lists
$ sudo apt-get update
```

<br/>

```
// Install the latest version of PostgreSQL.
// If you want a specific version, use 'postgresql-12' or similar instead of 'postgresql':
$ sudo apt-get -y install postgresql
```

<br/>

```
$ PG_VERSION=9.6
$ sudo apt-get install -y postgresql-$PG_VERSION postgresql-server-dev-$PG_VERSION

$ sudo service postgresql restart
$ sudo service postgresql status
$ sudo systemctl enable postgresql
```

<br/>

### Если нужно установить только psql клиент

Репо должно быть подключено.

```
$ apt-cache search postgresql-client
$ sudo apt install -y postgresql-client-15
$ psql --version
psql (PostgreSQL) 15.7 (Ubuntu 15.7-1.pgdg22.04+1)
```

<br/>

### UPD. Если ошибка (Была в mint), что-то вроде:

```
E: The repository 'https://apt.postgresql.org/pub/repos/apt una-pgdg Release' does not have a Release file.
```

<br/>

```
$ sudo vi /etc/apt/sources.list.d/pgdg.list
```

```
// В моем случае нужна была focal
deb https://apt.postgresql.org/pub/repos/apt focal-pgdg main
```

<br/>

```
$ cat /etc/os-release
NAME="Linux Mint"
VERSION="20.3 (Una)"
ID=linuxmint
ID_LIKE=ubuntu
PRETTY_NAME="Linux Mint 20.3"
VERSION_ID="20.3"
```

<br/>

### Для Astra Linux 1.8 и Debian 12 (Bookworm)

```
$ sudo sh -c 'echo "deb https://apt.postgresql.org/pub/repos/apt bookworm-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
```

<br/>

### Подключение к базе

```
# su - postgres

$ psql

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
