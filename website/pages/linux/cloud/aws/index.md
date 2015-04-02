---
layout: page
title:подключение к серверу в облаке AWS и инсталлировать руками Node.js
permalink: /linux/cloud/aws/
---

**Подключиться к серверу:**

1. Создать Pey Pair в консоли AWS и с скачать ключ .

2. chmod 400 /home/marley/Downloads/AWS-Key.pem

3. ssh -i /home/marley/Downloads/AWS-Key.pem ec2-user@<ip_сервера>.

___

**Инсталляция Node.js в облаке AWS**

    sudo yum update
    sudo yum install gcc-c++ make
    sudo yum install openssl-devel
    sudo yum install git

    cd /tmp/
    git clone git://github.com/joyent/node.git
    cd node 
    ./configure
    make
    sudo make install 

    sudo su
    vi /etc/sudoers

```

Defaults    secure_path = /sbin:/bin:/usr/sbin:/usr/bin

заменить на

Defaults    secure_path = /sbin:/bin:/usr/sbin:/usr/bin:/usr/local/bin

```


    cd /tmp/

    git clone https://github.com/isaacs/npm.git
    cd npm
    sudo make install 

___

см:  
http://www.youtube.com/watch?v=Ix5IDuyamuY  
http://iconof.com/blog/how-to-install-setup-node-js-on-amazon-aws-ec2-complete-guide/
