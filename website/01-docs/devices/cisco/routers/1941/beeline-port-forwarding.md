---
layout: page
title: Cisco Router 1941 Проброс порта в локальную сеть Билайн
permalink: /devices/cisco/routers/1941/beeline-port-forwarding/
---


<strong>Проброс порта в локальную сеть.</strong>
Для примера, пусть это будет вебсервер, который работает на хосте 192.168.1.201 и слушает порт 80.


    cisco-router-1941> en
    cisco-router-1941# conf t

-- Все запросы от внешних клиентов на IP адрес 95.31.31.8 с портом 80 должны перенаправляться на IP адрес 192.168.1.201 с портом 80

<!--

    cisco-router-1941(config)# ip nat inside source static tcp 95.31.31.8 80 192.168.1.201 80 extendable

-->

    cisco-router-1941(config)# ip nat inside source static tcp 192.168.1.201 80 95.31.31.8 80 extendable



http://www.cisco.com/c/en/us/support/docs/long-reach-ethernet-lre-digital-subscriber-line-xdsl/asymmetric-digital-subscriber-line-adsl/12905-827spat.html




<!--

http://subnets.ru/forum/viewtopic.php?f=14&t=394

interface Loopback0
ip address 95.31.31.8 255.255.255.255
ip nat outside
ip virtual-reassembly

-->
