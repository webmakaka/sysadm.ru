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
