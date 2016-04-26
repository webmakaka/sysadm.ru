---
layout: page
title: Запустить docker контейнер и выполнить команду npm start
permalink: /windows/virtual/docker/run-container-v2/
---


    $ docker run -p 8080:3000 -v $(pwd):/var/www -w "/var/www" node npm start

    -w - working directory (startup directory)



Еще бы так просто сделать npm install предварительно.
