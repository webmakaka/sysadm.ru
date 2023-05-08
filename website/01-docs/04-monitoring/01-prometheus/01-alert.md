---
layout: page
title: Prometheus Exporters
description: Prometheus Exporters
keywords: devops, linux, monitoring, prometheus
permalink: /monitoring/prometheus/alerts/
---

# Prometheus Exporters

**Делаю:**  
17.03.2021

<br/>

Идеи позаимствованы из курса: "Alerting on Issues with Prometheus Alertmanager"

Alertmanager download - [https://prometheus.io/download/#alertmanager](https://prometheus.io/download/#alertmanager)

<br/>

### [Запускаю Node Exporter](/monitoring/prometheus/)

<br/>

### Устанавливаю Alertmanager

```
$ cd ~/tmp

$ wget https://github.com/prometheus/alertmanager/releases/download/v0.21.0/alertmanager-0.21.0.linux-amd64.tar.gz

$ tar xvfz alertmanager-0.21.0.linux-amd64.tar.gz

$ cd alertmanager-0.21.0.linux-amd64

$ ./alertmanager --config.file=alertmanager.yml > alert.out 2>&1 &
```

<br/>

## Demo: Connecting Prometheus to Alertmanager

В каталоге, где лежит и prometheus.yml

<br/>

**$ vi rules.yml**

```yaml
groups:
  - name: example
    rules:
      - alert: InstanceDown
        expr: up == 0
        for: 1m
        labels:
          severity: critical
        annotations:
          summary: Instance is down
```

<br/>

Дефольный файл выглядит так:

prometheus.yml

```yaml
# my global config
global:
  scrape_interval: 15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
  # scrape_timeout is set to the global default (10s).

# Alertmanager configuration
alerting:
  alertmanagers:
    - static_configs:
        - targets:
          # - alertmanager:9093

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  # - "first_rules.yml"
  # - "second_rules.yml"

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: 'prometheus'

    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.

    static_configs:
      - targets: ['localhost:9090']

  - job_name: 'node_exporter'

    static_configs:
      - targets: ['172.31.27.27:9100']
```

<br/>

Финальный должен быть таким:

prometheus.yml

```yaml
# my global config
global:
  scrape_interval: 15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
  # scrape_timeout is set to the global default (10s).

# Alertmanager configuration
alerting:
  alertmanagers:
    - static_configs:
        - targets:
            - 192.168.1.9:9093

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  - 'rules.yml'

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  - job_name: 'node_exporter'

    static_configs:
      - targets: ['192.168.1.9:9100']
```

<br/>

Нужно зайти в контейнер и создать руками файл:

/etc/prometheus/rules.yml

<br/>

И перестартовать docker-compose

```
$ docker-compose restart prometheus
```

<br/>

Добавить файл как volume в docker-compose.yaml не получилось. Он собака монтировался как директория.

<br/>

**Node Exporter:**  
http://192.168.1.9:9100/metrics

**Alert Manager**  
http://localhost:9093/

**Prometheus**  
http://localhost:9091/

OK!

<br/>

# Sending Alerts with Receivers

Receiver documentation - [https://prometheus.io/docs/alerting/latest/configuration/#receiver](https://prometheus.io/docs/alerting/latest/configuration/#receiver)

## Demo: Slack Receiver

alertmanager.yml for slack receiver

```
global:
  #add your slack integration url below
  slack_api_url: 'https://hooks.slack.com…'

route:
  receiver: 'slack-notifications'

receivers:
- name: 'slack-notifications'
  slack_configs:
  - channel: '#prometheus-alerts'
    send_resolved: true
```

Reload Alertmanager config after changing `alertmanager.yml`

```
 sudo killall -HUP alertmanager
```

## Configuring an Email Receiver

alertmanager.yml for email receiver - gmail

```
global:

route:
  receiver: 'my-gmail'

receivers:
- name: my-gmail
  email_configs:
  - to: list@domain.com
    send_resolved: true
    from: my-email+alertmanager@gmail.com
    smarthost: smtp.gmail.com:587
    auth_username: my-email@gmail.com
    auth_identity: my-email@gmail.com
    auth_password: kfeydjkgighudfhe
```

alertmanager.yml for email receiver - other email service provider

```
global:

route:
  receiver: 'email'

receivers:
- name: email
  email_configs:
  - to: list@domain.com
    send_resolved: true
    from: email@mydomain.com
    smarthost: smtp-relay.sendinblue.com:587
    auth_username: email@mydomain.com
    auth_identity: email@mydomain.com
    auth_password: 3jduJ74JurdkD9Fv
```

## Demo: Webhook Receiver

alertmanager.yml for Zulip ([https://zulip.com/integrations/doc/alertmanager](https://zulip.com/integrations/doc/alertmanager))

```
global:
  resolve_timeout: 1m

route:
  receiver: 'zulip-notifications'

receivers:
- name: 'zulip-notifications'
  webhook_configs:
  - url: 'https://yourdomain.zulipchat.com/api/v1/external/alertmanager?api_key=adfaSDFES934asfdas8vasdvU37&stream=alertmanager'
    send_resolved: true
```

# Filtering, Managing, and Customizing Alerts

## Managing Alerts with Routing

test-rules.yml

```
groups:
- name: sample-alerts
  rules:
  - alert: App1Slow
    expr: 1
    labels:
      severity: warning
      service: app1
    annotations:
      summary: App 1 is running slow
  - alert: App1Down
    expr: 1
    labels:
      severity: critical
      service: app1
    annotations:
      summary: App 1 is down
  - alert: App2Down
    expr: 1
    labels:
      severity: critical
      service: app2
    annotations:
      summary: App 2 is down
  - alert: Server1LowDisk
    expr: 1
    labels:
      severity: warning
      service: servers
    annotations:
      summary: Low disk space on Server 1
  - alert: Server2LowDisk
    expr: 1
    labels:
      severity: warning
      service: servers
    annotations:
      summary: Low disk space on Server 2
  - alert: Server1Down
    expr: 1
    labels:
      severity: critical
      service: servers
    annotations:
      summary: Server1 is down
  - alert: Server2Down
    expr: 1
    labels:
      severity: critical
      service: servers
    annotations:
      summary: Server 2 is down
  - alert: NetworkDown
    expr: 1
    labels:
      severity: critical
      service: network
    annotations:
      summary: Network is down
```

You can validate the rules file using `promtool` which is included in the `prometheus` directory

```
./promtool check rules test-rules.yml
```

Make sure to reference the new `test-rules.yml` file in `prometheus.yml`. Also removed `node_exporter` for this example.

prometheus.yml

```
# my global config
global:
  scrape_interval:     15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
  scrape_timeout: 10s

# Alertmanager configuration
alerting:
  alertmanagers:
  - static_configs:
    - targets:
      - localhost:9093

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  - "test-rules.yml"

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: 'prometheus'

    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.

    static_configs:
    - targets: ['localhost:9090']
```

Then start or restart prometheus however you configured it.

```
sudo service prometheus start
```

alertmanager.yml file

```
route:
  receiver: 'email' #default
  routes:
    - match:
        service: app1
      reciever: 'z-dev-team1'
    - match:
        service: app2
      reciever: 'z-dev-team2'
    - match:
        service: servers
      reciever: 'z-server-team'
    - match:
        service: network
      reciever: 'z-network-team'

receivers:
- name: 'z-dev-team1'
  webhook_configs:
    - url: 'https://yourdomain.zulipchat.com/api/v1/external/alertmanager?api_key=asdf&stream=DevTeam1'
      send_resolved: true
- name: 'z-dev-team2'
  webhook_configs:
    - url: 'https://yourdomain.zulipchat.com/api/v1/external/alertmanager?api_key=asdf&stream=DevTeam2'
      send_resolved: true
- name: 'z-server-team'
  webhook_configs:
    - url: 'https://yourdomain.zulipchat.com/api/v1/external/alertmanager?api_key=asdf&stream=Servers'
      send_resolved: true
- name: 'z-network-team'
  webhook_configs:
    - url: 'https://yourdomain.zulipchat.com/api/v1/external/alertmanager?api_key=asdf&stream=Network'
      send_resolved: true
- name: email
  email_configs:
  - to: list@yourdomain.com
    send_resolved: true
    from: alert@yourdomain.com
    smarthost: smtp-relay.sendinblue.com:587
    auth_username: user@yourdomain.com
    auth_identity: user@yourdomain.com
    auth_password: 4jdisCHl043S2rNi
```

Validate file

```
./amtool check-config alertmanager.yml
```

Visualization for routing tree (be sure to remove any sensitive data before trying with your routes)

[https://prometheus.io/webtools/alerting/routing-tree-editor/](https://prometheus.io/webtools/alerting/routing-tree-editor/)

## Grouping Alerts

Alerts have some grouping by default. To turn off all grouping add the following under route

```
route:
  group_by ['...']
```

Group by values of the `service` label to the same receiver to create one message for all alerts that have the same value for `service`

alertmanager.yml

email:

```
route:
  group_by: ['service']
  receiver: 'email'

receivers:
- name: email
  email_configs:
  - to: list@yourdomain.com
    send_resolved: true
    from: alert@yourdomain.com
    smarthost: smtp-relay.sendinblue.com:587
    auth_username: user@yourdomain.com
    auth_identity: user@yourdomain.com
    auth_password: 4jdisCHl043S2rNi
```

zulip:

```
route:
  group_by: ['service']
  receiver: 'z-alerts'

receivers:
- name: 'z-alerts'
  webhook_configs:
    - url: 'https://yourdomain.zulipchat.com/api/v1/external/alertmanager?api_key=asdf&stream=Alerts'
      send_resolved: true
```

Add an alert with a new value for the `service` label (`app3`) and it will get grouped into its own message.

test-rules.yml

```
groups:
- name: sample-alerts
  rules:
  - alert: App3Down
    expr: 1
    labels:
      severity: critical
      service: app3
    annotations:
      summary: App 3 is down
```

## Managing Alerts with Throttling and Repetition

Default values for Alertmanager delivery settings

```
route:
  group_by: ['service']
  group_wait: 30s     #default
  group_interval: 5m  #default
  repeat_interval: 4h #default
  receiver: 'z-alerts'
```

Add an alert to observe `group_wait`

```
  - alert: App5Down
    expr: 1
    labels:
      severity: critical
      service: app5
    annotations:
      summary: App 5 is down
```

## Filtering Alerts with Inhibition and Silencing

alertmanager.yml

```
- inhibit_rules:
  - source_match:
      service: 'network'
    target_match:
      service: 'servers'

  - source_match:
      severity: 'critical’
    target_match:
      severity: 'warning'
    equal: ['service']
```

## Taylor Alerts with Notification Templates

Default Alertmanager template: [https://github.com/prometheus/alertmanager/blob/master/template/default.tmpl](https://github.com/prometheus/alertmanager/blob/master/template/default.tmpl)

Notification template reference:

[https://prometheus.io/docs/alerting/latest/notifications/](https://prometheus.io/docs/alerting/latest/notifications/)

Adding an info line referencing the expression value

```
- alert: NetworkDown
    expr: 1
    labels:
      severity: critical
      service: network
    annotations:
      summary: Network is down
      info:  'Expression evaluating at {{ $value }}'
```

Override template values

```
receivers:
- name: 'slack-notifications'
  slack_configs:
  - channel: '#prometheus-alerts'
    send_resolved: true
    text: 'Custom text message in Slack notification'
```

Creating a template file

/yourpath/alertmanager/templates/custom.tmpl

```
{{ define "slack.custom.text" }}Custom text message in Slack notification from template file{{ end }}
```

alertmanager.yml

```
receivers:
- name: 'slack-notifications'
  slack_configs:
  - channel: '#alerts'
    text: '{{ template "slack.custom.text" . }}'

templates:
- '/yourpath/alertmanager/templates/custom.tmpl'
```
