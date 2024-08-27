---
layout: page
title: Архив - Cisco Router 1941 настройка работы интернета в Билайн по l2tp
description: Архив - Cisco Router 1941 настройка работы интернета в Билайн по l2tp
keywords: devices, cisco, routers, 1941, beeline l2tp
permalink: /device/network/cisco/router/1941/beeline-l2tp/
---

# [Архив] Cisco Router 1941 настройка работы интернета в Билайн по l2tp

<br/>

<div align="center" style="border-width: 4px; padding: 10px; border-style: inset; border-color: red; ">

Многое поменялось. Теперь не нуно настраивать l2tp. Смотри подробнее <a href="/device/network/cisco/router/1941/beeline/">здесь</a>

</div>

<br/>

**Исправления и замечания приветствуются.**

<br/>
<br/>

<code><strong>L2TP SERVERS (BRAS)</strong></code>

85.21.0.241 - tp.internet.beeline.ru - работает как мне нужно  
85.21.0.254 - работает как мне нужно

85.21.0.109 - tp.corbina.net - как минимум не предоставляет маршруты после авторизации. (руками не пробовал прописывать)  
85.21.0.65 - vpn.corbina.ru - как минимум не предоставляет маршруты после авторизации. (руками не пробовал прописывать)

<br/>

### Базовая настройка роутера:

IOS: c1900-universalk9-mz.SPA.152-4.M2.bin<br/>
Последние версии ios крашились при попытке настроить NAT.
<br/><br/>

GigabitEthernet0/0 - внешняя сеть<br/>
GigabitEthernet0/1 - внутренняя сеть<br/>
<br/>

<pre>
<strong>cisco-router-1941> <code>en</code></strong>
</pre>

<pre>
<strong>cisco-router-1941# <code>conf t</code></strong>
</pre>

-- To enable the Domain Name System (DNS) server on a router

<pre>
<strong>cisco-router-1941(config)# <code>ip dns server</code></strong>
</pre>

<pre>
<strong>cisco-router-1941(config)# <code>ip name-server 85.21.192.3</code></strong>
</pre>

<pre>
<strong>cisco-router-1941(config)# <code>ip name-server 213.234.192.8</code></strong>
</pre>

<pre>
<strong>cisco-router-1941(config)# <code>ip name-server 85.21.192.5</code></strong>
</pre>

<pre>
<strong>cisco-router-1941(config)# <code>ntp server ntp.corbina.net</code></strong>
</pre>

<pre>
<strong>cisco-router-1941(config)# <code>ntp server ru.pool.ntp.org</code></strong>
</pre>

<br/>

-- удаляю прописанные ранее dns сервер и шлюз по умолчанию

<pre>
<strong>cisco-router-1941(config)# <code>no ip name-server 192.168.1.1</code></strong>
</pre>

<pre>
<strong>cisco-router-1941(config)# <code>no ip default-gateway 192.168.1.1</code></strong>
</pre>

<br/>

### Предварительная настройка интерфейсов

-- GigabitEthernet0/0

<pre>
<strong>cisco-router-1941(config)# <code>interface GigabitEthernet0/0</code></strong>
</pre>

<pre>
<strong>cisco-router-1941(config)# <code>ip address dhcp</code></strong>
</pre>

<pre>
<strong>cisco-router-1941(config)# <code>load-interval 30</code></strong>
</pre>

<pre>
<strong>cisco-router-1941(config)# <code>description ISP-External-Local</code></strong>
</pre>

<pre>
<strong>cisco-router-1941(config)# <code>no shutdown</code></strong>
</pre>

<pre>
<strong>cisco-router-1941(config)# <code>exit</code></strong>
</pre>

<br/>

-- GigabitEthernet0/1

<pre>
<strong>cisco-router-1941(config)# <code>interface GigabitEthernet0/1</code></strong>
</pre>

<pre>
<strong>cisco-router-1941(config)# <code>ip address 192.168.1.1 255.255.255.0</code></strong>
</pre>

<pre>
<strong>cisco-router-1941(config)# <code>description Internal-NetWork</code></strong>
</pre>

<pre>
<strong>cisco-router-1941(config)# <code>no shutdown</code></strong>
</pre>

<pre>
<strong>cisco-router-1941(config-if)# <code>exit</code></strong>
</pre>

<pre>
<strong>cisco-router-1941(config)# <code>exit</code></strong>
</pre>

<br/>

-- проверка получения ip адреса от провайдера

<pre>
<strong>cisco-router-1941# <code>show dhcp lease</code></strong>
</pre>

<pre>
Temp IP addr: 10.111.0.8  for peer on Interface: GigabitEthernet0/0
Temp  sub net mask: 255.255.248.0
   DHCP Lease server: 78.107.63.102, state: 5 Bound
   DHCP transaction id: 19EA
   Lease: 973148 secs,  Renewal: 486574 secs,  Rebind: 851504 secs
Temp ip static route0: dest 195.14.50.26 router 10.111.0.1
Temp ip static route1: dest 195.14.50.16 router 10.111.0.1
   Next timer fires after: 5d15h
   Retry count: 0   Client-ID: cisco-a493.4cba.00a0-Gi0/0
   Client-ID hex dump: 636973636F2D613439332E346362612E
                       303061302D4769302F30
   Hostname: cisco-router-1941

</pre>

<pre>
<strong>cisco-router-1941# <code>show ip interface brief</code></strong>
</pre>

<pre class="gray_border">
Interface                  IP-Address      OK? Method Status                Protocol
Embedded-Service-Engine0/0 unassigned      YES NVRAM  administratively down down
GigabitEthernet0/0         10.111.0.8      YES DHCP   up                    up
GigabitEthernet0/1         192.168.1.1     YES manual up                    up  
</pre>

<br/>

### Подготовка роутера к созданию l2tp соединения:

<pre>
<strong>cisco-router-1941# <code>conf t</code></strong>
</pre>

<pre>
<strong>cisco-router-1941(config)# <code>l2tp-class DEFAULT-L2TP-CLASS</code></strong>
</pre>

<pre>
<strong>cisco-router-1941(config-l2tp-class)# <code>end</code></strong>
</pre>

<br/>

<pre>
<strong>cisco-router-1941# <code>conf t</code></strong>
</pre>

<pre>
<strong>cisco-router-1941(config)# <code>pseudowire-class PW-L2TP-GigabitEthernet0/0</code></strong>
</pre>

<pre>
<strong>cisco-router-1941(config-pw-class)# <code>encapsulation l2tpv2</code></strong>
</pre>

<pre>
<strong>cisco-router-1941(config-pw-class)# <code>protocol l2tpv2 DEFAULT-L2TP-CLASS</code></strong>
</pre>

<pre>
<strong>cisco-router-1941(config-pw-class)# <code>ip local interface GigabitEthernet0/0</code></strong>
</pre>

<pre>
<strong>cisco-router-1941(config-pw-class)#  <code>end</code></strong>
</pre>

<br/>

### Создание туннельного интерфейса Virtual-PPP1:

<pre>
<strong>cisco-router-1941# <code>conf t</code></strong>
</pre>

<pre>
<strong>cisco-router-1941(config)# <code>interface Virtual-PPP1</code></strong>
</pre>

<pre>
<strong>cisco-router-1941(config-if)# <code>description ISP-External-Internet</code></strong>
</pre>

<pre>
<strong>cisco-router-1941(config-if)# <code>ip mtu 1460</code></strong>
</pre>

<!--

adjust-mss было 1420

PPS Поменял брас на "новый" 78.107.1.246 (tp.internet.beeline.ru), вернул ip tcp adjust-mss 1420 вроде проблема пропала: т.е. тормоза загрузки некоторых страниц, невозможность браузера открыть URL, те же проблемы но в iTunes на Mac OS X/App Store iOS

-->

<pre>
<strong>cisco-router-1941(config-if)# <code>ip tcp adjust-mss 1420</code></strong>
</pre>

<br/>

-- Данная функция позволяет предотвратить атаки связанные с фрагментацией пакетов, приводящая к переполнению буфера . Когда атакующим посылаются много незаконченных фрагментрированных пакетов и фаерволлу приходится их собирать, тратя на это память и время. В IOS с поддержкой безопасности включена по умолчанию.

<pre>
<strong>cisco-router-1941(config-if)# <code>no ip virtual-reassembly</code></strong>
</pre>

<br/>

-- Если дать команду no peer neighbor-route, то перестанет работать механизм ipcp, т.е. не будут автоматически прописаны маршруты по-умолчанию, нужно будет их прописать самостоятельно. Узнать можно, например, посмотрев на компьютере, подключенному к сети напрямую, получившим эти маршруты. Или где-то на сайте билайна.

<pre>
<strong>cisco-router-1941(config-if)# <code>no peer neighbor-route</code></strong>
</pre>

<pre>
<strong>cisco-router-1941(config-if)# <code>no cdp enable</code></strong>
</pre>

<pre>
<strong>cisco-router-1941(config-if)# <code>ip address negotiated</code></strong>
</pre>

<pre>
<strong>cisco-router-1941(config-if)# <code>no ppp authentication chap callin</code></strong>
</pre>

<br/>

-- В следующих 2-х командах, нужно руками вводить свой логин и пароль для доступа в интернет.

<pre>
<strong>cisco-router-1941(config-if)# <code>ppp chap hostname 'your-login'</code></strong>
</pre>

<pre>
<strong>cisco-router-1941(config-if)# <code>ppp chap password 'your-pass'</code></strong>
</pre>

-- Ну почему нельзя ввести какое-нибудь dns имя вместо IP?

<br/>
-- cisco-router-1941(config-if)# pseudowire &lt;l2tp server&gt; 10 pw-class PW-L2TP-GigabitEthernet0/0

<pre>
<strong>cisco-router-1941(config-if)# <code>pseudowire 85.21.0.241 10 pw-class PW-L2TP-GigabitEthernet0/0</code></strong>
</pre>

<pre>
<strong>cisco-router-1941(config-if-xconn)# <code>end</code></strong>
</pre>

<br/><br/>

-- Virtual-PPP1 присвоен ожидаемый IP адрес.

<pre>
<strong>cisco-router-1941# <code>show ip interface brief</code></strong>
</pre>

<pre class="gray_border">
Interface                  IP-Address      OK? Method Status                Protocol
Embedded-Service-Engine0/0 unassigned      YES NVRAM  administratively down down
GigabitEthernet0/0         10.111.0.8      YES DHCP   up                    up
GigabitEthernet0/1         192.168.1.2     YES NVRAM  up                    up
Virtual-PPP1               95.31.31.8      YES IPCP   up                    up  
</pre>

<br/>

### Проверка работы интернета на маршрутизаторе

-- Проверили локалку

<pre>
<strong>cisco-router-1941# <code> ping beeline.ru</code></strong>
</pre>

<pre class="gray_border">
Translating "beeline.ru"...domain server (85.21.192.3) [OK]

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 217.118.87.98, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/3/4 ms

</pre>

-- Проверили Интернет

<pre>
<strong>cisco-router-1941# <code> ping ya.ru source Virtual-PPP1</code></strong>
</pre>

<pre class="gray_border">
Translating "ya.ru"...domain server (85.21.192.3) [OK]

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 213.180.204.3, timeout is 2 seconds:
Packet sent with a source address of 95.31.31.8
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 4/19/80 ms
</pre>

<br/>

### Настройка NAT

<pre>

cisco-router-1941# conf t
cisco-router-1941(config)# interface GigabitEthernet0/1
cisco-router-1941(config-if)# ip nat inside

cisco-router-1941(config)# interface GigabitEthernet0/0
cisco-router-1941(config-if)# ip nat outside

cisco-router-1941(config)# interface Virtual-PPP1
cisco-router-1941(config-if)# ip nat outside

cisco-router-1941(config-if)#exit

</pre>

<!--
cisco-router-1941# conf t
cisco-router-1941# access-list 1 permit 192.168.1.0 0.0.0.255

cisco-router-1941(config-if)# ip nat inside source list 1 interface Virtual-PPP1 overload

-->

-- Листы доступа

<pre>

cisco-router-1941(config)# ip access-list extended nat
cisco-router-1941(config-ext-nacl)# deny ip any host 85.21.0.241
cisco-router-1941(config-ext-nacl)# permit ip 192.168.1.0 0.0.0.255 any
cisco-router-1941(config-ext-nacl)# exit

cisco-router-1941(config)# route-map public permit 10
cisco-router-1941(config-route-map)# match ip address nat
cisco-router-1941(config-route-map)# match interface Virtual-PPP1
cisco-router-1941(config-route-map)# exit

cisco-router-1941(config)# route-map local permit 10
cisco-router-1941(config-route-map)# match ip address nat
cisco-router-1941(config-route-map)# match interface GigabitEthernet0/0
cisco-router-1941(config-route-map)# exit

cisco-router-1941(config)# ip nat inside source route-map local interface GigabitEthernet0/0
cisco-router-1941(config)# ip nat inside source route-map public interface Virtual-PPP1 overload

</pre>

<!--

cisco-router-1941(config-if)# ip nat inside source list 1 interface Virtual-PPP1 overload


-- вот эту команду, наверное выполнять не нужно.
cisco-router-1941(config-if)# ip nat inside source list 1 interface GigabitEthernet0/0 overload

-->

<br/>

### Обязательные маршруты

<pre>

cisco-router-1941(config)#ip forward-protocol nd
cisco-router-1941(config)#ip route 0.0.0.0 0.0.0.0 Virtual-PPP1
cisco-router-1941(config)#ip route 85.21.0.241 255.255.255.255 dhcp


</pre>

<pre>

cisco-router-1941# show ip route

Gateway of last resort is 10.111.0.1 to network 0.0.0.0

S*    0.0.0.0/0 [254/0] via 10.111.0.1
      10.0.0.0/8 is variably subnetted, 3 subnets, 3 masks
S        10.0.0.0/8 [1/0] via 10.111.0.1
C        10.111.0.0/21 is directly connected, GigabitEthernet0/0
L        10.111.0.8/32 is directly connected, GigabitEthernet0/0
      78.0.0.0/32 is subnetted, 1 subnets
S        78.107.63.102 [254/0] via 10.111.0.1, GigabitEthernet0/0
      85.0.0.0/32 is subnetted, 2 subnets
C        85.21.0.241 is directly connected, Virtual-PPP1
S        85.21.192.3 [1/0] via 10.111.0.1
      89.0.0.0/32 is subnetted, 1 subnets
S        89.179.135.67 [1/0] via 10.111.0.1
      95.0.0.0/32 is subnetted, 1 subnets
C        95.31.31.8 is directly connected, Virtual-PPP1
      192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks
C        192.168.1.0/24 is directly connected, GigabitEthernet0/1
L        192.168.1.2/32 is directly connected, GigabitEthernet0/1
      195.14.50.0/32 is subnetted, 2 subnets
S        195.14.50.16 [254/0] via 10.111.0.1
S        195.14.50.26 [254/0] via 10.111.0.1
      213.234.192.0/32 is subnetted, 1 subnets
S        213.234.192.8 [1/0] via 10.111.0.1


</pre>

<strong>Почитать:</strong><br/><br/>
http://homenet.beeline.ru/index.php?showtopic=206930&st=0<br/>
http://habrahabr.ru/post/136342/<br/>
