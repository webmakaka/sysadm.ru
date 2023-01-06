---
layout: page
title: Установить в ubuntu nvidia драйвера вместо opensource
description: Установить в ubuntu nvidia драйвера вместо opensource
keywords: desktop, linux, hardware, videocards, nvidia, ubuntu, drivers
permalink: /desktop/linux/hardware/videocards/nvidia/ubuntu/drivers/
---

# Установить в ubuntu nvidia драйвера вместо opensource


<br/>


Моя карточка 1650.

<br/>

Делаю:  
06.01.2023

<br/>

```
== /sys/devices/pci0000:00/0000:00:01.0/0000:01:00.0 ==
modalias : pci:v000010DEd00001F82sv000010DEsd00001F82bc03sc00i00
vendor   : NVIDIA Corporation
model    : TU117 [GeForce GTX 1650]
driver   : nvidia-driver-418-server - distro non-free
driver   : nvidia-driver-525-server - distro non-free
driver   : nvidia-driver-525 - distro non-free
driver   : nvidia-driver-450-server - distro non-free
driver   : nvidia-driver-470 - distro non-free
driver   : nvidia-driver-515-open - distro non-free
driver   : nvidia-driver-525-open - distro non-free recommended
driver   : nvidia-driver-470-server - distro non-free
driver   : nvidia-driver-515 - distro non-free
driver   : nvidia-driver-510 - distro non-free
driver   : nvidia-driver-515-server - distro non-free
driver   : xserver-xorg-video-nouveau - distro free builtin
``

<br/>


Не включались мониторы. Удалось выйти в консоль. Починилось установив следующую версию`. Предварительно удалив имеющиеся драйвера.


```
$ sudo dpkg -l | grep nvidia-driver | awk '{print $2}'
nvidia-driver-515

$ sudo dpkg -P $(dpkg -l | grep nvidia-driver | awk '{print $2}')

$ sudo apt install -y nvidia-driver-515
```



<br/>

### Остальное, наверное уже неактуально.

<br/>

Можно также попробовать:

    $ software-properties-gtk

<br/>

> Additional Drivers

<br/>

Выбрать проприетарные драйвера и перезагрузиться.

<br/>

**Или в консоли:**

<br/>

### Установить драйвера Nvidia

    // $ sudo add-apt-repository ppa:xorg-edgers/ppa -y

    $ sudo add-apt-repository ppa:graphics-drivers/ppa

    $ sudo apt-get update

    $ sudo apt purge ~nnvidia
    $ sudo apt autoremove
    $ sudo apt clean

    $ apt search nvidia | grep nvidia-driver

    $ sudo apt install nvidia-driver-510


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
