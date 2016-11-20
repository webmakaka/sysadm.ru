---
layout: page
title: Простенький Dockerfile для nodejs
permalink: /linux/containers/docker/dockerfile/nodejs/nodejs-simple/
---

Dockerfile

    FROM node:latest

    MAINTAINER Dan Wahlin

    ENV NODE_ENV=development
    ENV PORT=3000

    COPY      . /var/www
    WORKDIR   /var/www

    RUN       npm install

    EXPOSE $PORT

    ENTRYPOINT ["npm", "start"]


<br/>

    $ docker build -f Dockerfile -t marley/node .

<br/>

    $ docker run -d -p 8080:3000 marley/node
