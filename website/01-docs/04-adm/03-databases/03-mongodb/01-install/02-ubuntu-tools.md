---
layout: page
title: Инсталляция MongoDB tools в Ubuntu 18.04
description: Инсталляция MongoDB tools в Ubuntu 18.04
keywords: linux, ubuntu, mongodb, tools, mongoimport, install
permalink: /adm/databases/mongodb/install/ubuntu/tools/
---

# Инсталляция MongoDB tools в Ubuntu 18.04

Делаю!  
08.03.2020

https://docs.mongodb.com/v4.2/tutorial/install-mongodb-on-ubuntu/

<br/>

    $ wget -qO - https://www.mongodb.org/static/pgp/server-4.2.asc | sudo apt-key add -

    $ echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.2.list

    $ sudo apt-get update

    $ sudo apt-get install -y mongodb-org-tools

    $ mongoimport --version
    mongoimport version: r4.2.3
