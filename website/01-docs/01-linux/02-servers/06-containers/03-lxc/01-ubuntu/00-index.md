---
layout: page
title: Ubuntu Linux Containers (lxc)
permalink: /linux/servers/containers/lxc/ubuntu/
---

# Ubuntu: Linux Containers (lxc)

<br/>

<div align="center">
    <iframe width="853" height="480" src="https://www.youtube.com/embed/CWmkSj_B-wo" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
</div>

<br/>

### Инсталляция LXC

    # apt install -y lxc lxd
    # systemctl enable lxd && systemctl start lxd

    # systemctl status lxd

    # getent group lxd
    lxd:x:130:marley

    # gpasswd -a marley lxd
    Adding user marley to group lxd

    # logout

<br/>

### Конфигурирование LXC

    // По умолчанию везде, кроме storage backend
    $ lxd init
    Would you like to use LXD clustering? (yes/no) [default=no]:
    Do you want to configure a new storage pool? (yes/no) [default=yes]:
    Name of the new storage pool [default=default]:

<br/>

    Name of the storage backend to use (btrfs, dir, lvm) [default=btrfs]: [dir]

<br/>

    Would you like to connect to a MAAS server? (yes/no) [default=no]:
    Would you like to create a new local network bridge? (yes/no) [default=yes]:
    What should the new bridge be called? [default=lxdbr0]:
    What IPv4 address should be used? (CIDR subnet notation, “auto” or “none”) [default=auto]:
    What IPv6 address should be used? (CIDR subnet notation, “auto” or “none”) [default=auto]:
    Would you like LXD to be available over the network? (yes/no) [default=no]:
    Would you like stale cached images to be updated automatically? (yes/no) [default=yes]
    Would you like a YAML "lxd init" preseed to be printed? (yes/no) [default=no]:

<br/>

    $ lxc version
    To start your first container, try: lxc launch ubuntu:18.04

    Client version: 3.0.3
    Server version: 3.0.3

<br/>

### Некоторые команды для работы с lxc

    lxc storage list

    lxc remote list

    lxc image list

    lxc image list images:
    lxc image list images:centos

    lxc launch ubuntu:16:04 myubuntu

    lxc list
    lxc stop myubuntu

    lxc exec myubuntu bash

    lxc delete myubuntu --force

    lxc info myubuntu
    lxc config show myubuntu

    lxc profile list

    lxc profile copy default custom
    lxc launch ubuntu:16:04 myubuntu --profile custom

<br/>

    lxc exec myvm bash

    lxc profile show default

    lxc config set myubuntu limits.memory 512MB
    lxc config set myubuntu limits.cpu 2

    lxc profile edit custom

    lxc launch ubuntu:16.04 myubuntu --profile custom

<br/>

    echo hello there > myfile
    lxc file push myfile myubuntu/root/

    lxc file pull first/etc/hosts .

<br/>

### [Ubuntu: Linux Containers (lxc) (Наверное устарело по большей части)](/linux/servers/containers/lxc/ubuntu/archive/)
