---
layout: page
title: Подключение к cisco в ubuntu 22.04
description: Подключение к cisco в ubuntu 22.04
keywords: devices, cisco, connection, ubuntu
permalink: /device/network/cisco/connect/
---

<br/>

# Подключение к cisco в ubuntu 22.04

<br/>

```
// Чтобы подключиться с 22 ubuntu
// marley - пользователь под которым работаю
// конфиг на несколько устройств
$ vi ~/.ssh/config
```

<br/>

```
Host cisco-router-1941
  User marley
  Hostname 192.168.1.1
  HostKeyAlgorithms +ssh-rsa
  KexAlgorithms +diffie-hellman-group1-sha1
  Ciphers +aes256-cbc
Host cisco-switch-2960
  User marley
  Hostname 192.168.1.2
  HostKeyAlgorithms +ssh-rsa
  KexAlgorithms +diffie-hellman-group1-sha1
  Ciphers +aes256-cbc
Host cisco-router-2921
  User marley
  Hostname 192.168.1.4
  HostKeyAlgorithms +ssh-rsa
  KexAlgorithms +diffie-hellman-group1-sha1
  Ciphers +aes256-cbc
Host cisco-switch-sg300
  User marley
  Hostname 192.168.1.5
  HostKeyAlgorithms +ssh-rsa
  KexAlgorithms +diffie-hellman-group1-sha1
  Ciphers +aes256-cbc
```

<br/>

```
// Подключиться к роутеру
$ ssh cisco-router-1941
```
