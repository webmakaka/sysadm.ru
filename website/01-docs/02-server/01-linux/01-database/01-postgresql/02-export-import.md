---
layout: page
title: Экспорт / Импорт базы Postgres
description: Экспорт / Импорт базы Postgres
keywords: server, linux, database, postgresql, export, import
permalink: /server/linux/database/postgresql/export-import/
---

# Экспорт / Импорт базы Postgres

<br/>

Делаю!  
2023.12.19

<br/>

```
$ psql --version
psql (PostgreSQL) 14.10 (Ubuntu 14.10-0ubuntu0.22.04.1)

$ pg_dump --version
pg_dump (PostgreSQL) 14.10 (Ubuntu 14.10-0ubuntu0.22.04.1)
```

<br/>

### pg_dump - снять дамп БД

```
// Если удаленный хост
// Значение в  <>  меняем на актуальные. После запуска утилиты будет запрошен пароль пользователя для доступа к СУБД.
$ pg_dump --host=<ServerName> --port=<5432> --dbname=<DBName> --username=<postgres> --format=c --file=<FullDumpName>
```

<br/>

### pg_restore - поднять дамп БД

```
// Если удаленный хост
//  Значение в  <>  меняем на актуальные. После запуска утилиты будет запрошен пароль пользователя для доступа к СУБД
$ pg_restore --host=<ServerName> --port=<5432> --dbname=<DBName> --username=<postgres> --format=c <FullDumpName>
```

<br/>

```
Если нужно покопать дамп, --format=c не нужно указывать
```

<br/>

### pg_dumpAll - снять полный дамп сервера с одномоментной упаковкой в архив GZip (Никогда не приходилось делатьg)

При использовании данного решения на сервере куда будет поднят данный дамп ВСЕ БД будут обновлены.

Здесь приведен пример снятия полного дампа СУБД с копированием его на другую машину средствами Linux и последующим подъемом.

<br/>

```
// снятие дампа в архив
$ pg_dumpall -U postgres | gzip > /data/dump/dumpall.gz

// копирование дампа между серверами Linux, команду выполнять на сервере источнике дампа
$ scp /data/dump/dumpall.gz postgres@releasepg.testnet.local:/data/dump/dumpall.gz

// подъем дампа без предварительной распаковки
$ zcat /data/dump/dumpall.gz | psql -U postgres postgres
```

<br/>

## Предыдущая дока

<br/>

### Экспорт базы Postgres

```
// Создание бекапа
$ pg_dump database_name > database_name_20160527.sql


// Если удаленный хост
// Значение в  <>  меняем на актуальные. После запуска утилиты будет запрошен пароль пользователя для доступа к СУБД.
$ pg_dump --host=<ServerName> --port=<5432> --dbname=<DBName> --username=<postgres> --format=c --file=<FullDumpName>
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

### Миграция схемы

```
// Export
$ pg_dump --host=<DataBaseHostName> --dbname=<DataBaseName> --schema=<SCHEMA_NAME> --username=<DataBaseUserName> > ~/tmp/<SCHEMA_NAME>.dmp
```

<br/>

```
// Import
$ psql --host=<DataBaseHostName> --dbname=<DataBaseName> --username=<DataBaseUserName> < ~/tmp/<SCHEMA_NAME>.dmp
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
