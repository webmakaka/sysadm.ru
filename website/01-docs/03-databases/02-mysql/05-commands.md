---
layout: page
title: Команды для работы с базой данных MySQL
description: Команды для работы с базой данных MySQL
keywords: Команды для работы с базой данных MySQL
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

<br/>

### Пример подключения к базе с использованием сертификатов и выполнения скрипта.

Взято отсюда:

https://practicum.yandex.ru/trainer/ycloud/lesson/68f46e4b-beb1-4741-b989-a51e5d0ff04b/

<br/>

```
mysql --host=<адрес хоста> \
        --port=3306 \
        --ssl-ca=~/.mysql/root.crt \
        --ssl-mode=VERIFY_IDENTITY \
        --user=<имя пользователя> \
        --password \
    имя_базы_данных < createTables.sql
```

<br/>

**createTables.sql**

```
CREATE TABLE IF NOT EXISTS users (
    user_id INT AUTO_INCREMENT,
    nickname VARCHAR(128) NOT NULL,
    avatar VARCHAR(255),
    mail VARCHAR(255),
        PRIMARY KEY (user_id)
) ENGINE=INNODB;
```

<br/>

https://github.com/datacharmer/test_db

Скачайте из репозитория и сохраните на компьютере файлы с расширениями .sql и .dump. В файле employees.sql содержатся SQL команды, необходимые для создания таблиц и добавления в них данных из dump файлов. Для переноса тестовой БД в облако понадобится запустить этот файл. Но, прежде чем приступить к переносу БД, откройте этот файл и удалите или закомментируйте в нем строку 110. В этой строке расположена команда FLUSH LOGS, которая закрывает и снова открывает файлы журналов, а они в этой тестовой БД отсутствуют.

Создайте базу данных employees через консоль управления, а затем восстановите данные из дампа с помощью команды:

<br/>

```
$ mysql --host=<адрес хоста> \
        --port=3306 \
        --ssl-ca=~/.mysql/root.crt \
        --ssl-mode=VERIFY_IDENTITY \
        --user=<имя_пользователя> \
        --password \
    имя_базы_данных < ~/employees.sql
```
