---
layout: page
title: Создание загрузочной USB флешки в Linux
description: Создание загрузочной USB флешки в Linux
keywords: Создание загрузочной USB флешки в Linux
permalink: /desktop/linux/linux-live-usb-flash/
---

# Создание загрузочной USB флешки в Linux

Как оказалось, в убунту записать образ Centos с помощью "Startup Disk Creator" нельзя. Не помню, вроде раньше было можно?

Какие-то другие программы для создания загрузочной USB флешки, не понравились. Более того, при инсталляции, они задавали дополнительные тупые вопросы и инсталляция завершалась.

В общем самый простой, изящный и удобный способ записать такую флешку, выполняется всего одной командой!

Только нужно убедиться, в правильности раздела на который нужно записывать, иначе можете все нахер уничтожить.

<br/>

    # ls /dev/sd*
    # fdisk -l /dev/sdf

<br/>

**Главное не перепутать диск!**

    # dd if=./CentOS-7-x86_64-DVD-1810.iso of=/dev/sdf

**С дебианом тоже работает**

    # dd if=./debian-8.6.0-amd64-DVD-1.iso of=/dev/sdg

**С ubuntu тоже работает**

    # dd if=./ubuntu-14.04.5-desktop-amd64.iso of=/dev/sdf

<br/>

### Создать загрузочную флешку с Windows 10 в Linux

Делаю:  
25.04.2021

<br/>

    $ sudo apt install gparted
    $ gparted

Выбираю флешку > Делаю unmount

Device > Create partition table > GPT

UnAllocated > New

ext4 меняю на fat32

Галочка применить

<br/>

Скопировать содержимое диска с виндовс на флешку

<br/>

https://www.linuxbabe.com/ubuntu/easily-create-windows-10-bootable-usb-ubuntu

Там же описывается, как можно и без USB установить
