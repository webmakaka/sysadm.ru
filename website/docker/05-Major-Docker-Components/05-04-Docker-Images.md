---
layout: page
title: 05 04 Docker Images
permalink: /docker/major-docker-components/docker-images/
---


## 05 Major Docker Components

05 04 Docker Images


    docker run -it fedora /bin/bash

    // взять из репо последнюю версию федоры
    docker pull fedora

    // взять все версии федоры
    docker pull -a fedora

    // получить список скачанных версий
    docker images fedora


    docker pull coreos/etcd


    ============================

    containers

    docker run -it ubuntu /bin/bash

    Подключились с контейнеру fedora.
    Отключились от него командой CTRL + P + Q


==================

06 04 Where Images Are Stored

$ docker images
$ docker pull coreos/apache
docker images --tree


06 05 Copying Images to Other Hosts


docker run ubuntu /bin/bash -c "echo 'cool content' > /tmp/cool-file"

docker ps -a

docker commit <container_id> fridge

docker images
docker images --tree

dcoker history fridge


docker save -o /tmp/fridge.tar fridge

ls -lh /tmp/fridge.tar

скопировали на centos

tar -tf /tmp/fridge.tar

docker load -i /tmp/fridge.tar

docker images

docker run -it fridge /bin/bash

cat /tmp/cool-file

====================================

06 07 One Process per Container

docker run -d ubuntu /bin/bash -c "ping 8.8.8.8 -c 30"

docker top <container_id>

====================================

06 08 Commands for Working with Containers


docker run -d ubuntu:14.4.1 /bin/bash -c "ping 8.8.8.8"
docker inspect <container_id>

========================

docker run -it ubuntu:14.4.1 /bin/bash
CTRL + P + Q

docker stop <container_id>

// Последний стартовавший контейнер.
docker ps -l

docker attach <container_id>

docker restart <container_id>

=================================

Удаляем контейнеры

ls -l /var/lib/docker/containers
ls -l /var/lib/docker/containers | wc -l
docker rm  <container_id>

docker rm -f <container_id>


==================================

docker top <container_id>

ps -ef

docker logs <container_id>


====================================

Подключиться к контейнеру

docker inspect <container_id> | grep Pid

nsenter -m -u -n -p -i -t 1923 /bin/bash
exit
docker-enter <container_id>
exit

docker exec -it <container_id> /bin/bash


===========


vi Dockerfile

#Ubuntu based Hello World container
FROM ubuntu:15.04
MAINTAINER test@example.com
RUN apt-get update
cmd ["echo", "Hello world"]


docker build -t helloworld:0.1 .
docker run helloworld:0.1

=============

09

https://registry.hub.docker.com/  
hub.docker.com

Создали репо

docker images
docker tag <image_id> nigelpoulton/helloworld:1.0
doceker push nigelpoulton/helloworld:1.0

doceker pull nigelpoulton/helloworld:1.0
==========


docker run -d -p 5000:5000 registry
docker images
docker ps
============================

Container ubuntu -> debian -> centos

docker images
docker tag <image_id> debian8.docker.course:5000/priv-test
docker push debian8.docker.course:5000/priv-test

docker run -d debian8.docker.course:5000/priv-test

===============================

Dockerfile2

docker build -t="build1" .
docker build -t="build2" .

----

vi Dockerfile

#Ubuntu based Hello World container
FROM ubuntu:15.04
MAINTAINER test@example.com
RUN apt-get update
RUN apt-get install -y apache2
RUN apt-get install -y vim
RUN apt-get install -y golang
cmd ["echo", "Hello world"]


docker history <container_id>

====


vi Dockerfile

#Ubuntu based Hello World container
FROM ubuntu:15.04
MAINTAINER test@example.com
RUN apt-get update
RUN apt-get install -y apache2
RUN apt-get install -y apache2-utils
RUN apt-get install -y vim
RUN apt-get clean

EXPOSE 80

cmd ["apache2ctl", "-D", "FOREGROUND"]

docker build -t="webserver" .


docker run -d -p 80:80 webserver


=================


vi Dockerfile

#Ubuntu based Hello World container
FROM ubuntu:15.04
MAINTAINER test@example.com
RUN apt-get update \
        && apt-get install -y apache2 apache2-utils vim \
        && apt-get clean \
        && rm -rf /var/lib/apt/lists/* /tmp/* var/tmp*

EXPOSE 80

cmd ["apache2ctl", "-D", "FOREGROUND"]




http://localhost:80



===================


vi Dockerfile

#Ubuntu based Hello World container
FROM ubuntu:15.04
MAINTAINER test@example.com
RUN apt-get update \
    && apt-get install -y iputils-ping apache2


ENTRYPOINT ["apache2ctl"]


docker run -d -p 80:80 web2 -D FOREGROUND

=====================

vi Dockerfile

#Ubuntu based Hello World container
FROM ubuntu:15.04
MAINTAINER test@example.com
RUN apt-get update \
&& apt-get install -y iputils-ping apache2

ENV var1=ping var=8.8.8.8

CMD $var1 $var2

docker build -t="pinger" .

docker run -d pinger

docker logs -f <containter_id>


env

=====================


docker run -it -v /test-vol --name=voltainer ubuntu:15.04 /bin/bash

conrainer
  nano /test-vol/testfile

docker inspect voltainer

===

docker run -it --volumes-from=voltainer ubuntu:15.04 /ban/bash


===

docker run -it --volumes-from=voltainer ubuntu:15.04 /ban/bash

=================================
=================================
=================================

11 networking

apt-get install bridge-utils
yum install bridge-utils

brctl show docker0



vi Dockerfile

#Test for networking module
FROM ubuntu:15.04

MAINTAINER test@example.com

RUN apt-get update \
&& apt-get install -y iputils-ping traceroute

ENTRYPOINT ["/bin/bash"]



docker run -it --name=net1 net-img
docker run -it --name=net2 net-img

CTRL+P+Q

brctl show


=====================================

docker port <container_id>

$ docker port railsdev
1337/tcp -> 0.0.0.0:1337
3000/tcp -> 0.0.0.0:3000
8080/tcp -> 0.0.0.0:80
9000/tcp -> 0.0.0.0:9000


=====================================
src - source
rcvr - reciever
ali-src - alias

docker run --name=src -d img

docker run --name=rcvr --link=src:ali-src -it ubuntu:15.04 /bin/bash


docker inspect rcvr

docker attach rcvr
env
env | grep ALI
cat /etc/hosts

===============================================
===============================================

12

docker -d -l debug &
