---
layout: page
title: Инсталляция Node.js в облаке AWS
permalink: /linux/cloud/aws/nodejs-server/
---

Подключились к серверу.

    sudo yum update
    sudo yum install gcc-c++ make
    sudo yum install openssl-devel
    sudo yum install git
    
    <br/>
    
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
Defaults secure_path = /sbin:/bin:/usr/sbin:/usr/bin
заменить на
Defaults secure_path = /sbin:/bin:/usr/sbin:/usr/bin:/usr/local/bin
{% endhighlight %}s secure_path = /sbin:/bin:/usr/sbin:/usr/bin:/usr/local/bin
```

    cd /tmp/
    git clone https://github.com/isaacs/npm.git
    cd npm
    sudo make install
___
см:  
http://iconof.com/blog/how-to-install-setup-node-js-on-amazon-aws-ec2-complete-guide/
