---
layout: page
title: Запуск PgAdmin4 WebClient
description: Запуск PgAdmin4 WebClient
keywords: databases, postgresql, linux, pgadmin, docker
permalink: /server/linux/database/postgresql/pgadmin/
---

# Запуск PgAdmin4 WebClient

<br/>

## Запуск PgAdmin4 WebClient в docker контейнере

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

<br/>

## Запуск PgAdmin4 в окружении python в astra linux 1.7 (На базе debian "buster")

<br/>

Делаю:  
2024.06.27

<br/>

https://www.pgadmin.org/download/pgadmin-4-python/

<br/>

```
$ sudo apt install -y make gcc
$ sudo apt-get install build-essential libssl-dev libffi-dev python3-dev
```

<br/>

```
python -m pip install --upgrade pip
pip install --upgrade setuptools

pip install cryptography
pip install wheel
```

<br/>

```
$ sudo mkdir /var/lib/pgadmin
$ sudo mkdir /var/log/pgadmin
$ sudo chown $USER /var/lib/pgadmin
$ sudo chown $USER /var/log/pgadmin
$ python3 -m venv pgadmin4
$ source pgadmin4/bin/activate
```

<br/>

```
$ pip install pgadmin4
$ pgadmin4
```

<br/>

http://127.0.0.1:5050
