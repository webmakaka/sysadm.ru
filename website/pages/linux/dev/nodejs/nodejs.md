---
layout: page
title: Инсталляция node.js, bower в centos 6.X
permalink: /linux/dev/nodejs/iojs/nodejs/
---


    # curl -sL https://rpm.nodesource.com/setup | bash -
    # yum install -y nodejs npm

    # node --version
    v0.10.32

    # npm --version
    1.4.28


    // устанавливаю глобально менеджер пакетов bower
    # npm install -g bower

    # bower --version
    1.3.12


<br/>

    // устанавливаю глобально nodemon
    # npm install -g nodemon

<br/>

    # mkdir -p /projects/myproject

    # useradd developer

    # chown -R developer /projects


    # su - developer

<br/>

    // Определяю куда bower будет копировать пакеты по умолчанию.

    $ vi ~/.bowerrc
    {
    	"directory": "public/vendor/lib"
    }

<br/>  

    $ cd /projects/myproject/


    $ npm init

    $ npm install express --save


<br/>

    // angular.js
    $ bower install angular

    // twitter bootstrap
    $ bower install bootstrap
