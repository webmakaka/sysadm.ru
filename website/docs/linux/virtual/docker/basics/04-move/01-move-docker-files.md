---
layout: page
title: Переместить файлы Docker в другой каталог
permalink: /linux/virtual/docker/basics/move-docker-files/
---

Разожрался докер, пожрал все ресурсы, да еще и на системном разделе

    # du -hx --max-depth=1 /var/lib/docker/ | sort -h
    4.0K	/var/lib/docker/tmp
    8.0K	/var/lib/docker/trust
    132K	/var/lib/docker/execdriver
    776K	/var/lib/docker/volumes
    1.5M	/var/lib/docker/graph
    23M	/var/lib/docker/init
    37M	/var/lib/docker/containers
    3.1G	/var/lib/docker/vfs
    12G	/var/lib/docker/aufs
    15G	/var/lib/docker/


<br/>


    # service docker stop

<br/>

    # mv /var/lib/docker /mnt/dsk2/docker


<br/>

    # vi /etc/default/docker

Дописываем

    DOCKER_OPTS="-g /mnt/dsk2/docker"

<br/>

    # service docker start
