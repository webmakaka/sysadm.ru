---
layout: page
title: Heroku Centos 6.X
permalink: /linux/cloud/heroku/
---

# Heroku Centos 6.X

Создал аккаунт на сайте heroku.com


Далее на centos инсталлирую по для работы с heroku

    # cd /tmp/
    # wget -qO- https://toolbelt.heroku.com/install.sh | sh

<br/>

    # su - developer

<br/>  

    $ vi ~/.bash_profile

<br/>

    #### HEROKU ##############################

        export HEROKU_HOME=/usr/local/heroku/bin/
        export PATH=$PATH:$HEROKU_HOME

    #### HEROKU ##############################


<br/>


    $ source ~/.bash_profile


Подготовка приложения.

    $ mkdir marley.org
    $ cd marley.org

Копирую приложение, в моем случае node.js приложение с bitbucket

    $ heroku login
    $ heroku apps:create marley-org


<!--

heroku apps:create marley-org


mkdir example

cd example

git init

heroku apps:create example


-->

    $ git remote -v
    heroku	https://git.heroku.com/marley-org.git (fetch)
    heroku	https://git.heroku.com/marley-org.git (push)

<br/>

    $ heroku config:set NODE_ENV=production

    Setting config vars and restarting morning-ridge-6211... done, v3
    NODE_ENV: production

<br/>

    $ git push heroku master

<br/>

    $ heroku ps:scale web=1
    Scaling dynos... done, now running web at 1:1X.

<br/>

    // Если есть браузер, можно запустить его в командной строке
    heroku open

<br/>

    // если нет, то из webконсоли heroku
    // в моем случае это
    https://marley-org.herokuapp.com/



<br/>

### Добавление домена на хостинг heroku

**Теперь, чтобы привязать домен к аккаунту требуется предоставить данные с редитки. Раньше можно было и без**


После того, как Heroku получил мою карточку. Он стал клянчить деньги. Говорит, мол ресурс стал популярным. Но я то знаю, что это не совсем так, или даже совсем не так.

    $ heroku domains:add marley.org
    $ heroku domains:add www.marley.org

<br/>

    $ heroku domains
    === marley-org Domain Names
    marley-org.herokuapp.com
    marley.org
    www.marley.org

<br/>

    // В админке управления доменом:
    CNAME marley.org marley-org.herokuapp.com


Troubleshooting

        heroku logs
        heroku restart


<br/>

### После долгой паузы, понадобилось подключиться и скопировать файлы с heroku

    $ heroku auth:login

<br/>

    $ heroku apps
    === My Apps
    marley-org

<br/>

    $ heroku git:clone -a marley-org



<br/>

### После долгой паузы, понадобилось подключиться и обновить node.js приложение в Heroku

    $ heroku auth:login

<br/>

    $ heroku apps
    === My Apps
    marley-org


<br/>

    $ cd ./marley.org

<br/>


    $ heroku config:set --app marley-org


<br/>

    $ git remote add heroku https://git.heroku.com/marley-org.git

<br/>

    $ git push heroku master

<br/>

    $ heroku config:set NODE_ENV=production

<br/>

    $ heroku ps:scale web=1
