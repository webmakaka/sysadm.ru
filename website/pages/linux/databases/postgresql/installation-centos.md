---
layout: page
title: PostgreSQL инсталляция в Centos 6.X
permalink: /linux/databases/postgresql/centos/
---


{% highlight text %}

# yum install -y postgresql-server
# chkconfig --levels 35 postgresql on

# service postgresql initdb

# service postgresql restart

# su - postgres

$ createdb mydatabase
$ psql mydatabase

CREATE USER scott WITH PASSWORD 'tiger';

GRANT ALL PRIVILEGES ON DATABASE mydatabase to scott;


Config:
vi /var/lib/pgsql/data/postgresql.conf

listen_addresses = '192.168.1.2'
port = 5432


vi /u02/pgdata/pg_hba.conf

{% endhighlight %}

Вроде неплохая статья:  
http://www.unixmen.com/postgresql-9-4-released-install-centos-7/
