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
$ psql

{% endhighlight %}
