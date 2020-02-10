---
layout: page
title: Improving local file security
permalink: /databases/mysql/installation/security/
---

# Improving local file security


    # vi /etc/my.cnf

<br/>

    [mysqld] section
    добавляем
    set-variable=local-infile=0

<br/>

    // Disabling remote access to the MySQL server (after saving and exiting remember to: service mysqld restart for changes to take effect).
    # vi /etc/my.cnf

<br/>

    [mysqld] section
    добавляем
    skip-networking

<br/>

    // Перестартуем сервис
    # service mysqld restart
