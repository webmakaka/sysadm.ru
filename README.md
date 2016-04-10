# Исходники сайта [sysadm.ru](http://sysadm.ru)


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
