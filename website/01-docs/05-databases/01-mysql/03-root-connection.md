---
layout: page
title: Удаленный доступ к базе rootом с правами суперпользователя
description: Удаленный доступ к базе rootом с правами суперпользователя
keywords: Удаленный доступ к базе rootом с правами суперпользователя
permalink: /databases/mysql/root-connection/
---

# Удаленный доступ к базе root'ом с правами суперпользователя

(Например, для разработки)

-- Толи баг, толи фича, но GRANT ALL PRIVILEGES не работает в версии mysql55. Приходилось подключаться к базе phpmyadmin и задавать набор прав в ручную. При этом приходилось снимать галочку из одной возможности суперпользователя. В версии, что из стандартного репозитория, все ок.

    # mysql -u root

    mysql> USE mysql;
    mysql> SELECT user,host FROM user;

    mysql> SELECT USER(), CURRENT_USER();

    mysql> CREATE USER 'root'@'%' IDENTIFIED BY 'root';

    mysql> GRANT USAGE ON *.* TO 'root'@'%' IDENTIFIED BY 'root' WITH MAX_QUERIES_PER_HOUR 0 MAX_CONNECTIONS_PER_HOUR 0 MAX_UPDATES_PER_HOUR 0 MAX_USER_CONNECTIONS 0;

    mysql> GRANT USAGE ON *.* TO 'root'@'%' WITH GRANT OPTION;

    mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%';

    mysql> FLUSH PRIVILEGES;

    mysql> exit
