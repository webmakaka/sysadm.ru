---
layout: page
title: Инсталляция PHP и настройка для работы с Nginx 1.6
description: Инсталляция PHP и настройка для работы с Nginx 1.6
keywords: Инсталляция PHP и настройка для работы с Nginx 1.6
permalink: /linux/webservers/nginx/1.6/debian/jessie/php/
---

# Инсталляция PHP и настройка для работы с Nginx 1.6

    # apt-cache search php5

<br/>

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

    # cd /var/www/html

<br/>

    # rm index.nginx-debian.html
    # vi index.php

    <?php phpinfo(); ?>

<br/>

    # cd /etc/nginx/sites-available/

<br/>

    # vi default

<br/>

Меняю блок location на следующее:

<br/>

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

<br/>

    # service php5-fpm restart
    # service nginx restart

<br/>

    # links http://localhost
