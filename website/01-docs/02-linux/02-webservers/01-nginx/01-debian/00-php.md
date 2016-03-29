---
layout: page
title: Инсталляция PHP и настройка для работы с Nginx
permalink: /linux/webservers/nginx/debian/php/
---

### Инсталляция PHP и настройка для работы с Nginx


    # apt-get install -y links

    # apt-cache search php5


    # apt-get install -y \
    php5-common \
    php5-cli \
    php5-mysqlnd \
    php5-pgsql \
    php5-sqlite \
    php5-mongo \
    php5-gd \
    php5-mcrypt \
    php5-apcu \
    php5-memcache \
    php5-memcached  \
    php5-xmlrpc \
    php5-fpm \
    php5-cgi

<br/>


    # vi /etc/nginx/sites-available/sysadm.ru.config

    server {
        listen     *:8080;
        server_name sysadm.ru;
        access_log /var/log/nginx/sysadm.ru/access.log;
        error_log /var/log/nginx/sysadm.ru/error.log;
        root /projects/sysadm.ru;

        location / {
            index index.html index.htm index.php;
        }

        location ~ \.php$ {
             try_files $uri = 404;
             include fastcgi_params;
             fastcgi_pass  unix:/var/run/php5-fpm.sock;
             fastcgi_index index.php;

             fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
       }
    }


<br/>

    # rm /projects/sysadm.ru/index.html
    # vi /projects/sysadm.ru/index.php

    <?php phpinfo(); ?>


    # service php5-fpm restart
    # service nginx restart

    # links sysadm.ru:8080
