---
layout: page
title: Install logstash ELK stack on CentOS 7 Elasticsearch, Logstash, Kibana
permalink: /linux/logging/elk/install/centos/
---

# Install logstash ELK stack on CentOS 7 Elasticsearch, Logstash, Kibana

<br/>

Делаю  
12.04.2019

По материалам индуса:

https://www.youtube.com/watch?v=Cfbanio3Lao&list=PL34sAs7_26wOgpqMW_0_E95k9tq2VkMOZ&index=1

<br/>
Рисунки индуса:
<br/>

![elk install 01](/img/logging/elk/intall/elk-install-01.png 'elk install 01'){: .center-image }

<br/>

![elk install 02](/img/logging/elk/intall/elk-install-02.png 'elk install 02'){: .center-image }

<br/>

![elk install 03](/img/logging/elk/intall/elk-install-03.png 'elk install 03'){: .center-image }

<br/>

![elk install 04](/img/logging/elk/intall/elk-install-04.png 'elk install 04'){: .center-image }

<br/>

![elk install 05](/img/logging/elk/intall/elk-install-05.png 'elk install 05'){: .center-image }

<br/>

![elk install 06](/img/logging/elk/intall/elk-install-06.png 'elk install 06'){: .center-image }

<br/>

### Подготавливаем host машину

<br/>

    $ vagrant plugin install vagrant-hostmanager

<br/>

    $ sudo /etc/hosts

```
#---------------------------------------------------------------------
# ELK hosts
#---------------------------------------------------------------------

192.168.0.10 client.example.com client
192.168.0.11 server.example.com server
```

<br/>

### На хост машине запускаем виртуалки

    $ mkdir ~/vagrant-elk && cd ~/vagrant-elk

    # git clone https://bitbucket.org/sysadm-ru/elk .

    $ cd vagrant-provisioning/

    $ vagrant up

<br/>

### Устанавливаем и настраиваем по следующей инструкции.

https://bitbucket.org/sysadm-ru/elk/src/master/INSTALL-CentOS7.md

root пароль - admin

<br/>

### На клиенте настройка Filebeat

    # cd /etc/filebeat/
    # cp filebeat.yml filebeat.yml.orig

<br/>

    # vi filebeat.yml

```
  # Change to true to enable this input configuration.
  enabled: true

```

<br/>

```
  paths:
    - /var/log/messages
    - /var/log/secure

```

<br/>

Комментируем

<br/>

```
#-------------------------- Elasticsearch output ------------------------------

  #output.elasticsearch:
  # Array of hosts to connect to.
  #hosts: ["localhost:9200"]

```

<br/>

```

#----------------------------- Logstash output --------------------------------
output.logstash:
  # The Logstash hosts
  hosts: ["server.example.com:5044"]

  # Optional SSL. By default is off.
  # List of root certificates for HTTPS server verifications
  ssl.certificate_authorities: ["/etc/pki/tls/certs/logstash.crt"]


```

<br/>

    # journalctl --unit filebeat
    -- Logs begin at Fri 2019-04-12 08:13:59 UTC, end at Fri 2019-04-12 08:38:26 UTC
    Apr 12 08:38:26 client.example.com systemd[1]: Started Filebeat sends log files

<br/>

### Настройка в GUI

http://server.example.com/app/kibana

<br/>

![elk install 07](/img/logging/elk/intall/elk-install-07.png 'elk install 07'){: .center-image }
