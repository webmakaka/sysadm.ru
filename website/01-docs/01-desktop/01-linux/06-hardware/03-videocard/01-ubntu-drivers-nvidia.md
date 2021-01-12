---
layout: page
title: Установить в ubuntu nvidia драйвера вместо opensource
description: Установить в ubuntu nvidia драйвера вместо opensource
keywords: ubuntu, nvidia, drivers
permalink: /desktop/linux/hardware/videocard/ubuntu/drivers/nvidia/
---

# Установить в ubuntu nvidia драйвера вместо opensource

Проблема такая. Опенсорсные драйвера работают хорошо. Но нестабильно. GUI может перестать отвечать на любые действия пользователей.

Если поставить закрытые драйвера от NVidia, то возможны случаи, когда после перезагрузки черный экран без каких-либо сообщений и ошибок, которые правда позволяет войти в консоль.

UPD. Оказалось, что NVidia больше не поддерживает работу для драйверов для моей видеокарты для новых версий ядер linux и единственный вариант (если не откатывать версию ubuntu) - использовать оперсорсные драйвера. Которые у меня вешают GUI.

<br/>

Наверное, имееет смысл зайти на сайт и посмотреть последние версии драйверов там:

https://www.nvidia.com/en-us/geforce/drivers/

Не все версии драйверов подходят.

<br/>

Можно также попробовать:

    $ software-properties-gtk

Additional Drivers

Выбрать проприетарные драйвера и перезагрузиться.

<br/>

**Или в консоли:**

    $ sudo add-apt-repository ppa:xorg-edgers/ppa -y
    $ sudo apt-get update

    $ apt search nvidia | grep driver

    // $ sudo apt install nvidia-340
    $ sudo apt install -y nvidia-driver-340

<br/>

### Удалить установленные драйвера Nvidia

Ченый экран GUI не стартует. В общем проблемы с драйверами от Nvidia. Пока не удалил, GUI не стартовали.

<br/>

    $ sudo dpkg -P $(dpkg -l | grep nvidia-driver | awk '{print $2}')

    $ sudo apt autoremove

    $ sudo apt install xserver-xorg-video-nouveau

    $ sudo reboot

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

    $ ubuntu-drivers devices
    == /sys/devices/pci0000:00/0000:00:07.0/0000:04:00.0 ==
    modalias : pci:v000010DEd00000612sv00001043sd000082A6bc03sc00i00
    vendor   : NVIDIA Corporation
    model    : G92 [GeForce 9800 GTX / 9800 GTX+]
    driver   : nvidia-340 - distro non-free recommended
    driver   : xserver-xorg-video-nouveau - distro free builtin

<br/>

    // ХЗ что делает.
    $ sudo ubuntu-drivers install

<br/>

    $ nvidia-smi

    Command 'nvidia-smi' not found, but can be installed with:

    sudo apt install nvidia-utils-440         # version 440.100-0ubuntu0.20.04.1, or
    sudo apt install nvidia-340               # version 340.108-0ubuntu2
    sudo apt install nvidia-utils-435         # version 435.21-0ubuntu7
    sudo apt install nvidia-utils-390         # version 390.141-0ubuntu0.20.04.1
    sudo apt install nvidia-utils-450         # version 450.102.04-0ubuntu0.20.04.1
    sudo apt install nvidia-utils-450-server  # version 450.80.02-0ubuntu0.20.04.3
    sudo apt install nvidia-utils-460         # version 460.32.03-0ubuntu0.20.04.1
    sudo apt install nvidia-utils-418-server  # version 418.152.00-0ubuntu0.20.04.1
    sudo apt install nvidia-utils-440-server  # version 440.95.01-0ubuntu0.20.04.1

<br/><br/><br/>

**Links:**  
http://www.binarytides.com/install-nvidia-drivers-ubuntu-14-04/
