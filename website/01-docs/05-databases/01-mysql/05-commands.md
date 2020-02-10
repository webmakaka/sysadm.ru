---
layout: page
title: Команды для работы с базой данных MySQL
permalink: /databases/mysql/commands/
---

# Команды для работы с базой данных MySQL

Подключиться к базе из командной строки

    # mysql --user=dba --password=mypass mydatabase

Узнать текущего пользователя:

    mysql> SELECT USER();


Узнать текущую базу данных:

    mysql> SELECT DATABASE();



<br/>

    // Проверка состояния базы
    # mysqladmin -u sysdba -p status
