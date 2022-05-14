---
layout: page
title: Работа с железками в linux
description: Работа с железками в linux
keywords: linux, hardware
permalink: /desktop/linux/hardware/
---

# Работа с железками в linux

### Информация по имеющемуся оборудованию

[Команды для получения информации по оборудованию в linux](/desktop/linux/hardware/info/)

<br/>

### MotherBoard

[Upgrade BIOS в linux](/desktop/linux/hardware/motherboard/bios-upgrade/)

<br/>

### HDD / SSD и другие блочные устройства

[HDD / SSD и другие блочные устройства в Linux](/desktop/linux/hardware/hdd/)

<br/>

### Информация об используемой памяти

    $ egrep --color 'Mem|Cache|Swap' /proc/meminfo
    MemTotal:        7801672 kB
    MemFree:          137984 kB
    MemAvailable:     230696 kB
    Cached:           797396 kB
    SwapCached:        12904 kB
    SwapTotal:       4718588 kB
    SwapFree:        1245060 kB

<br/>

### VideoCard

[Установить в Ubuntu nvidia драйвера вместо opensource](/desktop/linux/hardware/videocards/nvidia/ubuntu/drivers/)

// Узнать сколько видеокарта использует GPU памяти

    $ watch -n 1 nvidia-smi

<br/>

```
Thu May 11 21:42:29 2017
+------------------------------------------------------+
| NVIDIA-SMI 340.101    Driver Version: 340.101        |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  GeForce 9800 GT...  Off  | 0000:04:00.0     N/A |                  N/A |
| 35%   59C    P0    N/A /  N/A |    321MiB /   511MiB |     N/A      Default |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Compute processes:                                               GPU Memory |
|  GPU       PID  Process name                                     Usage      |
|=============================================================================|
|    0            Not Supported                                               |
+-----------------------------------------------------------------------------+
```

<br/>

### Wi-Fi адаптер

[Мудацкий Realtek опять не работает в Ubuntu](/desktop/linux/hardware/wi-fi/ubuntu/realtek/)
