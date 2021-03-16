---
layout: page
title: Инсталляция Percona XtraDB в centos
description: Инсталляция Percona XtraDB в centos
keywords: Инсталляция Percona XtraDB в centos
permalink: /adm/databases/percona/install/centos/
---

# Инсталляция Percona XtraDB в centos

<br/>

Делаю  
12.04.2019

По материалам индуса:

https://www.youtube.com/watch?v=wIEj3FLAX8M&list=PL34sAs7_26wOH9oppgwMtb5sfUSNaeLio&index=1

<br/>

<br/>

### Поехали

<br/>

https://www.percona.com/doc/percona-repo-config/percona-release.html#rpm-based-gnu-linux-distributions

<br/>

### На хост машине запускаем виртуалки

<br/>

    $ vagrant plugin install vagrant-hostmanager

<br/>

    $ mkdir ~/vagrant-percona && cd ~/vagrant-percona

    # git clone https://bitbucket.org/sysadm-ru/vagrant .

    $ cd vagrantfiles/centos7

    $ vi Vagrantfile

Ставим NodeCount = 2

    $ vagrant up

    $ vagrant status
    Current machine states:

    centosvm01                running (virtualbox)
    centosvm02                running (virtualbox)

<br/>

### Установка и настройка

Делаем по инструкции

https://bitbucket.org/sysadm-ru/percona/src/master/INSTALL_CentOS7.md

<br/>

### Проверка

Делаю на centosvm01

    # mysql -uroot -p

<br/>

    sql> show status like '%wsrep%';
    ***
    | wsrep_cluster_size               | 2
    ***

<br/>

    mysql> create database dummy;
    mysql> use dummy
    mysql> create table demo (name varchar(10), sex varchar(1), primary key (name));
    mysql> insert into demo values ('Venkat', 'M');

<br/>

Проверяю на centosvm02

    # mysql -uroot -p
    mysql> use dummy
    select * from demo;

Все ОК!
