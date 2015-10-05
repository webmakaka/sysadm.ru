# Исходники сайта sysadm.ru


Скопировать и запустить sysadm.ru на свой хост с использованием docker контейнера:

Инсталлируете docker, далее:

    docker pull marley/centos6-for-dev
    docker run -i -t –rm -p 80:8080 –-name sysadm-dev marley/centos6-for-dev /bin/bash

<br/>

    source ~/.bash_profile
    cd /projects
    git clone --depth=1 https://github.com/sysadm-ru/sysadm-ru.github.io
    cd sysadm-ru.github.io
    gem install jekyll
    jekyll serve --watch  --host 0.0.0.0 --port 8080


<br/>

Остается в браузере подключиться к localhost
