---
layout: page
title: Перенос форума в контейнеры Docker
permalink: /linux/virtual/docker/odba/
---




Имеем форум работающий в VirtualBox.

Хочу перенести его в контейнеры docker (да еще и на хост с coreos).


<!--


Нужен также docker compose, git. И для импорта / экспорта phpmyadmin.


Для начала на bitbucket делаю приватное репо с php скриптами и дампом базы данных.

-->

Раз мы используем docker, то я пожалуй возьму уже подготовленное окружение для mysql.


    $ docker pull debian
    $ docker pull mysql

<br/>


    $ docker run --name mysql_server -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql

<br/>

    $ docker ps
    CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
    726bc4c2433a        mysql               "/entrypoint.sh mysql"   9 seconds ago       Up 9 seconds        3306/tcp            mysql_server

<br/>

    $ docker stop mysql_server

<br/>

### Nginx c PHP я подниму сам и сделаю image


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

    $ docker pull marley/nginx_server



<!--

===================================================



    $ vi docker-compose.yml

<br/>

    web_server:
     image: marley/nginx_server
     ports:
        - "80:8080"
     links:
        - mysql_server       

    mysql_server:
      image: mysql:latest
      environment:
        MYSQL_ROOT_PASSWORD: "my-secret-pw"

<br/>


    $ docker-compose up --no-deps -d web_server


-->


    $ docker run -t -i --name web_server -p 80:8080 --link mysql_server:mysql_server marley/nginx_server:1


    # cd /projects/odba.ru/public
    # rm *


-- поставить с помощью git не получилось. В скрипте ругань на отсутствующий каталог. И он реально отсутствует.

    # git clone --depth=1 https://github.com/phpmyadmin/phpmyadmin

<br/>

Поэтому:

    # wget https://files.phpmyadmin.net/phpMyAdmin/4.5.0.2/phpMyAdmin-4.5.0.2-all-languages.tar.bz2
    # apt-get install -y tar bzip2

    # tar -jxf phpMyAdmin-4.5.0.2-all-languages.tar.bz2

<br/>

    # env
    MYSQL_SERVER_ENV_MYSQL_VERSION=5.7.11-1debian8
    HOSTNAME=43a5f7002bec
    MYSQL_SERVER_ENV_MYSQL_MAJOR=5.7
    TERM=xterm
    MYSQL_SERVER_PORT_3306_TCP=tcp://172.17.0.2:3306
    MYSQL_SERVER_PORT_3306_TCP_PORT=3306
    MYSQL_SERVER_PORT=tcp://172.17.0.2:3306
    MYSQL_SERVER_PORT_3306_TCP_ADDR=172.17.0.2
    PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
    PWD=/projects/odba.ru/public
    MYSQL_SERVER_ENV_MYSQL_ROOT_PASSWORD=my-secret-pw
    SHLVL=1
    HOME=/root
    MYSQL_SERVER_NAME=/web_server/mysql_server
    MYSQL_SERVER_PORT_3306_TCP_PROTO=tcp
    _=/usr/bin/env
    OLDPWD=/projects/odba.ru/public/setup/frames


<br/>

    # cp config.sample.inc.php config.inc.php

<br/>

    # vi config.inc.php

Прописываю вместо localhost ip сервера из environment.


Подключился к phpmyadmin.


Все пошел футбол смотреть.
Продолжу, наверное на следующих выходных.


Хотелось бы с использованием docker composer

И еще мне не нравится, что я под рутом стартую сервер.
