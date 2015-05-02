---
layout: page
title: PostgreSQL инсталляция в Centos 6.X
permalink: /linux/databases/postgresql/centos/
---

Делаю для контенера docker

{% highlight text %}

# yum install -y postgresql-server
# chkconfig --levels 35 postgresql on

# service postgresql initdb


Config:
# cp /var/lib/pgsql/data/postgresql.conf /var/lib/pgsql/data/postgresql.conf.orig
# vi /var/lib/pgsql/data/postgresql.conf

listen_addresses = '*'
port = 5432


# cp /var/lib/pgsql/data/pg_hba.conf /var/lib/pgsql/data/pg_hba.conf.orig


# vi /var/lib/pgsql/data/pg_hba.conf

добавляю:  

host          all           all           172.17.42.0      255.255.255.0         trust

172.17.42.0 - подсеть присвоенная виртуальному адаптеру docker

# service postgresql restart

# su - postgres

$ createdb mydatabase
$ psql mydatabase

CREATE USER scott WITH PASSWORD 'tiger';

GRANT ALL PRIVILEGES ON DATABASE mydatabase to scott;


{% endhighlight %}

Если из исходников более новую версию:
http://odba.ru/showthread.php?t=465

Вроде неплохая статья:  
http://www.unixmen.com/postgresql-9-4-released-install-centos-7/
