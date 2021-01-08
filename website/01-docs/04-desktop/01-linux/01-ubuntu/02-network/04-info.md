---
layout: page
title: Информация по сетевому адаптеру
description: Информация по сетевому адаптеру
keywords: Информация по сетевому адаптеру
permalink: /linux/ubuntu/network/info/
---

# Информация по сетевому адаптеру

    # cat /sys/class/net/enp7s1/speed
    1000

<br/>

    # mii-tool enp7s1
    enp7s1: negotiated 1000baseT-FD flow-control, link ok

<br/>

    # ethtool enp7s1
    Settings for enp7s1:
        Supported ports: [ TP MII ]
        Supported link modes:   10baseT/Half 10baseT/Full
                                100baseT/Half 100baseT/Full
                                1000baseT/Half 1000baseT/Full
        Supported pause frame use: No
        Supports auto-negotiation: Yes
        Supported FEC modes: Not reported
        Advertised link modes:  10baseT/Half 10baseT/Full
                                100baseT/Half 100baseT/Full
                                1000baseT/Half 1000baseT/Full
        Advertised pause frame use: Symmetric Receive-only
        Advertised auto-negotiation: Yes
        Advertised FEC modes: Not reported
        Link partner advertised link modes:  10baseT/Half 10baseT/Full
                                            100baseT/Half 100baseT/Full
                                            1000baseT/Full
        Link partner advertised pause frame use: No
        Link partner advertised auto-negotiation: Yes
        Link partner advertised FEC modes: Not reported
        Speed: 1000Mb/s
        Duplex: Full
        Port: MII
        PHYAD: 0
        Transceiver: internal
        Auto-negotiation: on
        Supports Wake-on: pumbg
        Wake-on: g
        Current message level: 0x00000033 (51)
                    drv probe ifdown ifup
        Link detected: yes

<br/>

    # lshw -C network
    *-network
        description: Ethernet interface
        product: RTL-8110SC/8169SC Gigabit Ethernet
        vendor: Realtek Semiconductor Co., Ltd.
        physical id: 1
        bus info: pci@0000:07:01.0
        logical name: enp7s1
        version: 10
        serial: bc:ae:c5:30:13:a5
        size: 1Gbit/s
        capacity: 1Gbit/s
        width: 32 bits
        clock: 66MHz
        capabilities: pm vpd bus_master cap_list rom ethernet physical tp mii 10bt 10bt-fd 100bt 100bt-fd 1000bt 1000bt-fd autonegotiation
        configuration: autonegotiation=on broadcast=yes driver=r8169 driverversion=2.3LK-NAPI duplex=full ip=192.168.1.9 latency=64 link=yes maxlatency=64 mingnt=32 multicast=yes port=MII speed=1Gbit/s
        resources: irq:19 ioport:e000(size=256) memory:fbe20000-fbe200ff memory:fbe00000-fbe1ffff
    *-network
        description: Ethernet interface
        physical id: 1
        logical name: docker0
        serial: 02:42:aa:e0:3f:71
        capabilities: ethernet physical
        configuration: broadcast=yes driver=bridge driverversion=2.3 firmware=N/A ip=172.17.0.1 link=no multicast=yes
