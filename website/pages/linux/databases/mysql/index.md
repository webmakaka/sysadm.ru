---
layout: page
title: Инсталляция MySQL в Centos 6.X
permalink: /linux/databases/mysql/
---


<br/><br/>
<h2>Инсталляция MySQL на Centos 6.5 из пакетов</h2>
<br/><br/>



<pre>

// Если в стандартном репозитории нет нужной версии, можно посмотреть здесь.
# rpm -Uvh http://mirror.webtatic.com/yum/el6/latest.rpm


==========================

# yum search mysql

Если нужно установить версию 5.5, а ее нет в репозитории по умолчанию:

rpm -Uvh http://mirror.webtatic.com/yum/el6/latest.rpm
yum install mysql55w mysql55w-server

Подробнее
https://webtatic.com/packages/mysql55/



===============
Если не нужно:

// Install mysql mysql-server:
# yum install -y mysql mysql-server


===============
# mysql --version
mysql  Ver 14.14 Distrib 5.5.38, for Linux (x86_64) using readline 5.1


# chkconfig --level 2345 mysqld on

// Start MySQL server daemon (mysqld)
# service mysqld start


// Login as root database admin to MySQL server:
# mysql -u root


// Delete ALL users who are not root:
mysql> delete from mysql.user where not (host="localhost" and user="root");


// Remove anonymous access to the database(s):
mysql> DELETE FROM mysql.user WHERE User = '';


// Change root database admin password: (note: once this step is complete you’ll need to login with: mysql -p -u root)
mysql> SET PASSWORD FOR 'root'@'localhost' = PASSWORD('root');


// Change root username to something less guessable for higher security.
mysql> update mysql.user set user="sysdba" where user="root";



// Add a new user with database admin privs for all databases:
mysql> GRANT ALL PRIVILEGES ON *.* TO 'dba'@'localhost' IDENTIFIED BY 'mypass' WITH GRANT OPTION;


// Add a new user with database admin privs for a specific database, in this case the database is called “bugzilla”: (note: The ‘bugzilla’ database must first be added, see below.)
mysql> GRANT ALL PRIVILEGES ON bugzilla.* TO 'dba'@'localhost' IDENTIFIED BY 'mypass';


// Add a MySQL database:
mysql> create database bugzilla;


mysql> quit;

// Improving local file security
# vi /etc/my.cnf

[mysqld] section
добавляем
set-variable=local-infile=0



// Disabling remote access to the MySQL server (after saving and exiting remember to: service mysqld restart for changes to take effect).
# vi /etc/my.cnf

[mysqld] section
добавляем
skip-networking



// Перестартуем сервис
# service mysqld restart

// Подключиться к базе
# mysql --user=dba --password=mypass


// backup с локального компьютера
# mysqldump --all-databases --user=dba --password=mypass -A > mysql_backup.dmp

// backup с удаленного компьютера
ssh root@192.168.2.20 /usr/bin/mysqldump --all-databases --user=dba --password=mypass -A > mysql_backup.dmp


// Проверка состояния базы
# mysqladmin -u sysdba -p status



Создание бекапов, траблшутинг и т.д.
http://centoshelp.org/servers/database/installing-configuring-mysql-server/

==========================================

==========================================
Удаленный доступ пользователя root с правами суперпользователя.
(Например, для разработки)
==========================================

-- Толи баг, толи фича, но GRANT ALL PRIVILEGES не работает в версии mysql55. Приходилось подключаться к базе phpmyadmin и задавать набор прав в ручную. При этом приходилось с нимать галочку из одной возможности суперпользователя. В версии, что из стандартного репозитория, все ок.


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


</pre>


<!--

Импорт


mysql -u root

use photoalbums

SET autocommit=0 ;
source /projects/demo/Beginning-Amazon-Web-Services-with-Node.js/setup/photoalbums.sql;
COMMIT;


-->

### dump

export

    mysqldump -u [username] -p [database_name] > [dumpfilename.sql]

import

    mysql -u [username] -p [database_name] < [dumpfilename.sql]


С сжатием:


For Export:

    mysqldump -u [user] -p [db_name] | gzip > [filename_to_compress.sql.gz]

For Import:

    gunzip < [compressed_filename.sql.gz]  | mysql -u [user] -p[password] [databasename]
