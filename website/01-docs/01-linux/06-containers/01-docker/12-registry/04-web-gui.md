---
layout: page
title: Docker registry frontend (WEB GUI для registry)
permalink: /linux/containers/docker/registry/web-gui/
---


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


