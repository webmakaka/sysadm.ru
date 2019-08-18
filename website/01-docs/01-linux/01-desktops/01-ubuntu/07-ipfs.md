---
layout: page
title: Знакомство с ipfs
permalink: /linux/desktops/ubuntu/ipfs/
---

# Знакомство с ipfs

Делаю впервые:  
03.01.2019


Делал на примере этого ролика:

https://www.youtube.com/watch?v=I4HunE6Txcg&t=309s



### Создание сервиса:


    # snap install go --classic
    # go get -u github.com/ipfs/ipfs-update

    -- show versions
    # ipfs-update versions
    # /root/go/bin/ipfs-update install latest

    # ipfs init

    # vi /etc/systemd/system/ipfsd.service

```bash

[Unit]
Description=ipfs daemon

[Service]
ExecStart=/usr/local/bin/ipfs daemon
Restart=always
User=root
Group=root

[Install]
WantedBy=multi-user.target

```


    # systemctl enable ipfsd.service
    # systemctl start  ipfsd.service
    # systemctl status ipfsd.service


<br/>

### Загрузка каталога в ipfs


    $ mkdir -p upload
    $ copy some_sheet ./upload


    $ ipfs init

    $ ipfs add -r upload/


    $ ipfs ls QmRwbCSp2mvC9onyt7cyeXzZV1sxK97QegZFAcx2qdCHnC


<br/>

    https://gateway.ipfs.io/ipfs/QmRwbCSp2mvC9onyt7cyeXzZV1sxK97QegZFAcx2qdCHnC


<br/>


Какой-то официальный ролик от создалелей:

    https://www.youtube.com/watch?time_continue=1032&v=8CMxDNuuAiQ