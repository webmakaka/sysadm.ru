---
layout: page
title: Ubuntu, Realtek RTL-8110SC/8169SC - отваливается сетевой интерфейс на ubuntu 14.04.5 lts
permalink: /linux/desktops/ubuntu/network/realtek-r8169-error/
---


#  [Ubuntu, Realtek RTL-8110SC/8169SC] отваливается сетевой интерфейс на ubuntu 14.04.5 lts

Стал отваливаться сетевой интерфейс. Особенно, если включить трансляции с twitch или youtube.


В dmesg сообщения вроде:


    r8169 0000:07:01.0 eth0: rtl_chipcmd_cond == 1 (loop: 100, delay: 100).


В dmesg лучше искать командой:

    # dmesg | grep -e eth -e r8169


При перезагрузке какое-то время сеть работает. Потом опять перестает работать.

Также помогают на какое-то время следующие команды. Эффект как от перезагрузки всего компьютера.

    -- нашел интерфейсы следующим образом
    # find /sys/devices -name eth*

    -- собственно перестартовка
    # echo 1 >/sys/devices/pci0000:00/0000:00:1e.0/0000:07:01.0/remove
    # echo 1 >/sys/devices/pci0000:00/0000:00:1e.0/rescan

<br/>

    # lsmod | grep r8169
    r8169                  86016  0 
    mii                    16384  1 r8169

<br/>

    # modinfo r8169 | grep -i version
    version:        2.3LK-NAPI
    srcversion:     3CDA56D002C01D874AD1DCD
    vermagic:       4.4.0-141-generic SMP mod_unload modversions retpoline 

<br/>

    -- найти драйвер в системе
    # find /lib/modules/$(uname -r) -name r8169.ko
    /lib/modules/4.4.0-141-generic/kernel/drivers/net/ethernet/realtek/r8169.ko


<br/>

    # ethtool -i eth0
    driver: r8169
    version: 2.3LK-NAPI
    firmware-version: 
    bus-info: 0000:07:01.0
    supports-statistics: yes
    supports-test: no
    supports-eeprom-access: no
    supports-register-dump: yes
    supports-priv-flags: no


<br/>

Мне помог только upgrade до новой версии.