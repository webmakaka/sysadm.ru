---
layout: page
title: Cisco Switch Catalyst-WS-C2960G-8TC-L
permalink: /devices/cisco/switches/catalyst-ws-c2960g-8tc-l/
---


<br/>

# Cisco Switch Catalyst-WS-C2960G-8TC-L



<a href="http://files.sysadm.ru/devices/cisco/switches/catalyst-ws-c2960g-8tc-l/Catalyst_2960_Switch_GSG_8port.pdf">pdf с документацией</a>

<br/>

### Cisco Switch Catalyst-WS-C2960G-8TC-L - первичная настройка


<br/>

<pre>

==========================================
В комплекте с устройством консольного кабеля не было.
Для первичной настройки, необходимо включить устройство, удерживать кнопку mode слева внизу на устройстве пока все индикаторы не станут зеленого цвета, далее необходимо подключиться к маршрутизатору http://10.0.0.1/ и уже там установить ip адрес устройства.

Я когда первый раз выполнял операции, невнимательно прочитал пошаговые инструкции из-за чего потерял много времени.

==========================================
</pre>

**Базовая настройка в командной строке**

    marley@workstation:~$ telnet 192.168.1.21
    Trying 192.168.1.21...
    Connected to 192.168.1.21.
    Escape character is '^]'.

    User Access Verification


<br/>

    Switch>
    Switch>en
    Password:
    Switch#

<br/>

Switch#conf t

**Задать имя устройству**

    Switch(config)#hostname cisco-switch-2960


<br/>

    cisco-switch-2960(config)#end
    cisco-switch-2960#show interfaces status

    Port      Name               Status       Vlan       Duplex  Speed Type
    Gi0/1                        connected    1          a-full  a-100 10/100/1000BaseTX
    Gi0/2                        notconnect   1            auto   auto 10/100/1000BaseTX
    Gi0/3                        notconnect   1            auto   auto 10/100/1000BaseTX
    Gi0/4                        notconnect   1            auto   auto 10/100/1000BaseTX
    Gi0/5                        notconnect   1            auto   auto 10/100/1000BaseTX
    Gi0/6                        notconnect   1            auto   auto 10/100/1000BaseTX
    Gi0/7                        notconnect   1            auto   auto 10/100/1000BaseTX
    Gi0/8                        notconnect   1            auto   auto Not Present


<br/>

    cisco-switch-2960#conf t
    cisco-switch-2960(config)#interface vlan 1
    cisco-switch-2960(config-if)#
    cisco-switch-2960(config-if)#ip address 192.168.1.2 255.255.255.0
    cisco-switch-2960(config-if)#ip default-gateway 192.168.1.1
    cisco-switch-2960(config-if)#end

<br/>

**Пароль на enable**

    cisco-switch-2960#conf t
    cisco-switch-2960(config)#enable secret your_password

    cisco-switch-2960(config)#line vty 0 ?  
      <1-15>  Last Line number
      <cr>


**Пароль на подключение по telnet**

    cisco-switch-2960(config)#line vty 0 15
    cisco-switch-2960(config-line)#password your_password
    cisco-switch-2960(config-line)#login
    cisco-switch-2960(config-line)#end


**Сохраняю конфиг:**

    cisco-switch-2960#copy running-config startup-config
    Destination filename [startup-config]? startup-config
    Building configuration...
    [OK]
    0 bytes copied in 2.852 secs (0 bytes/sec)
