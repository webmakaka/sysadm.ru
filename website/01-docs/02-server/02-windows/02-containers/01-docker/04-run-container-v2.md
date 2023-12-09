---
layout: page
title: Запустить docker контейнер и выполнить команду npm start
description: Запустить docker контейнер и выполнить команду npm start
keywords: Запустить docker контейнер и выполнить команду npm start
permalink: /server/windows/containers/docker/run-container-v2/
---

# Запустить docker контейнер и выполнить команду npm start

    $ docker run -p 8080:3000 -v $(pwd):/var/www -w "/var/www" node npm start

    -w - working directory (startup directory)
