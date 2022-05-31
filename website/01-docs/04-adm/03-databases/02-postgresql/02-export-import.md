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

Еще есть такой вариант:

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

<br/>

### Экспорт таблицы

```
$ pg_dump --no-owner -d <database_name> -t <table_name> > file.sql
```

<br/>

```
$ pg_dump -h <адрес сервера СУБД> \
    -U <имя пользователя> \
    -p <порт> \
    --no-owner -d <database_name> -t <table_name> > file.sql
```

<br/>

```
$ pg_dump -h <DataBaseName> \
    -U <DataBaseUser> \
    -p 5432 \
    --no-owner -d dqiptf_dqiptf -t iptf_neural_model > iptf_neural_model.sql
```

<br/>

### Миграция схемы

```
// Export
$ pg_dump --host=<DataBaseName> --username=<DataBaseUser> --dbname=<DataBaseName> --schema=<SCHEMA_NAME> > /home/marley/tmp/<SCHEMA_NAME>.dmp
```

<br/>

```
// Import
$ psql --host=<DataBaseName> --username=<DataBaseUser> --dbname=<DataBaseName> < /home/marley/tmp/<SCHEMA_NAME>.dmp
```

<br/>

### Миграция данных в Облако Yandex

https://practicum.yandex.ru/trainer/ycloud/lesson/7fcf670e-67d5-48ef-9456-527ba71e78f7/

<br/>

```
//
// В этой команде при экспорте исключаются все данные, которые связаны с привилегиями и ролями.
$ pg_dump -h <адрес сервера СУБД> \
    -U <имя пользователя> \
    -p <порт> \
    --schema-only \
    --no-privileges \
    --no-subscriptions \
    -d <имя базы данных> -Fd -f /tmp/db_dump
```

<br/>

```
$ pg_restore -Fd -v --single-transaction -s --no-privileges \
          -h <адрес приемника> \
          -U <имя пользователя> \
          -p 6432 \
          -d <имя базы данных> /tmp/db_dump
```
