---
layout: page
title: Cisco Router 1941 сбросо всех настроек
description: Cisco Router 1941 сбросо всех настроек
keywords: Cisco Router 1941 сбросо всех настроек
permalink: /device/network/cisco/router/1941/erase-startup-config/
---

# Cisco Router 1941 сбросо всех настроек

<br/>

Были проблемы, решилось сбросом всех настроек.

<br/>

Для этого выполнил команды:

```
cisco-router-1941>enable
```

<br/>

```
cisco-router#erase startup-config
Erasing the nvram filesystem will remove all configuration files! Continue? [confirm]y[OK]
Erase of nvram: complete

cisco-router#reload

Proceed with reload? [confirm]y

******

Cisco CISCO1941/K9 (revision 1.0) with 491520K/32768K bytes of memory.
Processor board ID FGL162612RB
2 Gigabit Ethernet interfaces
1 terminal line
DRAM configuration is 64 bits wide with parity disabled.
255K bytes of non-volatile configuration memory.
250880K bytes of ATA System CompactFlash 0 (Read/Write)

%Error opening tftp://192.168.1.1/network-confg (Timed out)

            --- System Configuration Dialog ---

Would you like to enter the initial configuration dialog? [yes/no]: no
Router>
```
