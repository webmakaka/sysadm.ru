---
layout: page
title: PostgreSQL
description: PostgreSQL
keywords: Linux, PostgreSQL
permalink: /adm/databases/postgresql/
---

# Инсталляция PostgreSQL

<br/>

[Ubuntu](/adm/databases/postgresql/setup/ubuntu/)  
[Centos](/adm/databases/postgresql/setup/centos/)

<br/>

[Запуск PgAdmin4 WebClient в docker контейнере](/adm/databases/postgresql/pgadmin/)

[Экспорт / Импорты базы Postgres](/adm/databases/postgresql/export-import/)

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

![no pg_hba.conf entry for host](/img/adm/databases/postgresql/datagrip-postgresql-heroku.png 'no pg_hba.conf entry for host'){: .center-image }
