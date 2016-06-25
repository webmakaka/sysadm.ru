---
layout: page
title: Инсталляция node.js, bower в centos 6.X
permalink: /linux/dev/nodejs/installation/centos/
---

### Инсталляция node.js, bower в centos 6.X

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

<br/>

// Создание пользователя, который будет работать с проектом


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

<br/>

//  Инициализация проекта. Описание проекта, будет храниться в файле package.json

    $ npm init

<br/>

    // Инсталляция фреймворка express (если нужно). С опицей save, в package.json будет добавлена информация о том, что этот фреймворк требуется для приложения.
    $ npm install --save express


    // Установить все зависимости проекта, которые описаны в файле package.json
    $ npm install   

<br/>

    // Инсталляция с помощью bower. С опцией save, данные будут записаны в файл bower.json


    // twitter bootstrap
    $ bower install --save bootstrap

    // jquery  
    $ bower install --save jquery

    // angular.js
    $ bower install --save angular


<br/>

### Обновление Node.js на Centos 6.X

    # node -v
    v0.10.40

    # npm cache clean -f
    # npm install -g n

    # /usr/lib/node_modules/n/bin/n latest

    # ln -sf /usr/local/n/versions/node/<VERSION>/bin/node /usr/bin/node

    # node -v
    v6.2.2


<br/>

### Обновление NPM на Centos 6.X

    # npm -v
    2.14.7

    # npm install -g npm

    # npm -v
    3.10.2



<!--

<br/>

    $ cd myproject/
    $ git clone https://github.com/oracle-jet/work-better-jet
    $ cd work-better-jet/

<br/>

    $ vi app.js

<br/>

    var http = require('http'),
        fs = require('fs');


    fs.readFile('./index.html', function (err, html) {
        if (err) {
            throw err;
        }       
        http.createServer(function(request, response) {  
            response.writeHeader(200, {"Content-Type": "text/html"});  
            response.write(html);  
            response.end();  
        }).listen(8000);
    });


<br/>


http://192.168.56.2:8000/


-->
