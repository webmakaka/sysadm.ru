---
layout: page
title: Cisco Router 1941 Проброс порта в локальную сеть Билайн
permalink: /devices/routers/cisco/1941/beeline-port-forwarding/
---


<strong>Проброс порта в локальную сеть.</strong>
Для примера, пусть это будет вебсервер, который работает на хосте 192.168.1.201 и слушает порт 80.


    cisco-router-1941> en
    cisco-router-1941# conf t

-- Все запросы от внешних клиентов на IP адрес 192.168.1.100 с портом 80 должны перенаправляться на IP адрес 192.168.2.20 с портом 80

    cisco-router-1941(config)# ip nat inside source static tcp 95.31.31.8 80 192.168.1.201 80 extendable
