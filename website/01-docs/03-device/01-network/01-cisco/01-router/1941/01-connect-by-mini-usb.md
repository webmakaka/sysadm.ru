---
layout: page
title: Cisco Router 1941 подключение по mini usb и первичная настройка в консоли Ubuntu 22.04 x64
description: Cisco Router 1941 подключение по mini usb и первичная настройка в консоли Ubuntu 22.04 x64
keywords: Cisco Router 1941 подключение по mini usb и первичная настройка в консоли Ubuntu 22.04 x64
permalink: /device/network/cisco/router/1941/connect-by-mini-usb/
---

# Cisco Router 1941 подключение по mini usb и первичная настройка в консоли Ubuntu 22.04 x64

<br/>

Подключил Cisco с помощью голубого консольного кабеля, что шел в комплекте с устройством к компьютеру. (К интерфейсам RJ-45 ничего не подключал).

<br/>

![cicso mini usb console cable](/img/device/network/cisco/router/1941/connect-by-mini-usb/cicso-mini-usb-console-cable.jpg 'cicso mini usb console cable'){: .center-image }

<br/>

Выполнял команды под учетной записью root, под учетной записью обычного пользователя получал сообщения об ошибке:

```
Sorry, could not find a PTY
```

<br/>

```
# apt-get install -y screen
```

<br/>

```
# lsusb
Bus 009 Device 003: ID 05a6:0009 Cisco Systems, Inc.
```

<br/>

```
# ls /dev/*ACM0*
/dev/ttyACM0
```

<br/>

```
# screen /dev/ttyACM0 9600
```

<br/>

Иногда в консоли появляется всякий мусор, приходилось откючать и включать usb кабель заново.

<br/>

В интернетах пишут, что лучше сразу задать администратора.

```
Router> conf t
Router> username admin privilege 15 password cisco12345
```

<br/>

### Настройки

<br/>

```
Router> enable
Router# configure terminal
Router(config)# no logging console
```

<br/>

```
Router(config)# hostname cisco-router-1941
cisco-router-1941(config)# ip domain-name marley.local
```

<br/>

```
cisco-router-1941(config)# ip default-gateway 192.168.1.1
cisco-router-1941(config)# ip name-server 192.168.1.1
cisco-router-1941(config)# ip domain lookup
```

<!--
int loopback 0
cisco-router-1941(config-if)# ip address 192.168.1.100 255.255.255.0
-->

<br/>

```
cisco-router-1941(config)# interface GigabitEthernet0/0
cisco-router-1941(config-if)# ip address 192.168.1.100 255.255.255.0
cisco-router-1941(config-if)# no shutdown
```

<br/>

```
cisco-router-1941(config-if)# interface GigabitEthernet0/1
cisco-router-1941(config-if)# ip address 192.168.2.100 255.255.255.0
cisco-router-1941(config-if)# no shutdown
```

<br/>

```
cisco-router-1941(config-if)# end
```

<br/>

```
cisco-router-1941# show ip interface brief
Interface                  IP-Address      OK? Method Status                Protocol
Embedded-Service-Engine0/0 unassigned      YES NVRAM  administratively down down
GigabitEthernet0/0         192.168.1.100   YES NVRAM  up                    up
GigabitEthernet0/1         192.168.2.100   YES NVRAM  up                    up
```
