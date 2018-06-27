---
layout: page
title: Docker Lamp Server
permalink: /linux/servers/containers/docker/lamp/
---

# Docker Lamp Server

<div align="center">

    <iframe width="853" height="480" src="https://www.youtube.com/embed/zcCEA0aG3aU" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

</div>

<br/><br/>

Коннектиться нужно к localhost.<br/>
Пароли для подключения к базе в конфигах. <br/>
База, если что назвается "db".<br/>

<br/>

    $ git clone https://github.com/tkyk/docker-compose-lamp
    $ cd docker-compose-lamp
    $ docker-compose build
    $ docker-compose up -d
    $ docker-compose stop
    $ docker-compose rm

<br/>

Пол часа потерять, чтобы потом за 5 минут запускать LAMP!

<br/>

<div align="center">


	<img src="http://img.ipev.ru/2018/06/27/Screenshot-from-2018-06-27-053342.png" border="0" alt="lamp server inside docker">


</div>
