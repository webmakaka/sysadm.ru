---
layout: page
title: Запуск PgAdmin4 WebClient в docker контейнере
description: Запуск PgAdmin4 WebClient в docker контейнере
keywords: databases, postgresql, linux, pgadmin, docker
permalink: /databases/postgresql/pgadmin/
---

# Запуск PgAdmin4 WebClient в docker контейнере

<br/>

Делаю:  
30.01.2021

<br/>

Нужно установить нормальный логин и пароль!

```
$ docker run -e PGADMIN_DEFAULT_EMAIL='username' -e PGADMIN_DEFAULT_PASSWORD='password' -p 5555:80 --name pgadmin dpage/pgadmin4
```

<br/>

http://localhost:5555/

<br/>

В последний раз подключался к базе, указа ip адрес хост машины с контейнером. (Не локалхост)
