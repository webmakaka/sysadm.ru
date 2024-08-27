---
layout: page
title: Ubuntu 18.04 - ssh Cisco - no matching cipher found
description: Ubuntu 18.04 - ssh Cisco - no matching cipher found
keywords: Ubuntu 18.04 - ssh Cisco - no matching cipher found
permalink: /device/network/cisco/no-matching-cipher-found/
---

# Cisco Ошибка "no matching cipher found" при подключении к устройству по ssh в Ubuntu 18.04

<br/>

[Для ubuntu 22.04](/device/network/cisco/routers/1941/unassigned-ip-address/)

<br/>

В ubuntu 18.04 появилась ошибка при попытке подключения по ssh. При этом на клиенте с centos не помню какой версии, подключалось нормально.

<br/>

    Unable to negotiate with 192.168.1.1 port 22: no matching cipher found. Their offer: aes128-cbc,3des-cbc,aes192-cbc,aes256-cbc

<br/>

Подключиться можно используя команду:

    $ ssh -c aes256-cbc 192.168.1.1

<br/>

Или предлагают:

add command "ip ssh client algorithm encryption aes256-cbc" in your router config for working.

Но у меня такое (я не специалист и разбираться сейчас не хочу)

    cisco-router-1941(config)#ip ssh client algorithm encryption aes256-cbc
                                    ^
    % Invalid input detected at '^' marker.

<br/>

### В Ubuntu 20.04.01 теперь такое

    $ ssh -c aes256-cbc 192.168.1.1
    Unable to negotiate with 192.168.1.1 port 22: no matching key exchange method found. Their offer: diffie-hellman-group-exchange-sha1,diffie-hellman-group14-sha1,diffie-hellman-group1-sha1

<br/>

Подключаюсь командой:

```
$ ssh \
    -oKexAlgorithms=+diffie-hellman-group1-sha1 \
    -c aes256-cbc \
    192.168.1.1
```
