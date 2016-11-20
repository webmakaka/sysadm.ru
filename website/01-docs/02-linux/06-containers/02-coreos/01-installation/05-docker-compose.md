---
layout: page
title: Инсталляция Docker Compose на CoreOS
permalink: /linux/virtual/coreos/installation/docker-compose/
---


### Инсталляция Docker Compose на CoreOS

    $ sudo su -

    # mkdir -p /opt/bin


    # curl -L `curl -s https://api.github.com/repos/docker/compose/releases/latest | jq -r '.assets[].browser_download_url | select(contains("Linux") and contains("x86_64"))'` > /opt/bin/docker-compose

    # chmod +x /opt/bin/docker-compose


Result:

    $ docker-compose --version
    docker-compose version 1.7.0, build 0d7bf73
