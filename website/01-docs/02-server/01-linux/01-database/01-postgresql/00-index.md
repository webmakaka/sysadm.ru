---
layout: page
title: PostgreSQL
description: PostgreSQL
keywords: Linux, PostgreSQL
permalink: /server/linux/database/postgresql/
---

# Инсталляция PostgreSQL

<br/>

[Ubuntu](/server/linux/database/postgresql/setup/ubuntu/)  
[Centos](/server/linux/database/postgresql/setup/centos/)

<br/>

### Подключение к базе в командной строке:

<br/>

```
$ psql "host=FQDN_хоста \
      port=5432 \
      sslmode=verify-full \
      dbname=<имя базы данных> \
      user=<имя пользователя базы данных> \
      target_session_attrs=read-write"
```

<br/>

sslmode - если используется ssl

<br/>

[Запуск PgAdmin4 WebClient](/server/linux/database/postgresql/pgadmin/)

[Экспорт / Импорт базы Postgres](/server/linux/database/postgresql/export-import/)

<br/>

[Запросы к базе PostgreSQL](/server/linux/database/postgresql/queries/)

<br/>

### Автоустановщик PostgreSQL в режиме master-slave и standalone:

<a href="https://www.linux.org.ru/news/opensource/15245410">LOR</a> |<a href="https://github.com/Anton-PG/pgsql-for-you">Git</a>

<br/>

### Datagrip (IntellyJ) ошибка при подключении к базе PostgreSQL в Heroku

**[28000] FATAL: no pg_hba.conf entry for host "MY_HOST", user "MY_USER", database "MY_DATABASE", SSL off**

Собственно строка подключения должна быть следующей:

```
jdbc:postgresql://<hostname>:<port>/<database>?sslmode=require
```

т.е. нужно явно добавить ?sslmode=require. Обращаю внимание на последний слеш в строке. Его не должно быть. Я минут 40 искал решение и поэтому решил записать, может кому еще понадобится.

<br/>

![no pg_hba.conf entry for host](/img/server/linux/database/postgresql/datagrip-postgresql-heroku.png 'no pg_hba.conf entry for host'){: .center-image }
