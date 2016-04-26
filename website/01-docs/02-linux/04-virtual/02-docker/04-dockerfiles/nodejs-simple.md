---
layout: page
title: Простенький Dockerfile для nodejs
permalink: /linux/virtual/docker/dockerfile/nodejs/nodejs-simple/
---


    FROM node:latest

    MAINTAINER Dan Wahlin

    ENV NODE_ENV=development
    ENV PORT=3000

    COPY      . /var/www
    WORKDIR   /var/www

    RUN       npm install

    EXPOSE $PORT

    ENTRYPOINT ["npm", "start"]
