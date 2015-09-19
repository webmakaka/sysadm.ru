---
layout: page
title: PostgreSQL инсталляция в Ubuntu
permalink: /linux/databases/postgresql/ubuntu/
---


{% highlight text %}

sudo aptutude search postgresql

sudo apt-add-repository ppa:pitti/postgresql
sudo apt-get update -y
sudo apt-get install -y postgresql-9.2 libpq-dev

sudo service postgresql status


sudo su -c psql postgres

{% endhighlight %}
