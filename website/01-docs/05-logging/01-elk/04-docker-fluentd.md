---
layout: page
title: How to install EFK stack using Docker with Fluentd
description: How to install EFK stack using Docker with Fluentd
keywords: linux, logging, elk, docker, fluentd
permalink: /linux/logging/elk/docker-fluentd/
---

# How to install EFK stack using Docker with Fluentd

<br/>

Делаю  
24.04.2019

По материалам индуса:

https://www.youtube.com/watch?v=MNId4HG0wV8&list=PL34sAs7_26wOgpqMW_0_E95k9tq2VkMOZ&index=3

<br/>
Рисунки индуса:
<br/>

![elk docker Fluentd](/img/logging/elk/intall/elk-docker-fluentd-01.png 'elk docker Fluentd'){: .center-image }

<br/>

![elk docker Fluentd](/img/logging/elk/intall/elk-docker-fluentd-02.png 'elk docker Fluentd'){: .center-image }

<br/>

    # vi /etc/sysctl.conf

Добавляем:

```
#################
# User defined
#
vm.max_map_count=262144
```

<br/>

    // Применит параметры до перезагрузки
    # sysctl -w vm.max_map_count=262144

    # sysctl -a | grep vm.max_map_count

<br/>

    $ mkdir ~/tmp && cd ~/tmp
    $ git clone https://bitbucket.org/sysadm-ru/elk.git

    $ cd ~/tmp/elk/docker-efk/
    $ docker-compose up -d

<br/>

    $ docker-compose ps
        Name                   Command               State                              Ports
    ---------------------------------------------------------------------------------------------------------------------
    elasticsearch   /usr/local/bin/docker-entr ...   Up      0.0.0.0:9200->9200/tcp, 9300/tcp
    fluentd         /bin/entrypoint.sh /bin/sh ...   Up      0.0.0.0:24224->24224/tcp, 0.0.0.0:24224->24224/udp, 5140/tcp
    kibana          /usr/local/bin/kibana-docker     Up      0.0.0.0:5601->5601/tcp

<br/>

    $ docker-compose logs kibana | less
    $ docker-compose logs elasticsearch | less
    $ docker-compose logs fluentd | less
    $ sudo netstat -nltp | grep docker
    tcp6       0      0 :::9200                 :::*                    LISTEN      2713/docker-proxy
    tcp6       0      0 :::24224                :::*                    LISTEN      2678/docker-proxy
    tcp6       0      0 :::5601                 :::*                    LISTEN      3070/docker-proxy

<br/>

http://localhost:5601

<br/>

### Создаем клиента, который будет кидать логи

    $ lxc launch images:centos/7 efkclient

    $ lxc list
    +-----------+---------+---------------------+------+------------+-----------+
    |   NAME    |  STATE  |        IPV4         | IPV6 |    TYPE    | SNAPSHOTS |
    +-----------+---------+---------------------+------+------------+-----------+
    | efkclient | RUNNING | 10.81.125.56 (eth0) |      | PERSISTENT | 0         |
    +-----------+---------+---------------------+------+------------+-----------+

    $ lxc exec efkclient bash


    # yum install -y sudo net-tools

https://docs.fluentd.org/v1.0/articles/install-by-rpm

    # curl -L https://toolbelt.treasuredata.com/sh/install-redhat-td-agent3.sh | sh

<br/>

    # systemctl enable td-agent

<br/>

    # vi /etc/td-agent/td-agent.conf

Удаляем все нафиг и заменяем на

```
<source>
  @type syslog
  @id input_syslog
  port 42185
  tag efkclient.system
</source>

<match *.**>
  @type forward
  @id forward_syslog
  <server>
    host 192.168.1.9
  </server>
</match>
```

192.168.1.9 - ip моей хост машины

<br/>

    # systemctl start td-agent

<br/>

    # cd /var/log/td-agent/
    # less td-agent.log

<br/>

### Rsyslog отправляем fluetd

    # vi /etc/rsyslog.conf

Добавляем в конец

```
#################
# User defined
#
*.* @127.0.0.1:42185
```

<br/>

    # systemctl restart rsyslog

<br/>

    # netstat -nlup
    Active Internet connections (only servers)
    Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
    udp        0      0 0.0.0.0:42185           0.0.0.0:*                           999/ruby
    udp        0      0 0.0.0.0:68              0.0.0.0:*                           248/dhclient
    udp        0      0 0.0.0.0:53351           0.0.0.0:*                           1028/rsyslogd

<br/>

### Смотрим логи в кибане

http://localhost:5601

![elk docker Fluentd](/img/logging/elk/intall/elk-docker-fluentd-03.png 'elk docker Fluentd'){: .center-image }

![elk docker Fluentd](/img/logging/elk/intall/elk-docker-fluentd-04.png 'elk docker Fluentd'){: .center-image }

![elk docker Fluentd](/img/logging/elk/intall/elk-docker-fluentd-05.png 'elk docker Fluentd'){: .center-image }

![elk docker Fluentd](/img/logging/elk/intall/elk-docker-fluentd-06.png 'elk docker Fluentd'){: .center-image }

<br/>

### Отправляем сообщения в лог

    # logger -t JUNGLE hello this is from the efkclient for testing
    # tail -f /var/log/messages

![elk docker Fluentd](/img/logging/elk/intall/elk-docker-fluentd-07.png 'elk docker Fluentd'){: .center-image }

<br/>

### Останавливаем все это добро

    $ docker-compose down
    $ lxc delete efkclient --force
    $ lxc list
