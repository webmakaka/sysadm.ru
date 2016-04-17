---
layout: page
title: Информацию по имеющемуся оборудованию
permalink: /linux/basics/hardware-info/
---


    $ dpkg --print-architecture
    amd64


<br/>



    # dmidecode -t memory


    bios
    system
    baseboard
    chassis
    processor
    memory
    cache
    connector
    slot


<br/>

### Сетевые интерфейсы:

    # yum install -y lshw
    # lshw -class network



// Данные по жестким дискам

    # hdparm -I  /dev/sdb

// Данные по storage

    # lshw -class disk -class storage

// Видеокарта

    /usr/bin/lspci | grep VGA


// Данные по всему

    # lshw

// Видео

    #  lshw -c video

<br/>

### Узнать температуру процессора, материнской платы


    # apt-get install -y lm-sensors
    # sensors

    atk0110-acpi-0
    Adapter: ACPI interface
    Vcore Voltage:       +0.95 V  (min =  +0.80 V, max =  +1.60 V)
    +3.3V Voltage:       +3.23 V  (min =  +2.97 V, max =  +3.63 V)
    +5V Voltage:         +5.00 V  (min =  +4.50 V, max =  +5.50 V)
    +12V Voltage:       +12.03 V  (min = +10.20 V, max = +13.80 V)
    CPU Fan Speed:      1834 RPM  (min =  600 RPM)
    Chassis1 Fan Speed: 1068 RPM  (min =  600 RPM)
    Chassis2 Fan Speed:    0 RPM  (min =  600 RPM)
    NB Fan Speed:          0 RPM  (min =  600 RPM)
    Power Fan Speed:       0 RPM  (min =    0 RPM)
    CPU Temperature:     +47.0°C  (high = +60.0°C, crit = +95.0°C)
    MB Temperature:      +45.0°C  (high = +45.0°C, crit = +75.0°C)
    NB Temperature:      +59.5°C  (high = +60.0°C, crit = +95.0°C)

Дополнительно:
http://help.ubuntu.ru/wiki/lm_sensors



<br/>


### Данные об операционной системе:

    $ cat /proc/version
    Linux version 3.0.0-32-server (buildd@allspice) (gcc version 4.6.1 (Ubuntu/Linaro 4.6.1-9ubuntu3) ) #51-Ubuntu SMP Thu Mar 21 16:09:49 UTC 2013


<br/>

    $ lsb_release -a
    No LSB modules are available.
    Distributor ID:	Ubuntu
    Description:	Ubuntu 11.10
    Release:	11.10
    Codename:	oneiric


<br/>

    $ lspci -k


<br/>

    $ uname -a
    Linux appserv 3.0.0-32-server #51-Ubuntu SMP Thu Mar 21 16:09:49 UTC 2013 x86_64 x86_64 x86_64 GNU/Linux


<br/>

Ubuntu  
Какие репозитории подключены

    grep -v '^#\|^$' /etc/apt/sources.list{,.d/*.list}


http://repogen.simplylinux.ch/
