---
layout: page
title: MySQL - Конфигурирование пользователей после инсталляции
permalink: /linux/databases/mysql/installation/users/
---

# MySQL - Конфигурирование пользователей после инсталляции

    // Login as root database admin to MySQL server:
    # mysql -u root

<br/>

    // Change root database admin password: (note: once this step is complete you’ll need to login with: mysql -p -u root)
    mysql> SET PASSWORD FOR 'root'@'localhost' = PASSWORD('root');

<br/>

    // Add a MySQL database:
    mysql> create database mydatabase;

<br/>

    mysql> quit;

<br/>

### Дополнительно для большей безопасности можно настроить следующее:

<br/>

    // Delete ALL users who are not root:
    mysql> DELETE FROM mysql.user WHERE NOT (host="localhost" AND user="root");

<br/>

    // Remove anonymous access to the database(s):
    mysql> DELETE FROM mysql.user WHERE User = '';

<br/>

---

<br/>

    // Можно переименовать root
    // Change root username to something less guessable for higher security.
    mysql> update mysql.user set user="sysdba" where user="root";

<br/>

    // Add a new user with database admin privs for all databases:
    mysql> GRANT ALL PRIVILEGES ON *.* TO 'sysdba'@'localhost' IDENTIFIED BY 'mypass' WITH GRANT OPTION;

<br/>

    // Создать нового пользователя в базе данных
    mysql> CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';

    // Ставил *, а нужно было %
    mysql> CREATE USER 'newuser'@'%' IDENTIFIED BY 'password';


    // Предоставить все привелегии
    mysql> GRANT ALL PRIVILEGES ON *.* TO 'newuser'@'localhost';

    mysql> GRANT ALL PRIVILEGES ON *.* TO 'newuser'@'%';

<br/>

    // Add a new user with database admin privs for a specific database, in this case the database is called “bugzilla”: (note: The ‘bugzilla’ database must first be added, see below.)
    mysql> GRANT ALL PRIVILEGES ON bugzilla.* TO 'dba'@'localhost' IDENTIFIED BY 'mypass';

<br/>

### Информация о пользователях в базе:

    mysql> select User,Host from mysql.user;

<br/>

    mysql> SELECT CONCAT(QUOTE(user),'@',QUOTE(host)) UserAccount FROM mysql.user;

<br/>

    mysql> SHOW GRANTS FOR 'username'@'%';
