---
layout: page
title: Экспорт / Импорты базы Postgres
description: Экспорт / Импорты базы Postgres
keywords: databases, linux, export, import
permalink: /adm/databases/postgresql/export-import/
---

# Экспорт / Импорты базы Postgres

<br/>

### Экспорт базы Postgres

```
// Создание бекапа
$ pg_dump database_name > database_name_20160527.sql
```

<br/>

### Импорт базы

```shell
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
