---
layout: page
title: Перенос форума в контейнеры Docker
permalink: /linux/containers/docker/odba/
---




Имеем форум работающий в VirtualBox.

Хочу перенести его в контейнеры docker (да еще и на хост с coreos).


<!--


Нужен также docker compose, git. И для импорта / экспорта phpmyadmin.


Для начала на bitbucket делаю приватное репо с php скриптами и дампом базы данных.

-->

Раз мы используем docker, то я пожалуй возьму уже подготовленное окружение для mysql. Web Server собиру сам.


    $ docker pull debian
    $ docker pull mysql

<br/>

<!--
    $ docker run --name mysql_server -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql

<br/>

    $ docker ps
    CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
    726bc4c2433a        mysql               "/entrypoint.sh mysql"   9 seconds ago       Up 9 seconds        3306/tcp            mysql_server

<br/>

    $ docker stop mysql_server


-->

<br/>

### Nginx c PHP я поднимаю сам и делаю image


    $ docker run -it --name nginx_server -d debian
    $ docker attach nginx_server

<br/>

    # apt-get install -y git

Настраиваю как здесь:

http://sysadm.ru/linux/webservers/nginx/1.8/debian/jessie/installation/


    $ docker commit nginx_server marley/nginx_server:1

    $ docker login
    $ docker push marley/nginx_server:1


Теперь, при желании, можно забрать командой:

    $ docker pull marley/nginx_server:1



<br/>

### Вариант линковски с помощью Docker compose


    $ cd ~/

    $ vi my_web_serv.yml

<br/>



    web_server:
      container_name: nginx_serv
      image: marley/nginx_server:1
      command: nginx -g 'daemon off;'
      ports:
         - "8080:8080"
      links:
         - mysql_server       

    mysql_server:
      container_name: mysql_serv
      image: mysql:latest
      environment:
         - MYSQL_ROOT_PASSWORD=P@SSW0RD


<br/>


    $ docker-compose -f my_web_serv.yml up -d web_server

<br/>

    $ docker-compose -f my_web_serv.yml ps
       Name                Command             State           Ports          
    -------------------------------------------------------------------------
    mysql_serv   docker-entrypoint.sh mysqld   Up      3306/tcp               
    nginx_serv   nginx -g daemon off;          Up      0.0.0.0:8080->8080/tcp


<br/>

    $ docker ps
    CONTAINER ID        IMAGE                   COMMAND                  CREATED              STATUS              PORTS                    NAMES
    778b1b3b6c58        marley/nginx_server:1   "nginx -g 'daemon off"   About a minute ago   Up 59 seconds       0.0.0.0:8080->8080/tcp   nginx_serv
    72e78734dd63        mysql:latest            "docker-entrypoint.sh"   About a minute ago   Up About a minute   3306/tcp                 mysql_serv




При необходимости:

    $ docker-compose -f my_web_serv.yml stop

    $ docker-compose -f my_web_serv.yml rm



Можно с хоста попробовать подключиться к вебсерверу:  

http://localhost:8080/

(502 Bad Gateway)


<br/>

### Вариант линковски без Docker compose


    $ docker run -t -i --name web_server -p 80:8080 --link mysql_server:mysql_server marley/nginx_server:1


<br/>


### Работа внутри контейнера:


    $ docker exec -it nginx_serv bash
    $ service php5-fpm restart

    # cd /projects/odba.ru/public
    # rm *


-- поставить с помощью git не получилось. В скрипте ругань на отсутствующий каталог. И он реально отсутствует.

    # git clone --depth=1 https://github.com/phpmyadmin/phpmyadmin

<br/>

Поэтому:


    # wget https://files.phpmyadmin.net/phpMyAdmin/4.6.0/phpMyAdmin-4.6.0-all-languages.7z
    # 7z x ./phpMyAdmin-4.6.0-all-languages.7z
    # rm phpMyAdmin-4.6.0-all-languages.7z
    # mv phpMyAdmin-4.6.0-all-languages phpmyadmin
    # cd phpmyadmin

<br/>

    # env
    MYSQL_SERVER_ENV_MYSQL_VERSION=5.7.11-1debian8
    HOSTNAME=56f762e4adb6
    MYSQL_SERVER_ENV_MYSQL_MAJOR=5.7
    MYSQL_SERV_PORT_3306_TCP_ADDR=172.17.0.2
    MYSQL_SERVER_PORT_3306_TCP=tcp://172.17.0.2:3306
    MYSQL_SERV_PORT=tcp://172.17.0.2:3306
    MYSQL_SERVER_PORT_3306_TCP_PORT=3306
    MYSQL_SERVER_PORT=tcp://172.17.0.2:3306
    MYSQL_SERV_ENV_MYSQL_MAJOR=5.7
    MYSQL_SERV_PORT_3306_TCP_PORT=3306
    MYSQL_SERVER_PORT_3306_TCP_ADDR=172.17.0.2
    MYSQL_SERV_ENV_MYSQL_VERSION=5.7.11-1debian8
    PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
    MYSQL_SERV_PORT_3306_TCP_PROTO=tcp
    PWD=/projects/odba.ru/public/phpmyadmin
    MYSQL_SERVER_ENV_MYSQL_ROOT_PASSWORD="P@SSW0RD"
    MYSQL_SERV_PORT_3306_TCP=tcp://172.17.0.2:3306
    SHLVL=1
    HOME=/root
    MYSQL_SERV_NAME=/nginx_serv/mysql_serv
    MYSQL_SERVER_NAME=/nginx_serv/mysql_server
    MYSQL_SERV_ENV_MYSQL_ROOT_PASSWORD="P@SSW0RD"
    MYSQL_SERVER_PORT_3306_TCP_PROTO=tcp
    _=/usr/bin/env
    OLDPWD=/projects/odba.ru/public



<br/>

    # cp config.sample.inc.php config.inc.php

<br/>

    # vi config.inc.php

Прописываю вместо localhost ip сервера из environment.


    # vi /etc/php5/fpm/php.ini

    post_max_size = 50M
    upload_max_filesize = 50M


Ошибка при импорте

    # cd /etc/nginx/conf.d/
    # vi odba.ru.conf


    server {
        ***
        client_max_body_size 50m;


Подключился к phpmyadmin.

http://localhost:8080/phpmyadmin/


Пароль, нужно набирать с кавычками. Т.е. "P@SSW0RD"

Все подключился к консоли phpmyadmin


Мда. Какой-то баг. Экспортированный файл дампа не импортируется в новую с ошибкой.
Написано, что пофикшена в новой версии, которая у меня не стартует, т.к. php устарела.

Вообщем пока откладываю. Все сложно. И не особо пока нужно.
