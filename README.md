# Исходники сайта [sysadm.ru](http://sysadm.ru)

[![Join the chat at https://gitter.im/sysadm-ru/Lobby](https://badges.gitter.im/sysadm-ru/Lobby.svg)](https://gitter.im/sysadm-ru/Lobby?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge) [![Build Status](https://travis-ci.org/sysadm-ru/sysadm.ru.svg?branch=gh-pages)](https://travis-ci.org/sysadm-ru/sysadm.ru)

Скопировать и запустить sysadm.ru на свой хост с использованием docker контейнера:

Инсталлируете docker и docker-compose, далее:

    $ cd ~
    $ mkdir -p sysadm.ru && cd sysadm.ru
    $ git clone --depth=1 https://sysadm-ru@bitbucket.org/sysadm-ru/sysadm.ru.git .
    $ docker-compose up

// Запустить на background

    $ docker-compose up &
    $ curl http://localhost:64001

<br/>

Остается в браузере подключиться к localhost:65001



<br/>

### Как сервис на coreos    

    $ sudo su -
    # cd /home/core/sysadm.ru/
    # cp sysadm_ru.service /etc/systemd/system
    # systemctl enable sysadm_ru.service
    # systemctl start sysadm_ru.service
    # systemctl status sysadm_ru.service
