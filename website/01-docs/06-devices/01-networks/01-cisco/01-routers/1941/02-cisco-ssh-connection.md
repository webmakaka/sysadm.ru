---
layout: page
title: Cisco Router 1941 настройка SSH
description: Cisco Router 1941 настройка SSH
keywords: Cisco Router 1941 настройка SSH
permalink: /devices/cisco/routers/1941/cisco-ssh-connection/
---

# Cisco Router 1941 настройка SSH

```
cisco-router-1941>en
cisco-router-1941#
cisco-router-1941#conf t

// Задать если не заданы ранее имя хоста и доменное имя
cisco-router-1941(config)# hostname cisco-router-1941
cisco-router-1941(config)# ip domain-name marley.local


cisco-router-1941(config)# username your_name secret your_pass
cisco-router-1941(config)# crypto key generate rsa modulus 2048

cisco-router-1941(config)#line vty 0 15
cisco-router-1941(config-line)#login local
cisco-router-1941(config-line)#transport input ssh
cisco-router-1941(config-line)#exit
cisco-router-1941(config)#ip ssh version 2


// Проверка SSH
marley@workstation:~$ ssh marley@192.168.2.100
The authenticity of host '192.168.2.100 (192.168.2.100)' can't be established.
RSA key fingerprint is d3:89:67:8c:b9:04:c6:41:4c:87:6b:aa:c1:7f:77:e1.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '192.168.2.100' (RSA) to the list of known hosts.
Password:

cisco-router-1941>en
Password:
cisco-router-1941#


// Проверка Telnet
// Как и ожидалось, подключиться по telnet на наш роутер больше нельзя

marley@workstation:~$ telnet 192.168.2.100
Trying 192.168.2.100...
telnet: Unable to connect to remote host: Connection refused
```
