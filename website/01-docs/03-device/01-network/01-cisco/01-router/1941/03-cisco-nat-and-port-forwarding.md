---
layout: page
title: Cisco Router 1941 настройка NAT (PAT) и проброс портов во внутреннюю сеть
description: Cisco Router 1941 настройка NAT (PAT) и проброс портов во внутреннюю сеть
keywords: Cisco Router 1941 настройка NAT (PAT) и проброс портов во внутреннюю сеть
permalink: /device/network/cisco/router/1941/cisco-nat-and-port-forwarding/
---

# Cisco Router 1941 настройка NAT (PAT) и проброс портов во внутреннюю сеть

<pre>

<strong>Nat (PAT)</strong>

cisco-router-1941>en
cisco-router-1941# show ip interface brief
Interface                  IP-Address      OK? Method Status                Protocol
Embedded-Service-Engine0/0 unassigned      YES NVRAM  administratively down down
GigabitEthernet0/0         192.168.1.100   YES NVRAM  up                    up
GigabitEthernet0/1         192.168.2.100   YES NVRAM  up                    up


GigabitEthernet0/0 - внешняя сеть
GigabitEthernet0/1 - внутренняя сеть


cisco-router-1941# conf t
cisco-router-1941# access-list 1 permit 192.168.2.0 0.0.0.255


cisco-router-1941(config)# interface GigabitEthernet0/1
cisco-router-1941(config-if)# ip nat inside


cisco-router-1941(config)# interface GigabitEthernet0/0
cisco-router-1941(config-if)# ip nat outside


cisco-router-1941(config-if)# ip nat inside source list 1 interface GigabitEthernet0/0 overload

cisco-router-1941(config)# ip route 0.0.0.0 0.0.0.0 192.168.1.1


=========================================

<strong>Проброс порта в локальную сеть.</strong>
Для примера, пусть это будет вебсервер, который работает на хосте 192.168.2.20 и слушает порт 80.


cisco-router-1941> en
cisco-router-1941# conf t

-- Все запросы от внешних клиентов на IP адрес 192.168.1.100 с портом 80 должны перенаправляться на IP адрес 192.168.2.20 с портом 80
cisco-router-1941(config)# ip nat inside source static tcp 192.168.2.20 80 192.168.1.100 80 extendable


</pre>
