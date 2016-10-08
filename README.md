# Исходники сайта [sysadm.ru](http://sysadm.ru)

[![Join the chat at https://gitter.im/sysadm-ru/Lobby](https://badges.gitter.im/sysadm-ru/Lobby.svg)](https://gitter.im/sysadm-ru/Lobby?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge) [![Build Status](https://travis-ci.org/sysadm-ru/sysadm.ru.svg?branch=gh-pages)](https://travis-ci.org/sysadm-ru/sysadm.ru) 

Скопировать и запустить sysadm.ru на свой хост с использованием docker контейнера:

Инсталлируете docker, далее:

    $ docker pull marley/centos6-for-dev
    $ docker run -i -t -p 80:8080 marley/centos6-for-dev /bin/bash

Далее уже в контейнере docker:

    $ source ~/.bash_profile
    $ cd /projects
    $ git clone --depth=1 https://github.com/sysadm-ru/sysadm.ru
    $ cd sysadm.ru/
    $ gem install jekyll
    $ jekyll serve --watch  --host 0.0.0.0 --port 8080


<br/>

Остается в браузере подключиться к localhost
