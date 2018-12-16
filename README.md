# Исходники сайта [sysadm.ru](http://sysadm.ru)

Скопировать и запустить sysadm.ru на свой хост с использованием docker контейнера:

    $ docker run -i -t -p 80:80 --name sysadm.ru marley/sysadm.ru

<br/>

### Несколько устаревший вариант

Инсталлируете docker и docker-compose, далее:

    $ cd ~
    $ mkdir -p sysadm.ru && cd sysadm.ru
    $ git clone --depth=1 https://sysadm-ru@bitbucket.org/sysadm-ru/sysadm.ru.git .
    $ docker-compose up

// Запустить background

    $ docker-compose up &
    $ curl http://localhost:65001

<br/>

Остается в браузере подключиться к localhost:65001



<br/>

### Как сервис 

    $ sudo su -
    # mkdir -p /project
    # cd /project
    # git clone --depth=1 https://sysadm-ru@bitbucket.org/sysadm-ru/sysadm.ru.git
    # cd sysadm.ru/
    
    # cp sysadm_ru.service /etc/systemd/system
    # chmod 777 -R /project/
    
    # systemctl enable sysadm_ru.service
    # systemctl start sysadm_ru.service
    # systemctl status sysadm_ru.service


http://localhost:65001
