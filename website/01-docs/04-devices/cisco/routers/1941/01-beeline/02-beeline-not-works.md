---
layout: page
title: Cisco Router 1941 если отвалился интернет в локальной сети Билайн
permalink: /devices/cisco/routers/1941/beeline-not-works/
---


# Cisco Router 1941 если отвалился интернет в локальной сети Билайн


    cisco-router-1941> en


<br/>

    # show ip interface brief
    Interface                  IP-Address      OK? Method Status                Protocol
    Embedded-Service-Engine0/0 unassigned      YES NVRAM  administratively down down    
    GigabitEthernet0/0         unassigned      YES DHCP   up                    up      
    GigabitEthernet0/1         192.168.1.1     YES NVRAM  up                    up      
    NVI0                       unassigned      YES unset  administratively down down    
    Virtual-PPP1               unassigned      YES NVRAM  administratively down down    


<br/>

    DHCP билайновский не хочет отдавать мне IP.


<br/>

    #show dhcp lease
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


<br/>

    Мдя, буду интерфейсы перестартовывать.

<br/>

    # conf t

    # interface GigabitEthernet0/0
    # shutdown

    # no shutdown

<br/>

Не помогло. Проблема скорее всего на их стороне. Можно еще кабель подергать. Хотя он и мигает зеленым цветом.

<br/>

Подергал бочку. Мдя. Помогло. <br/>
Опять я на@#$% на Билайн. <br/>
2 : 2