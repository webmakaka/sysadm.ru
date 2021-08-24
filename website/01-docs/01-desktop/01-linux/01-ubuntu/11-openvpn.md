---
layout: page
title: OpenVPN в Ubuntu
description: OpenVPN в Ubuntu
keywords: desktop, linux, ubuntu, vpn, openvpn
permalink: /desktop/linux/ubuntu/vpn/openvpn/
---

# OpenVPN

https://openvpn.net/community-downloads/

<br/>

### Способ 1

    $ sudo apt install openvpn -y

<br/>

### Способ 2

    $ cd ~/tmp/
    $ wget https://swupdate.openvpn.org/community/releases/openvpn-2.5.3.tar.gz

    $ tar -zxvf openvpn-2.5.3.tar.gz
    $ cd openvpn-2.5.3/

Хз, что дальше.

Наверное, configure, make, make install

<br/>

### Подключаемся

<br/>

Запускаю от root. От обычного ошибка при подключении.

<br/>

    $ sudo su -

<br/>

    # openvpn \
        --auth-nocache \
        --config /etc/openvpn/config.ovpn
