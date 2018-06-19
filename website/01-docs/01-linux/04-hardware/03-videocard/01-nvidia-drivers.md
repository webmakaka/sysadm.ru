---
layout: page
title: Установить в ubuntu nvidia драйвера вместо opensource
permalink: /linux/hardware/videocard/ubuntu/drivers/nvidia/
---


# Установить в ubuntu nvidia драйвера вместо opensource

У меня после обновлений в ubuntu возникли проблемы с опенсорсными драйверами.

После начала работы, через некоторое время, GUI переставал показывать рабочий стол. Только черная консоль в которой постоянно писалось сообщение об ошибке. Ошибка повторялась при перезагрузке.

Пришлось установить драйвера с закрытым исходным кодом.

Ошибка после этого пропала.  
Изображение стало немного мягче.


	$ sudo add-apt-repository ppa:xorg-edgers/ppa -y
	$ sudo apt-get update
	$ sudo apt-get install nvidia-current

<br/>

	$ software-properties-gtk

<br/>

Остается выбрать проприетарные драйвера и перезагрузиться.



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



Обратить внимание на строчку:

	Kernel driver in use: nvidia

<br/>

	$ lsmod | grep nvidia
	nvidia              10744943  53
	drm                   303102  2 nvidia



<br/>

	$ modprobe -R nvidia
	nvidia_331_updates


<br/><br/><br/>

Links:  
http://www.binarytides.com/install-nvidia-drivers-ubuntu-14-04/
