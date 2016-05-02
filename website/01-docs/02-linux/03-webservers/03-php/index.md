---
layout: page
title: Инсталляция PHP на Centos 6.6 из пакетов
permalink: /linux/webservers/apache/php/
---

=================

Если нужно установить php 5.5 и выше из пакетов:
http://snippets.khromov.se/update-php-5-5-from-remi-repo-on-centos-6/


    # yum --enablerepo=remi-php55,remi update -y php\*

=================

    # yum install -y php.x86_64

    # yum install -y php-pdo.x86_64

    # yum install -y php-mysql

    # cp /etc/php.ini /etc/php.ini.orig.$(date +%Y-%m-%d)

    # find / -name *pdo*.so
    /usr/lib64/php/modules/pdo_mysql.so
    /usr/lib64/php/modules/pdo_sqlite.so
    /usr/lib64/php/modules/pdo.so
