---
layout: page
title: PostgreSQL
permalink: /linux/servers/databases/postgresql/
---

# Инсталляция PostgreSQL 

[Ubuntu](/linux/servers/databases/postgresql/install/ubuntu/)  
[Centos](/linux/servers/databases/postgresql/install/centos/)  


<br/>

### Datagrip (IntellyJ) ошибка при подключении к базе PostgreSQL в Heroku

**[28000] FATAL: no pg_hba.conf entry for host "MY_HOST", user "MY_USER", database "MY_DATABASE", SSL off**

Собственно строка подключения должна быть следующей:

    jdbc:postgresql://<hostname>:<port>/<database>?sslmode=require

т.е. нужно явно добавить ?sslmode=require. Обращаю внимание на последний слеш в строке. Его не должно быть. Я минут 40 искал решение и поэтому решил записать, может кому еще понадобится.

<br/>

<div align="center">

    <img src="//files.sysadm.ru/img/linux/databases/postgresql/datagrip-postgresql-heroku.png" border="0" alt="no pg_hba.conf entry for host">  

</div>

