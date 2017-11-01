---
layout: page
title: Cisco Router 1941 Basics Commands
permalink: /devices/cisco/routers/1941/basics-commands/
---

# Cisco Router 1941 Basics Commands

    show ip route

<br/>

    ip route 0.0.0.0 0.0.0.0 192.168.1.1
    no ip route 0.0.0.0 0.0.0.0 192.168.1.1

<br/>

    route add -net 192.168.2.0 netmask 255.255.255.0 gw 192.168.1.100
    route del -net 192.168.2.0 netmask 255.255.255.0 gw 192.168.1.100


<pre>

show ip route summary
IP routing table name is default (0x0)
IP routing table maximum-paths is 32
Route Source    Networks    Subnets     Replicates  Overhead    Memory (bytes)
connected       0           4           0           240         704
static          1           0           0           60          176
internal        2                                               1040
Total           3           4           0           300         1920

</pre>


<pre>

show ip route 0.0.0.0  
Routing entry for 0.0.0.0/0, supernet
  Known via "static", distance 1, metric 0, candidate default path
  Routing Descriptor Blocks:
  * 192.168.1.1
      Route metric is 0, traffic share count is 1


</pre>


    ip route 0.0.0.0 0.0.0.0 192.168.1.1

<pre>

cisco-router-1941#show control-plane host open-ports
Active internet connections (servers and established)
Prot               Local Address             Foreign Address                  Service    State
 tcp                        *:22                         *:0               SSH-Server   LISTEN
 tcp                        *:23                         *:0                   Telnet   LISTEN
 tcp                        *:22           192.168.2.5:59686               SSH-Server ESTABLIS
 udp                      *:1975                         *:0                      IPC   LISTEN

</pre>

    # show access-lists 1
