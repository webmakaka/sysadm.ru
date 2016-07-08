---
layout: page
title: Cisco Router 1941 backup конфигов на Linux с помощью scp
permalink: /devices/cisco/routers/1941/backup-configs-to-linux-by-scp/
---


### Cisco Router 1941 backup конфигов на Linux с помощью scp

cisco-router-1941#conf t
cisco-router-1941(config)#ip scp server enable
cisco-router-1941(config)#exit


<br/>


### Backup Startup Config by SCP


    dir nvram:/
    Directory of nvram:/

      249  -rw-        1743                    <no date>  startup-config
      250  ----        3578                    <no date>  private-config
      251  -rw-        1743                    <no date>  underlying-config
        1  -rw-        2945                    <no date>  cwmp_inventory
        4  ----           0                    <no date>  rf_cold_starts
        5  ----         413                    <no date>  persistent-data
        6  -rw-         559                    <no date>  IOS-Self-Sig#1.cer
        7  -rw-          17                    <no date>  ecfm_ieee_mib


<br/>



    cisco-router-1941# copy nvram:/startup-config scp://marley@192.168.1.5://home/marley/Documents/startup-config

    Address or name of remote host [192.168.1.5]?
    Destination username [marley]?
    Destination filename [/home/marley/Documents/startup-config]?
    Writing /home/marley/Documents/startup-config
    Password:
    ! Sink: C0644 1743 startup-config

    1743 bytes copied in 6.672 secs (261 bytes/sec)


На вопросы отвечать Enter а не Y


### Backup Running Config by SCP

    cisco-router-1941#dir system:/
    Directory of system:/

        2  -r--           0                    <no date>  default-running-config
        4  dr-x           0                    <no date>  memory
        1  -rw-        3264                    <no date>  running-config
        3  dr-x           0                    <no date>  vfiles


        cisco-router-1941# copy system:/running-config scp://marley@192.168.1.5://home/marley/Documents/running-config
