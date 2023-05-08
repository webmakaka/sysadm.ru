---
layout: page
title: Инсталляция PHP и настройка для работы с Nginx 1.8
description: Инсталляция PHP и настройка для работы с Nginx 1.8
keywords: Инсталляция PHP и настройка для работы с Nginx 1.8
permalink: /webservers/nginx/1.8/debian/jessie/php/
---

# Инсталляция PHP и настройка для работы с Nginx 1.8

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

    # cd /usr/share/nginx/html

<br/>

    # rm *
    # vi index.php

    <?php phpinfo(); ?>

<br/>

    # cd /etc/nginx/conf.d
    # cp default.conf default.conf.orig

<br/>

    # vi default.conf

<br/>

Меняю содержимое на:

<br/>

```
server {
    listen       80;
    server_name  localhost;
    root   /usr/share/nginx/html;

    location / {
        index index.php index.html;
    }

    location ~ \.php$ {
            try_files $uri = 404;
            include fastcgi_params;
            fastcgi_pass  unix:/var/run/php5-fpm.sock;
            fastcgi_index index.php;

            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
    }

}
```

<br/>

Без добавления в группу не работало:

    # usermod -G www-data nginx

<br/>

    #  groups nginx
    nginx : nginx www-data

<br/>

<!--
    # cp /etc/php5/fpm/pool.d/www.conf  /etc/php5/fpm/pool.d/www.conf.orig
    # vi /etc/php5/fpm/pool.d/www.conf

    listen.mode = 0660

-->

    # service php5-fpm restart
    # service nginx restart

<br/>

    # curl -I http://localhost

<br/>

    # links http://localhost
