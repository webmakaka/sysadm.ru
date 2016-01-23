---
layout: page
title: С помощью Docker Compose
permalink: /linux/virtual/docker/linking-containers/docker-compose/
---


С помощью Docker Compose создаем файл YML с инструкциями о том, какие контейнеры запускать и как линковать их между собой.



    # vi docker-compose.yml

<br/>

    django:
      image: username/image:latest
      command: python manage.py supervisor
      environment:
        RUN_ENV: "$RUN_ENV"
      ports:
       - "80:8001"
      volumes:
       - .:/project
      links:
       - redis
       - postgres

    celery_worker:
      image: username/image:latest
      command: python manage.py celery worker -l info
      links:
       - postgres
       - redis

    postgres:
      image: postgres:9.1
      volumes:
        - local_postgres:/var/lib/postgresql/data
      ports:
       - "5432:5432"
      environment:
        POSTGRES_PASSWORD: "$POSTGRES_PASSWORD"
        POSTGRES_USER: "$POSTGRES_USER"  

    redis:
      image: redis:latest
      command: redis-server --appendonly yes


<br/>

    $ docker-compose up

или

    $ docker-compose up --no-deps -d postgres


http://dou.ua/lenta/articles/docker/
