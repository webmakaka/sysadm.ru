---
layout: page
title: Cisco Router 1941 Проброс порта в локальную сеть Билайн
description: Cisco Router 1941 Проброс порта в локальную сеть Билайн
keywords: Cisco Router 1941, Проброс порта в локальную сеть Билайн
permalink: /devices/cisco/routers/1941/beeline-port-forwarding/
---

# Cisco Router 1941 Проброс порта в локальную сеть Билайн

<strong>Проброс порта в локальную сеть.</strong>
Для примера, пусть это будет вебсервер, который работает на хосте 192.168.1.101 и слушает порт 80.

```
// Коннект к cisco по ssh
$ ssh -c aes256-cbc 192.168.1.1
```

<br/>

```
cisco-router-1941> en
cisco-router-1941# conf t
```

-- Все запросы от внешних клиентов на IP адрес 95.31.31.8 с портом 80 должны перенаправляться на IP адрес 192.168.1.101 с портом 80

    cisco-router-1941(config)# ip nat inside source static tcp 192.168.1.101 80 95.31.31.8 80 extendable

<!--

    // Тоже самое для https
    cisco-router-1941(config)# ip nat inside source static tcp 192.168.1.101 443 95.31.31.8 443 extendable

-->

<!-- no ip nat inside source static tcp     192.168.1.101 80 95.31.31.8 80
    no ip nat inside source static tcp 192.168.1.101 443 95.31.31.8 443 -->

    // Посмотреть маршруты
    # sho run | in nat

http://www.cisco.com/c/en/us/support/docs/long-reach-ethernet-lre-digital-subscriber-line-xdsl/asymmetric-digital-subscriber-line-adsl/12905-827spat.html

<!--

http://subnets.ru/forum/viewtopic.php?f=14&t=394

interface Loopback0
ip address 95.31.31.8 255.255.255.255
ip nat outside
ip virtual-reassembly

-->

<!--
en
conf t
interface loopback 1
ip address 95.31.31.8 255.255.255.0
#exit
-->
