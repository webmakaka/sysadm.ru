---
layout: page
title: Self-hosted Registry
permalink: /linux/servers/containers/docker/self-hosted-registry/
---

# Self-hosted Registry

Делаю:  
11.05.2019

По материалам **Implementing a Self-hosted Docker Registry**. Что лежин на большом трекере.

<br/>

# Deploying Your First Registry to Distribute Images

<br/>

    # vi /etc/hostname

    registry.local

<br/>

    # vi /etc/hosts

    127.0.0.1 registry.local

<br/>


    $ docker run -it -d -p 5000:5000 --name registry_local -v registry-data:/var/lib/registry registry:2


    // Если потом нужно будет удалить volume:

    $ docker volume ls
    ***
    local               registry-data

    $ docker volume rm registry-data


<!-- $ docker run --rm -it -p 5000:5000 registry:2 -->

<!-- $ curl registry.local:5000/v2/_catalog -->

<br/>


    $ docker pull mongo
    $ docker tag mongo registry.local:5000/mongo
    $ docker push registry.local:5000/mongo

<br/>

    $ curl registry.local:5000/v2/_catalog

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

    $ systemctl daemon-reload
    $ systemctl restart docker

<br/>

    $ docker info
    ***
    Insecure Registries:
    registry.local:5000
    127.0.0.0/8

    $ docker pull registry.local:5000/mongo


<br/>

# Registry Mirroring with a Pull-through Cache


    $ mkdir mirror
    $ cd mirror/
    $ vi docker-compose.yml

https://github.com/g0t4/course-implementing-self-hosted-docker-registry/blob/master/mirror/docker-compose.yml

    $ docker-compose up
    $ curl registry.local:5000/v2/_catalog
    {"repositories":[]}

<br/>

**На клиенте**

    # vi /etc/docker/daemon.json

Возможно, что insecure-registries здесь не нужно.

```
{
      "insecure-registries": ["registry.local:5000"],
      "registry-mirrors": ["http://registry.local:5000"]
}
```

    $ sudo systemctl daemon-reload
    $ sudo systemctl restart docker

<br/>

    $ docker system info
    ***
    Insecure Registries:
    registry.local:5000
    127.0.0.0/8
    Registry Mirrors:
    http://registry.local:5000/

<br/>

    $ time docker image pull node
    ***
    real	1m17.225s


    $ curl registry.local:5000/v2/_catalog
    {"repositories":["library/node"]}

    $ docker image rm node

    $ time docker image pull node
    ***
    real	0m21.755s

<br/>

# Вариант с самоподписанным TLS сертификатом

    $ cd ~
    $ git clone https://github.com/g0t4/course-implementing-self-hosted-docker-registry
    $ cd course-implementing-self-hosted-docker-registry/security/tls/
    $ docker-compose up

<br/>

https://192.168.0.11/v2/_catalog


<br/>

    # vi /etc/hosts

    192.168.0.11 example.com

<br/>

    $ cd ~
    $ docker cp tls_secured_1:/certs .
    $ sudo cp certs/selfsigned.crt /usr/local/share/ca-certificates/
    $ sudo update-ca-certificates

<br/>

    $ sudo systemctl daemon-reload
    $ sudo systemctl restart docker


<br/>

    $ docker image tag busybox example.com/busybox
    $ docker image push example.com/busybox


<br/>

**На клиенте**

<br/>

    # vi /etc/hosts

    192.168.0.11 example.com

<br/>

Скопировал с помощью sftp файлы на клиент

<br/>

    $ sudo cp certs/selfsigned.crt /usr/local/share/ca-certificates/
    $ sudo update-ca-certificates

<!-- <br/>

    $ sudo systemctl daemon-reload
    $ sudo systemctl restart docker -->

<br/>

    $ docker pull mongo
    $ docker tag mongo example.com/mongo
    $ docker push example.com/mongo


<br/>

    $ curl --insecure https://example.com/v2/_catalog
    {"repositories":["busybox","mongo"]}



<br/>

# Docker registry frontend (WEB GUI для registry)

    http://192.168.0.11:5000/v2/mongo/tags/list

<br/>

```
$ docker run \
  -d \
  -e ENV_DOCKER_REGISTRY_HOST=192.168.0.11 \
  -e ENV_DOCKER_REGISTRY_PORT=5000 \
  -p 8080:80 \
  konradkleine/docker-registry-frontend:v2
```

http://192.168.0.11:8080/home

<br/>

# Storing Regisry Data on a Named Volume

    $ docker run --rm -it -d -p 5000:5000 -v registry-data:/var/lib/registry registry:2






<!-- <br/>

# Automating Builds with Notifications

<br/>

### 04.Automating Builds with Notifications

    $ git clone --single-branch --branch docker-registry https://github.com/g0t4/docker-mongo-sample-datasets
    $ cd docker-mongo-sample-datasets/
    $ docker-compose up

http://192.168.0.11:8090  
http://192.168.0.11:8091

<br/>

https://docs.docker.com/registry/notifications/#configuration

<br/>

    $ ~/mirror
    $ docker-compose up

    $ docker ps | grep mirror
    47ddcc892ce6        registry:2          "/entrypoint.sh /etc…"   2 hours ago         Up 28 seconds       0.0.0.0:5000->5000/tcp   mirror_registry_1


<br/>

В общем текущий конфиг registry можно скопировать командой.

    $ docker container cp mirror_registry_1:/etc/docker/registry/config.yml .

И чтобы работали notification, нужно добавить соответствующий блок.

<br/>

    $ cd ~
    $ git clone https://github.com/g0t4/course-implementing-self-hosted-docker-registry
    $ cd course-implementing-self-hosted-docker-registry/mongo/
    $ docker-compose up


    # apt  install jq

    $ docker-compose logs --follow vetted-registry \
	| grep --line-buffered -o '{.*}' \
    | jq 

<br/>

    // compact & minimal fields:
    $ docker-compose logs --follow vetted-registry \
	| grep --line-buffered -o '{.*}' \
	| jq -c '{ time, level: .level[0:4], msg }'

<br/>

http://192.168.0.11:8000

<br/>

jenkins

// admin/admin
http://192.168.0.11:8190

<br/>

# push an image to vetted-registry (pull and tag busybox for this purpose)

    $ docker pull busybox
    $ docker tag busybox localhost:55000/busybox
    $ docker push localhost:55000/busybox

<br/>

    $ docker-compose up --force-recreate --renew-anon-volumes vetted-registry -->
