---
layout: page
title: Инсталляция Elasticsearch в виртуальной машине VirtualBox Ubuntu 18
permalink: /linux/logging/elk/install/vbox-ubuntu/
---

# Инсталляция Elasticsearch в виртуальной машине VirtualBox Ubuntu 18

<br/>

Делаю  
25.10.2019

По материалам видеокурса:  
**[Sundog Education by Frank Kane] Elasticsearch 6 and Elastic Stack - In Depth and Hands On!**

<br/>

**И статьи к данному видеокурсу:**  
https://sundog-education.com/elasticsearch/

<br/>

### Запускаю виртуальную машину

    $ mkdir ~/vagrant-elk && cd ~/vagrant-elk

    # git clone https://bitbucket.org/sysadm-ru/vagrant .

    $ cd vagrantfiles/ubuntu18/

    $ vi Vagrantfile

    v.memory = 8096

    $ vagrant up

    $ vagrant ssh

<br/>

### Донастройка виртуальной машины

![elk ubuntu vbox 01](/img/adm/logging/elk/intall/elk-ubuntu-vbox-01.png 'elk ubuntu vbox 01'){: .center-image }

<br/>

![elk ubuntu vbox 02](/img/adm/logging/elk/intall/elk-ubuntu-vbox-02.png 'elk ubuntu vbox 02'){: .center-image }

<br/>

    $ sudo apt update
    $ sudo apt-get install openjdk-8-jre-headless -y
    $ sudo apt-get install openjdk-8-jdk-headless -y

<br/>

    $ wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -

    $ sudo apt-get install apt-transport-https

    $ echo "deb https://artifacts.elastic.co/packages/6.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-6.x.list

    $ sudo apt-get update && sudo apt-get install elasticsearch

<br/>

    $ sudo vi /etc/elasticsearch/elasticsearch.yml

    #network.host: 192.168.0.1

    меняю на

    network.host: 0.0.0.0

<br/>

    $ sudo /bin/systemctl daemon-reload
    $ sudo /bin/systemctl enable elasticsearch.service
    $ sudo /bin/systemctl start elasticsearch.service

<br/>

Можно выполнять с localhost

    $  curl 127.0.0.1:9200

<br/>

```
$ curl -XGET "http://localhost:9200/_search" -H 'Content-Type: application/json' -d'
{
  "query": {
    "match_all": {}
  }
}'
```

<br/>

### Kibana

    $ sudo apt-get install kibana

    $ sudo vi /etc/kibana/kibana.yml

// Утанавливаю
server.host: 0.0.0.0

    $ sudo /bin/systemctl daemon-reload
    $ sudo /bin/systemctl enable kibana.service
    $ sudo /bin/systemctl start kibana.service

<!--

    $ curl -H 'Content-Type: application/json' -XPOST 127.0.0.1:9200/product/default/_bulk/pretty" --data-binary @11.2 test-data.json.json

    GET /_cat/health?v
    GET /_cat/nodes?v
    GET /_cat/indices?v
    GET /_cat/allocation?v
    GET /_cat/shards?v

-->

<br/>

### Добавляем какие-то данные в ElasticSearch

    $ wget http://media.sundog-soft.com/es6/shakes-mapping.json

    $ curl -H 'Content-Type: application/json' -XPUT 127.0.0.1:9200/shakespeare --data-binary @shakes-mapping.json

    $ wget http://media.sundog-soft.com/es6/shakespeare_6.0.json

    $ curl -H 'Content-Type: application/json' -X POST 'localhost:9200/shakespeare/doc/_bulk?pretty' --data-binary @shakespeare_6.0.json

    $ curl -H 'Content-Type: application/json' -XGET '127.0.0.1:9200/shakespeare/_search?pretty' -d ' {
    "query" : {
    "match_phrase" : {
    "text_entry" : "to be or not to be"
    }
    }
    }
    '
