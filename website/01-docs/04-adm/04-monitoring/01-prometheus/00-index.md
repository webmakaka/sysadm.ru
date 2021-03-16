---
layout: page
title: Prometheus Exporters
description: Prometheus Exporters
keywords: devops, linux, monitoring, prometheus
permalink: /adm/monitoring/prometheus/
---

# Prometheus Exporters

**Делаю:**  
16.03.2021

<br/>

## Node Exporter

**node_exporter**  
https://prometheus.io/download/#node_exporter

<br/>

```shell script

$ cd ~/tmp

#Download to machine
$ wget https://github.com/prometheus/node_exporter/releases/download/v1.1.2/node_exporter-1.1.2.linux-amd64.tar.gz

#unpack the tar file
$ tar xvfz node_exporter-1.1.2.linux-amd64.tar.gz

#go into directory
$ cd node_exporter-1.1.2.linux-amd64/

#start the exporter, log output to file, send process to background (you may want to set up the exporter as a service)
$ ./node_exporter > node.out 2>&1 &


# 192.168.1.9 - host ip
#verify the exporter is exposing metrics
$ curl http://192.168.1.9:9100/metrics
```

```
$ cd ~/tmp

$ git clone https://github.com/marcel-dempers/docker-development-youtube-series

$ cd docker-development-youtube-series/monitoring/prometheus/
```

<br/>

**Prometheus Configuration**

**prometheus.yml**

<br/>

```yaml
scrape_configs:
```

<br/>

**Добавляю:**

```yaml
- job_name: 'node_exporter'
  static_configs:
      - targets: ['192.168.1.9:9100']
```

<br/>

```
$ docker-compose up -d prometheus
```

<br/>

http://localhost:9091/

OK!

<br/>

## MySQL exporter

https://prometheus.io/docs/instrumenting/exporters/#databases

mysqld exporter download page for link to latest file

https://prometheus.io/download/#mysqld_exporter

<br/>

```
$ sudo mysql
```

```sql
--create a user for the exporter to connect to the database
CREATE USER 'mysqld_exporter'@'localhost' IDENTIFIED BY 'password';

GRANT PROCESS, REPLICATION CLIENT, SELECT ON *.* TO 'mysqld_exporter'@'localhost';

FLUSH PRIVILEGES;

EXIT
```

```shell script
#create an environment variable for the exporter to get credentials
export DATA_SOURCE_NAME='mysqld_exporter:password@(localhost:3306)/'
```

<br/>

```

$ mkdir ~/tmp
$ cd ~/tmp

$ wget https://github.com/prometheus/mysqld_exporter/releases/download/v0.12.1/mysqld_exporter-0.12.1.linux-amd64.tar.gz

$ tar xvfz mysqld_exporter-0.12.1.linux-amd64.tar.gz

$ cd mysqld_exporter-0.12.1.linux-amd64
$ ./mysqld_exporter > mysqld.out 2>&1 &

$ curl http://localhost:9104/metrics
```

<br/>

**Prometheus Configuration**

**prometheus.yml**

<br/>

```yaml
scrape_configs:
    - job_name: 'mysqld_exporter'
      static_configs:
          - targets: ['192.168.0.11:9104']
```

<br/>

## Blackbox exporter

Blackbox exporter download page for link to latest file

https://prometheus.io/download/#blackbox_exporter

blackbox.yml configuration options
https://github.com/prometheus/blackbox_exporter/blob/master/CONFIGURATION.md

```shell script
#download, extract, view files
$ wget https://github.com/prometheus/blackbox_exporter/releases/download/v0.17.0/blackbox_exporter-0.17.0.linux-amd64.tar.gz

$ tar xvfz blackbox_exporter-0.17.0.linux-amd64.tar.gz
$ cd blackbox_exporter-0.17.0.linux-amd64/
```

blackbox.yml - modify http_2xx module to accept IPv4 as successful probe

```yaml
modules:
    http_2xx:
        prober: http
        http:
            preferred_ip_protocol: 'ip4'
```

```shell script
#Run exporter and check that it's up
./blackbox_exporter > blackbox.out 2>&1 &
curl http://localhost:9115/metrics
```

### Prometheus Configuration

prometheus/prometheus.yml

```yaml
# my global config
global:
    scrape_interval: 15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
    evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
    scrape_timeout: 10s
```

...\<snip>...

```yaml
scrape_configs:
    - job_name: 'blackbox'
      metrics_path: /probe
      params:
          module: [http_2xx]
      static_configs:
          - targets: ['https://www.pluralsight.com', 'http://www.prometheus.io']
      relabel_configs:
          - source_labels: [__address__]
            target_label: __param_target
          - source_labels: [__param_target]
            target_label: instance
          - target_label: __address__
            replacement: 172.31.40.148:9115
```

<br/>

### Monitoring Kubernetes

Prometheus Operator Github page

https://github.com/prometheus-operator/prometheus-operator

Helm documentation

https://helm.sh/docs/intro/using_helm/

Installing Helm and prometheus-operator chart

```shell script
#install helm
curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash

#add repo for prometheus-operator
helm repo add stable https://kubernetes-charts.storage.googleapis.com

#install prometheus-operator chart
helm install my-prometheus-operator stable/prometheus-operator
```

Access Prometheus dashboard using preferred method (port forward, set up service, etc.)

<br/>

### Monitoring Messaging Systems

Prometheus Exporters

https://prometheus.io/docs/instrumenting/exporters/#messaging-systems

RabbitMQ - enable the plugin on your RabbitMQ server

```shell script
rabbitmq-plugins enable rabbitmq_prometheus
```

<br/>

### Prometheus Configuration

prometheus/prometheus.yml

```yaml
scrape_configs:
    - job_name: 'rabbitmq_exporter'
      static_configs:
          - targets: ['172.31.20.86:15692']
```
