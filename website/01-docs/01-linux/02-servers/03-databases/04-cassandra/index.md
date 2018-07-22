---
layout: page
title: Инсталляция Apache Cassandra (DataStax Community Edition) в Centos 6.7
permalink: /linux/servers/databases/cassandra/centos/6.7/
---


# Инсталляция Apache Cassandra (DataStax Community Edition) в Centos 6.7

## Устанавливаю первый раз!

http://www.planetcassandra.org/cassandra/

http://docs.datastax.com/en/cassandra/2.1/cassandra/install/installRHEL_t.html


<br/>

JDK 7.25 и выше должно быть установлено. (впрочем это не значит, что JDK8 подойдет, поэтому устанавливаю JDK7)

https://javadev.org/development-tools/jdk7/installation/centos/6/


    # vi /etc/yum.repos.d/datastax.repo

<br/>

    [datastax]
    name = DataStax Repo for Apache Cassandra
    baseurl = http://rpm.datastax.com/community
    enabled = 1
    gpgcheck = 0

<br/>

    # yum install -y \
    dsc21-2.1.5-1  \
    cassandra2.1.5-1

<br/>

    # yum install -y cassandra21-tools-2.1.5-1

<br/>

    # chkconfig cassandra on
    # service cassandra start

<br/>

### Пробуем

Выполняю команду под рутом, под обычным пользователем ошибки.

    # cassandra

<br/>

    $ cqlsh

<br/>


    cqlsh> create KEYSPACE testdb WITH REPLICATION = {'class':'SimpleStrategy', 'replication_factor':3};

    cqlsh> describe KEYSPACES;

    cqlsh> USE testdb;

    cqlsh:testdb> CREATE TABLE users(id uuid, name text, email text, username text, PRIMARY KEY(id, name, email, username));

    cqlsh:testdb> DESCRIBE TABLE USERS;

    cqlsh:testdb> INSERT INTO users(id, name, email, username) VALUES(now(), 'Brad Traversy', 'techguyinfo@gmail.com', 'techguy');

    cqlsh:testdb> Select name from users;
