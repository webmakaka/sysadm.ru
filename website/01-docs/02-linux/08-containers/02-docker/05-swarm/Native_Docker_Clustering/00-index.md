---
layout: page
title: Docker Swarm > Native Docker Clustering [2016, ENG]
permalink: /linux/containers/docker/swarm/Native_Docker_Clustering/
---

# Docker Swarm: Native Docker Clustering [2016, ENG]

<br/>

<div align="center">
    <img src="//files.sysadm.ru/img/linux/containers/docker/swarm/native-docker-clustering/pic1.png" border="0" alt="Native Docker Clustering">
</div>

<br/>

https://github.com/progrium/busybox


https://hub.docker.com/r/progrium/consul/


Походу вот такой dockerfile, но у меня он не собрался, т.к. минимум ссылки на консул протухли.


<br/>

    $ vi Dockerfile

<br/>


**Dockerfile (progrium/consul)**

    FROM 		progrium/busybox
    MAINTAINER 	Jeff Lindsay <progrium@gmail.com>

    ADD https://dl.bintray.com/mitchellh/consul/0.4.0_linux_amd64.zip /tmp/consul.zip
    RUN cd /bin && unzip /tmp/consul.zip && chmod +x /bin/consul && rm /tmp/consul.zip

    ADD https://dl.bintray.com/mitchellh/consul/0.4.0_web_ui.zip /tmp/webui.zip
    RUN cd /tmp && unzip /tmp/webui.zip && mv dist /ui && rm /tmp/webui.zip

    ADD https://get.docker.io/builds/Linux/x86_64/docker-1.2.0 /bin/docker
    RUN chmod +x /bin/docker

    RUN opkg-install curl bash

    ADD ./config /config/
    ONBUILD ADD ./config /config/

    ADD ./start /bin/start
    ADD ./check-http /bin/check-http
    ADD ./check-cmd /bin/check-cmd

    EXPOSE 8300 8301 8301/udp 8302 8302/udp 8400 8500 53/udp
    VOLUME ["/data"]

    ENV SHELL /bin/bash

    ENTRYPOINT ["/bin/start"]
    CMD []


<br/>
<br/>

**Dockerfile (gliderlabs/registrator) хз, не искал**

**Dockerfile (progrium/busybox) хз, не искал**


<br/>
<br/>

<ul>
    <li>
        <a href="/linux/containers/docker/swarm/Native_Docker_Clustering/Building_Your_Swarm_Infrastructure/">Module 4: Building your Swarm Infrastructure</a>
    </li>
    <li>
        <a href="/linux/containers/docker/swarm/Native_Docker_Clustering/Securing_your_Swarm_Cluster/">Module 5: Securing your Swarm Cluster</a>
    </li>
</ul>
