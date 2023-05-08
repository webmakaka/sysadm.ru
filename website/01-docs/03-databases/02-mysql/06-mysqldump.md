---
layout: page
title: mysqldump
description: mysqldump
keywords: databases, mysql, mysqldump
permalink: /databases/mysql/mysqldump/
---

# mysqldump

    // backup с локального компьютера
    # mysqldump --all-databases --user=dba --password=mypass -A > mysql_backup.dmp

<br/>

    // backup с удаленного компьютера
    ssh root@192.168.2.20 /usr/bin/mysqldump --all-databases --user=dba --password=mypass -A > mysql_backup.dmp

<br/>

Создание бекапов, траблшутинг и т.д.  
http://centoshelp.org/servers/database/installing-configuring-mysql-server/

<br/>

### dump

export

    mysqldump -u [username] -p [database_name] > [dumpfilename.sql]

import

    mysql -u [username] -p [database_name] < [dumpfilename.sql]

Со сжатием:

For Export:

    mysqldump -u [user] -p [db_name] | gzip > [filename_to_compress.sql.gz]

For Import:

    gunzip < [compressed_filename.sql.gz]  | mysql -u [user] -p[password] [databasename]

<!--

# cd /etc/init.d/
# ./apache2 stop




$ mysqldump wordpress --user=blogadmin --password=Password1* > mysql_backup.dmp


<br/>

Cloud SQL Instances -- MySQL

DB_NAME - wordpress
DB_USER - blogadmin
DB_PASSWORD - Password1*


Cloud SQL Instances -- Create Database

# mysql --host=104.197.170.9 \
    --user=root \
    --password


$ mysql --host=104.197.170.9 \
    wordpress \
    --user=blogadmin \
    --password < mysql_backup.dmp


$ mysql --host=104.197.170.9 \
    wordpress \
    --user=blogadmin \
    --password


show tables;

====

cd /var/www/html/wordpress

vi wp-config.php

# cd /etc/init.d/
# ./apache2 restart


GSP306
-->
