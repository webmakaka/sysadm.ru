---
layout: page
title: Инсталляция Node.js в облаке AWS
permalink: /linux/cloud/aws/nodejs-server/
---

Имеем Amazon'овский образ RedHat based  

Подключились к серверу.

    sudo yum update -y 
    sudo yum install -y gcc-c++ make
    sudo yum install -y openssl-devel
    sudo yum install -y git
    
<br/>

**Инсталляция node.js**
    
    cd /tmp/
    git clone git://github.com/joyent/node.git
    cd node
    ./configure
    make
    sudo make install
    
<br/>
    
    sudo su
    vi /etc/sudoers


{% highlight text %}

Нужно добавить в secure_path :/usr/local/bin

Defaults secure_path = /sbin:/bin:/usr/sbin:/usr/bin
заменить на
Defaults secure_path = /sbin:/bin:/usr/sbin:/usr/bin:/usr/local/bin

{% endhighlight %}

**Инсталляция npm**

    cd /tmp/
    git clone https://github.com/isaacs/npm.git
    cd npm
    sudo make install
    
    
    
**Возможная проверка работы Node.js приложения в AWS**


{% highlight text %}

require("http").createServer(function(request, response){
  response.writeHeader(200, {"Content-Type": "text/plain"});  
  response.write("Hello World!");  
  response.end();
}).listen(8080);

{% highlight text %}


    node test_server.js
    curl http://localhost:8080


P.S. Express приложение в opsWorks у меня не заработало, хотя я создавал в корне файл server.js с содержимым /bin/www и явными инструкциями для запуска приложения в package.json. Кто умеет, поделитесь информацией.

___
см:  
http://iconof.com/blog/how-to-install-setup-node-js-on-amazon-aws-ec2-complete-guide/
