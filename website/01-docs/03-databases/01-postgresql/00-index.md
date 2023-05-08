---
layout: page
title: PostgreSQL
description: PostgreSQL
keywords: Linux, PostgreSQL
permalink: /databases/postgresql/
---

# Инсталляция PostgreSQL

<br/>

[Ubuntu](/databases/postgresql/setup/ubuntu/)  
[Centos](/databases/postgresql/setup/centos/)

<br/>

### Подключение к базе в командной строке:

```
$ psql "host=FQDN_хоста \
      port=6432 \
      sslmode=verify-full \
      dbname=<имя базы данных> \
      user=<имя пользователя базы данных> \
      target_session_attrs=read-write"
```

<br/>

sslmode - если используется ssl

<br/>

[Запуск PgAdmin4 WebClient в docker контейнере](/databases/postgresql/pgadmin/)

[Экспорт / Импорты базы Postgres](/databases/postgresql/export-import/)

<br/>

[Запросы к базе PostgreSQL](/databases/postgresql/queries/)

<br/>

### Автоустановщик PostgreSQL в режиме master-slave и standalone:

<a href="https://www.linux.org.ru/news/opensource/15245410">LOR</a> |<a href="https://github.com/Anton-PG/pgsql-for-you">Git</a>

<br/>

### Datagrip (IntellyJ) ошибка при подключении к базе PostgreSQL в Heroku

**[28000] FATAL: no pg_hba.conf entry for host "MY_HOST", user "MY_USER", database "MY_DATABASE", SSL off**

Собственно строка подключения должна быть следующей:

    jdbc:postgresql://<hostname>:<port>/<database>?sslmode=require

т.е. нужно явно добавить ?sslmode=require. Обращаю внимание на последний слеш в строке. Его не должно быть. Я минут 40 искал решение и поэтому решил записать, может кому еще понадобится.

<br/>

![no pg_hba.conf entry for host](/img/databases/postgresql/datagrip-postgresql-heroku.png 'no pg_hba.conf entry for host'){: .center-image }
