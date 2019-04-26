---
layout: page
title: Monitoring Apache Logs and Metrics in ELK stack
permalink: /linux/servers/logging/elk/apache-logs-and-metrics/
---

# Monitoring Apache Logs and Metrics in ELK stack

<br/>

Делаю  
26.04.2019

По материалам индуса:

https://www.youtube.com/watch?v=hUIP0qW6NF4&list=PL34sAs7_26wOgpqMW_0_E95k9tq2VkMOZ&index=5

<br/>

### Поднимаю ELK в контейнере

Поднимаю ELK в контейнере и запускаю клиенты, устанавливаю и настраиваю filebeat, metricbeat как <a href="/linux/servers/logging/elk/docker/">здесь</a>. Клиент у нас только centos7.

<br/>

### Устанавливаем и настраиваем apache

    $ sudo su -
    # yum install -y httpd
    # echo "<h1>Just me and Open Source</h1>" > /var/www/html/index.html

    # systemctl enable httpd
    # systemctl restart httpd
    # systemctl status httpd

<br/>

http://192.168.0.11/
OK

<br/>

### Устанавливаем и настраиваем filebeat apache2

    # filebeat modules enable apache2
    # vi /etc/filebeat/modules.d/apache2.yml

Делаю, чтобы было:

    var.paths: ["/var/log/httpd/access_log"]

    var.paths: ["/var/log/httpd/error_log"]

<br/>

Не знаю, насколько нужно выполнять следующие шаги, но без них не заработало

    # filebeat setup
    # systemctl restart filebeat
    # systemctl status filebeat

<br/>

http://localhost:5601/app/kibana

Dashboard -> [Filebeat Apache2] Access and error logs

<br/>

### Metricbeat

    # vi /etc/httpd/conf/httpd.conf

Добавляем в конец

```
<Location /server-status>
    SetHandler server-status
</Location>

```

<br/>

http://192.168.0.11/server-status
OK

<br/>

    # systemctl restart httpd

<!-- $ sudo metricbeat modules disable system -->

    # metricbeat modules enable apache

    # vi /etc/metricbeat/modules.d/apache.yml

Разкомментировали

```
  metricsets:
    - status
```

<br/>

    # metricbeat setup
    # systemctl restart metricbeat
    # systemctl status metricbeat

<br/>

http://localhost:5601/app/kibana

Dashboard -> [Metricbeat Apache] Overview

<br/>

### Поиск ошибок

    # grep -i error /var/log/metricbeat/metricbeat
