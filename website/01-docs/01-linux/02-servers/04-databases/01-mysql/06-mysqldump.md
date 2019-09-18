---
layout: page
title: mysqldump
permalink: /linux/servers/databases/mysql/mysqldump/
---

# mysqldump

    // backup с локального компьютера
    # mysqldump --all-databases --user=dba --password=mypass -A > mysql_backup.dmp

<br/>

    // backup с удаленного компьютера
    ssh root@192.168.2.20 /usr/bin/mysqldump --all-databases --user=dba --password=mypass -A > mysql_backup.dmp



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

$ mysql -u blogadmin -p wordpress --host=35.202.4.99 < mysql_backup.dmp

blogadmin

Password1*


$ mysql --host=35.202.4.99 \
    --user=blogadmin --password

use wordpress

show tables;


cd /var/www/html/wordpress

vi wp-config.php

# cd /etc/init.d/
# ./apache2 restart

===============




# cd /etc/init.d/
# ./apache2 restart

GSP306
-->