---
layout: page
title: Подключиться к серверу в облаке AWS в командной строке linux
permalink: /linux/cloud/aws/connect-to-server/
---

**Подключиться к серверу:**

Создать Key Pair в консоли AWS и с скачать ключ.

    chmod 400 /home/marley/Downloads/AWS-Key.pem
    ssh -i /home/marley/Downloads/AWS-Key.pem ec2-user@<ip_сервера>.

___

см:  
http://www.youtube.com/watch?v=Ix5IDuyamuY  



### Converting a ppk file to a pem file for accessing AWS ec2 instances on Linux

    $ sudo apt-get install putty-tools

    $ puttygen ppkkey.ppk -O private-openssh -o pemkey.pem


http://webkul.com/blog/convert-a-ppk-file-to-a-pem-file/
