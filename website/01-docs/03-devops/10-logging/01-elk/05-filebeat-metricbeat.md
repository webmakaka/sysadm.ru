---
layout: page
title: Monitoring System Logs and Metrics in ELK Stack (FileBeat & MetricBeat)
permalink: /linux/logging/elk/filebeat-metricbeat/
---

# Monitoring System Logs and Metrics in ELK Stack (FileBeat & MetricBeat)

<br/>

Делаю  
26.04.2019

По материалам индуса:

https://www.youtube.com/watch?v=JB3Pbt7ijNU&list=PL34sAs7_26wOgpqMW_0_E95k9tq2VkMOZ&index=4

<br/>

![FileBeat & MetricBeat](/img/devops/linux/logging/elk/intall/filebeat-metricbeat-01.png 'FileBeat & MetricBeat'){: .center-image }

<br/>

### Поднимаю ELK в контейнере

Поднимаю ELK в контейнере как <a href="/linux/logging/elk/docker/">здесь</a>. (Только на localhost)

<br/>

### Запускаем виртуальные машины клиенты

    $ cd ~/tmp
    $ git clone https://bitbucket.org/sysadm-ru/vagrant
    $ cd ~/tmp/vagrant/vagrantfiles/centos7/
    $ vagrant up

    $ cd ../ubuntu18/
    $ vi Vagrantfile

ip поставил 192.168.0.2#{i}

    $ vagrant up

<br/>

### Проверяем плагины в контейнере elasticsearch (необязательный шаг)

    $ docker exec elasticsearch ls /usr/share/elasticsearch/plugins
    ingest-geoip
    ingest-user-agent


    // Если предыдущая команда не вернет ничего, выполнить
    $ docker exec elasticsearch ls /usr/share/elasticsearch-plugin install --batch ingest-geoip

<br/>

### Инструкция по настройка есть на kibana

<br/>

![FileBeat & MetricBeat](/img/devops/linux/logging/elk/intall/filebeat-metricbeat-02.png 'FileBeat & MetricBeat'){: .center-image }

<br/>

### Centos7 клиент

    $ vagrant ssh centosvm01

https://www.elastic.co/guide/en/beats/filebeat/6.5/setup-repositories.html

    $ sudo rpm --import https://packages.elastic.co/GPG-KEY-elasticsearch


    $ sudo vi /etc/yum.repos.d/elastic.repo

```
[elastic-6.x]
name=Elastic repository for 6.x packages
baseurl=https://artifacts.elastic.co/packages/6.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
```

<br/>

    $ sudo yum install -y filebeat metricbeat net-tools

<br/>

### Ubuntu клиент

    $ cd ../ubuntu18/
    $ vagrant ssh

https://www.elastic.co/guide/en/beats/filebeat/6.5/setup-repositories.html

    $ wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -

    $ sudo apt-get install apt-transport-https

    $ echo "deb https://artifacts.elastic.co/packages/6.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-6.x.list

    $ sudo apt-get update
    $ sudo apt-get install -y filebeat metricbeat

<br/>

### Продолжение установки filebeat вне зависимости от выбранного дистрибутива

    $ sudo vi /etc/filebeat/filebeat.yml

В блоках

setup.kibana:
и
output.elasticsearch:

    host: "192.168.1.9:5601"

192.168.1.9 - ip host моей машины

<br/>
    $ sudo filebeat test config
    $ sudo filebeat test output

<br/>

    $ sudo filebeat modules enable system

    $ sudo filebeat modules list

<br/>

    // При необходимости более тонкой настройки отредактировать
    $ sudo vi /etc/filebeat/modules.d/system.yaml

<br/>

    $ sudo systemctl enable filebeat
    $ sudo filebeat setup
    $ sudo systemctl start filebeat
    $ sudo systemctl status filebeat

<br/>

http://localhost:5601/app/kibana

Dashboard -> [Filebeat System] Syslog dashboard

<br/>

![FileBeat & MetricBeat](/img/devops/linux/logging/elk/intall/filebeat-metricbeat-03.png 'FileBeat & MetricBeat'){: .center-image }

<br/>

![FileBeat & MetricBeat](/img/devops/linux/logging/elk/intall/filebeat-metricbeat-04.png 'FileBeat & MetricBeat'){: .center-image }

<br/>

### Тоже самое, только для MetricBeat

    $ sudo vi /etc/metricbeat/metricbeat.yml

    $ sudo metricbeat test config
    $ sudo metricbeat test output

    $ sudo systemctl enable metricbeat
    $ sudo metricbeat setup
    $ sudo systemctl start metricbeat
    $ sudo systemctl status metricbeat

<br/>

http://localhost:5601/app/kibana

Dashboard -> [Metricbeat System] Overview

<br/>

![FileBeat & MetricBeat](/img/devops/linux/logging/elk/intall/filebeat-metricbeat-05.png 'FileBeat & MetricBeat'){: .center-image }

<br/>

![FileBeat & MetricBeat](/img/devops/linux/logging/elk/intall/filebeat-metricbeat-06.png 'FileBeat & MetricBeat'){: .center-image }
