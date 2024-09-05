---
layout: page
title: Домашний интернет. Cisco. Пропал интернет
description: Домашний интернет. Cisco. Пропал интернет
keywords: devices, cisco, routers, 1941, dhcp, unassigned IP-Address
permalink: /device/network/cisco/router/1941/error/unassigned-ip-address/
---

<br/>

# Домашний интернет. Cisco. Пропал интернет

<br/>

# Что делаем в таких случаях

<br/>

[Подключение к cisco в ubuntu 22.04](/device/network/cisco/connect/)

<br/>

```
// Подключиться к роутеру
$ ssh cisco-router-1941
```

<br/>

```
cisco-router-1941> en
```

<br/>

```
# show ip interface brief
Interface                  IP-Address      OK? Method Status                Protocol
Embedded-Service-Engine0/0 unassigned      YES NVRAM  administratively down down
GigabitEthernet0/0         unassigned      YES DHCP   up                    up
GigabitEthernet0/1         192.168.1.1     YES NVRAM  up                    up
NVI0                       unassigned      YES unset  administratively down down
Virtual-PPP1               unassigned      YES NVRAM  administratively down down
```

<br/>

**DHCP билайновский не хочет отдавать мне мой IP.**

<br/>

```
# show dhcp lease
Temp IP addr: 0.0.0.0  for peer on Interface: GigabitEthernet0/0
Temp  sub net mask: 0.0.0.0
    DHCP Lease server: 0.0.0.0, state: 3 Selecting
    DHCP transaction id: 78
    Lease: 0 secs,  Renewal: 0 secs,  Rebind: 0 secs
    Next timer fires after: 00:00:01
    Retry count: 1   Client-ID: cisco-a493.4cba.00a0-Gi0/0
    Client-ID hex dump: 636973636F2D613439332E346362612E
                        303061302D4769302F30
    Hostname: cisco-router-1941
```

<br/>

**Буду интерфейсы перестартовывать**

<br/>

```
# conf t

# interface GigabitEthernet0/0
# shutdown

# no shutdown
```

<br/>

**Разумеется, не помогло!**

<br/>

### М.б. что полезное можно посмотреть

```
$ debug ip dhcp server events
$ debug ip dhcp server packet

$ undebug ip dhcp server events
$ undebug ip dhcp server packet
```
