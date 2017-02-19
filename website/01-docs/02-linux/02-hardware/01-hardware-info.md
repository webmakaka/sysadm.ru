---
layout: page
title: Команды для получения информации по оборудованию в linux
permalink: /linux/hardware/info/
---

# Команды для получения информации по оборудованию в linux

<br/>

### Архитектура

    $ dpkg --print-architecture
    amd64

<br/>

### Процессор

    # lscpu
    Architecture:          x86_64
    CPU op-mode(s):        32-bit, 64-bit
    Byte Order:            Little Endian
    CPU(s):                4
    On-line CPU(s) list:   0-3
    Thread(s) per core:    1
    Core(s) per socket:    4
    CPU socket(s):         1
    NUMA node(s):          1
    Vendor ID:             GenuineIntel
    CPU family:            6
    Model:                 23
    Stepping:              6
    CPU MHz:               2493.739
    BogoMIPS:              4987.88
    Virtualization:        VT-x
    L1d cache:             32K
    L1i cache:             32K
    L2 cache:              6144K
    NUMA node0 CPU(s):     0-3

<br/>

Более подробно:

    # cat /proc/cpuinfo

<br/>

    # cat /proc/cpuinfo | grep model
    model		: 23
    model name	: Intel(R) Xeon(R) CPU           E5420  @ 2.50GHz
    model		: 23
    model name	: Intel(R) Xeon(R) CPU           E5420  @ 2.50GHz
    model		: 23
    model name	: Intel(R) Xeon(R) CPU           E5420  @ 2.50GHz
    model		: 23
    model name	: Intel(R) Xeon(R) CPU           E5420  @ 2.50GHz


<br/>

### Версия BIOS

    # dmidecode -s bios-version
    1402   


<br/>

### Остальное:


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


<br/>

### Данные по жестким дискам

    # hdparm -I  /dev/sdb

// Данные по storage

    # lshw -class disk -class storage


<br/>

### Видеокарта

    /usr/bin/lspci | grep VGA


<br/>

### Данные по всему

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
