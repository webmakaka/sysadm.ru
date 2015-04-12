---
layout: page
title: PostgreSQL инсталляция в Centos 6.X
permalink: /linux/databases/postgresql/centos/
---


{% highlight text %}

sudo aptutude search postgresql

sudo apt-add-repository ppa:pitti/postgresql
sudo apt-get update -y
sudo apt-get install -y postgresql-9.2 libpq-dev

sudo service postgresql status


sudo su -c psql postgres

{% endhighlight %}
