---
layout: page
title: MongoDB
description: MongoDB
keywords: linux, MongoDB
permalink: /databases/mongodb/
---

# MongoDB

**MongoDB Client:**  

Compass:  
https://www.mongodb.com/download-center/compass

RoboMongo


<br/>

**Запуск в контейнере:**

    $ docker run -d -p 27017-27019:27017-27019 --name mongodb \
        -e MONGO_INITDB_ROOT_USERNAME=mongoadmin \
        -e MONGO_INITDB_ROOT_PASSWORD=secret \
        mongo

<br/>

### [MongoDB инсталляция в Ubuntu 18.04.1](/databases/mongodb/install/ubuntu/)

### [Инсталляция MongoDB tools в Ubuntu 18.04](/databases/mongodb/install/ubuntu/tools/)

### [MongoDB инсталляция в Centos 6.X](/databases/mongodb/install/centos/)

### [Обучающие видео по работе с базой данных MongoDB](https://www.youtube.com/watch?v=LBthwZDRR-c&list=PL34sAs7_26wPvZJqUJhjyNtm7UedWR8Ps)

### [How to deploy mongodb replica set with authentification](/databases/deploy-mongodb-replica-set-with-authentification/)
