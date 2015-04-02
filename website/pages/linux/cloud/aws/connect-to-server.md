---
layout: page
title: Подключиться к серверу в облаке AWS в командной строке linux
permalink: /linux/cloud/aws/connect-to-server/
---

**Подключиться к серверу:**

1. Создать Pey Pair в консоли AWS и с скачать ключ .

2. chmod 400 /home/marley/Downloads/AWS-Key.pem

3. ssh -i /home/marley/Downloads/AWS-Key.pem ec2-user@<ip_сервера>.

___


см:  
http://www.youtube.com/watch?v=Ix5IDuyamuY  
