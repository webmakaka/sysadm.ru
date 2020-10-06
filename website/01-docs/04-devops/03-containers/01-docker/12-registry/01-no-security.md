---
layout: page
title: Собственный Docker Registry без Security
description: Собственный Docker Registry без Security
keywords: devops, docker, Собственный Docker Registry без Security
permalink: /devops/containers/docker/registry/no-security/
---

# Собственный Docker Registry без Security

Делаю:  
17.05.2019

По материалам **Implementing a Self-hosted Docker Registry**. Что лежин на большом трекере.

<br/>

# Deploying Your First Registry to Distribute Images

<br/>

    # vi /etc/hosts

    127.0.0.1 registry.local

<br/>

    $ docker run -it -d -p 5000:5000 --restart=always --name registry_local -v registry-data:/var/lib/registry registry:2

<!-- <br/>

    // Если потом нужно будет удалить volume:

    $ docker volume ls
    ***
    local               registry-data

    $ docker volume rm registry-data -->

<br/>

    // Проверка, если нужно
    $ docker pull busybox
    $ docker tag busybox registry.local:5000/busybox
    $ docker push registry.local:5000/busybox

<br/>

    $ curl registry.local:5000/v2/_catalog
    {"repositories":["busybox"]}

<br/>

**На клиенте**

<br/>

Нужно добавить поднятый registry в список тех, с кем может работать клиент.

<br/>

    # vi /etc/hosts

    192.168.0.11 registry.local

<br/>

    $ curl registry.local:5000/v2/_catalog

    $ docker info
    ***
    Insecure Registries:
    127.0.0.0/8

    # vi /etc/docker/daemon.json

```
{
      "insecure-registries": ["registry.local:5000"]
}
```

<br/>

    # systemctl daemon-reload
    # systemctl restart docker

<br/>

    $ docker info
    ***
    Insecure Registries:
    registry.local:5000
    127.0.0.0/8

    $ docker pull registry.local:5000/mongo
