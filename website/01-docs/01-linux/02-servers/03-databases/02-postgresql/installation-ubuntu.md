---
layout: page
title: PostgreSQL инсталляция в Ubuntu
permalink: /linux/servers/databases/postgresql/ubuntu/
---

# PostgreSQL инсталляция в Ubuntu

Делаю:  

01.08.2018


Нужна версия именно postgresql-9.6. 


    # apt-cache search postgresql-9.6



нету....


```

# vi /etc/apt/sources.list.d/pgdg.list

deb http://apt.postgresql.org/pub/repos/apt/ xenial-pgdg main


# apt-get install -y wget ca-certificates
# wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add -
# apt-get update
# apt-get upgrade -y


# apt-get install -y postgresql-9.6


# service postgresql start
# service postgresql status

# /usr/lib/postgresql/9.6/bin/pg_ctl --version
pg_ctl (PostgreSQL) 9.6.9


```

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

https://wiki.postgresql.org/wiki/Apt


<br/>

### DEV сервер


    # apt-get install -y postgresql-server-dev-9.6

    -- Показать пути к расширениям
    $ pg_config  --pkglibdir




<br/>



# \dx
                 List of installed extensions
  Name   | Version |   Schema   |         Description          
---------+---------+------------+------------------------------
 plpgsql | 1.0     | pg_catalog | PL/pgSQL procedural language
(1 row)
