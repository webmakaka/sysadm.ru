---
layout: page
title: How to deploy mongodb replica set with authentification
permalink: /linux/servers/databases/deploy-mongodb-replica-set-with-authentification/
---

# How to deploy mongodb replica set with authentification

Ok, guys. Here is a manual how to deploy mongodb replica set with authentification

```
Step 1. Create a keyfile
openssl rand -base64 756 > keyfile

Then upload keyfile to all nodes

chmod 400 keyfile
sudo chown 999 keyfile

Step 2. Docker-compose files
version: '3.4'

services:
  mongodb:
    hostname: 'mongo0'
    container_name: 'y_mongo0'
    image: mongo:4.2
    restart: always
    environment:
      - MONGO_DATA_DIR=/data/db
      - MONGO_LOG_DIR=/dev/null
    volumes:
      - ./data/db:/data/db
      - type: bind
        source: ./keyfile
        target: /usr/src/keyfile
        read_only: true
    ports:
      - 27017:27017
    command:
      - mongod
      - "--quiet"
      - "--bind_ip_all"
      - "--replSet"
      - "rs0"
      - "--keyFile"
      - "/usr/src/keyfile"
    networks:
      - y_cluster

networks:
  y_cluster:

Same docker-compose file for second node but
    hostname: 'mongo1'
    container_name: 'y_mongo1'

Start all nodes.

Step 3. Replica Set Iniate
rs.initiate({
 _id: "rs0",
 members: [
  {_id: 0, host: "10.XXX.XXX.XX1:27017"},
  {_id: 1, host: "10.XXX.XXX.XX2:27017"}
 ]
})

Step 4. Create root user.
Connect to primary node and create root user. You can create only one user in this session, so it should be root first, and in a next session you will create others

use admin
db.createUser(
  {
    user: "mongo-root",
    pwd: "password",
    roles: [ { role: "root", db: "admin" } ]
  }
)

Step 5. Restart nodes
docker-compose down
docker-compose up -d

Step 6. Create another users
use admin
db.createUser(
  {
    user: "mongo-admin",
    pwd: "password",
    roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
  }
)

use mydb
db.createUser(
  {
    user: "mongo-user",
    pwd: "password",
    roles: [ { role: "readWrite", db: "mydb" } ]
  }
)

That's it )
```

https://t.me/justmeandopensourcegroup/517
