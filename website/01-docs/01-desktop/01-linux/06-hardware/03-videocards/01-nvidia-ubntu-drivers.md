---
layout: page
title: Установить в ubuntu nvidia драйвера вместо opensource
description: Установить в ubuntu nvidia драйвера вместо opensource
keywords: desktop, linux, hardware, videocards, nvidia, ubuntu, drivers
permalink: /desktop/linux/hardware/videocards/nvidia/ubuntu/drivers/
---

# Установить в ubuntu nvidia драйвера вместо opensource

<br/>

Делаю:  
2024.06.23

<br/>

Моя карточка nvidia 1650

<br/>

Не включались мониторы. Удалось выйти в консоль. Починилось установкой следующей версии. Предварительно удалив имеющиеся драйвера.

<br/>

```
$ sudo dpkg -l | grep nvidia-driver | awk '{print $2}'
nvidia-driver-535
```

<br/>

```
// Удаление
$ sudo dpkg -P $(dpkg -l | grep nvidia-driver | awk '{print $2}')

$ sudo apt install -y nvidia-driver-510
```

<br/>

### Остальное, наверное уже неактуально.

<br/>

Можно также попробовать:

```
$ software-properties-gtk
```

<br/>

> Additional Drivers

<br/>

Выбрать проприетарные драйвера и перезагрузиться.

<br/>

**Или в консоли:**

<br/>

### Установить драйвера Nvidia

```
// $ sudo add-apt-repository ppa:xorg-edgers/ppa -y

$ sudo add-apt-repository ppa:graphics-drivers/ppa

$ sudo apt-get update

$ sudo apt purge ~nnvidia
$ sudo apt autoremove
$ sudo apt clean

$ apt search nvidia | grep nvidia-driver

$ sudo apt install nvidia-driver-510 -y
```

<!--

<br/>

### Удалить установленные драйвера Nvidia и установить opensource nouveau

Ченый экран GUI не стартует. В общем проблемы с драйверами от Nvidia. Пока не удалил, GUI не стартовали.

<br/>

    $ sudo dpkg -P $(dpkg -l | grep nvidia-driver | awk '{print $2}')

    $ sudo apt autoremove -y

    $ sudo apt install xserver-xorg-video-nouveau

    $ sudo reboot

-->

<br/>

### Получить доп информацию при необходимости

```
$ lspci -vnn | grep -i VGA -A 12
04:00.0 VGA compatible controller [0300]: NVIDIA Corporation GF119 [GeForce GT 520] [10de:1040] (rev a1) (prog-if 00 [VGA controller])
    Flags: bus master, fast devsel, latency 0, IRQ 75
    Memory at fa000000 (32-bit, non-prefetchable) [size=16M]
    Memory at d8000000 (64-bit, prefetchable) [size=128M]
    Memory at d6000000 (64-bit, prefetchable) [size=32M]
    I/O ports at cc00 [size=128]
    [virtual] Expansion ROM at fbc00000 [disabled] [size=512K]
    Capabilities: <access denied>
    Kernel driver in use: nvidia

04:00.1 Audio device [0403]: NVIDIA Corporation GF119 HDMI Audio Controller [10de:0e08] (rev a1)
    Flags: bus master, fast devsel, latency 0, IRQ 37
    Memory at fbcfc000 (32-bit, non-prefetchable) [size=16K]
```

<br/>

**Обратить внимание на строчку:**

    Kernel driver in use: nvidia

<br/>

    $ lsmod | grep nvidia
    nvidia              10744943  53
    drm                   303102  2 nvidia

<br/>

    $ modprobe -R nvidia
    nvidia_331_updates

<br/>

    $ dpkg -l | grep nvidia
    iF  nvidia-340                                 340.108-0ubuntu2                    amd64        NVIDIA binary driver - version 340.108
    ii  nvidia-opencl-icd-340                      340.108-0ubuntu2                    amd64        NVIDIA OpenCL ICD
    ii  nvidia-settings                            440.82-0ubuntu0.20.04.1             amd64        Tool for configuring the NVIDIA graphics driver
    ii  screen-resolution-extra                    0.18build1                          all          Extension for the nvidia-settings control panel

<br/>

    // ХЗ что делает.
    $ sudo ubuntu-drivers install

<br/>

    $ nvidia-smi

<br/>

**Links:**
http://www.binarytides.com/install-nvidia-drivers-ubuntu-14-04/

<br/>

### Тест видеокарты

<br/>

```
$ sudo apt install -y glmark2
$ glmark2
```
