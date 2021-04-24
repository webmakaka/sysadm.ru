---
layout: page
title: Hackintosh
description: Hackintosh
keywords: desktop, dackintosh
permalink: /desktop/hackintosh/setup/
---

# Создание загрузочного USB инсталлера Hackintosh (Big Sur) в Ubuntu Linux 20.04.

<br/>

**Делаю:**  
23.04.2021

<br/>

**Дистриб: Hackintosh Big Sur~ 13 GB**

```
magnet:?xt=urn:btih:5ee8b32c1d6cfc12cc1f55fc67b704a8a9f3092a
```

<br/>

    $ sudo apt-get install dmg2img

<br/>

Переименованный скачанный образ в Hackintosh.dmg

<br/>

    $ dmg2img -v -i /home/marley/Downloads/Hackintosh.dmg -o /home/marley/Downloads/Hackintosh.iso

<br/>

Подрубаю флешку

<br/>

    $ sudo fdisk -l

<br/>

Подмонтировалась как sdd.

**Важно не перепуать имя диска, иначе все данные с него будут потеряны без возможности востановления.**

<br/>

    $ sudo fdisk -l /dev/sdd

<br/>

    $ sudo dd if=/home/marley/Downloads/Hackintosh.iso of=/dev/sdd bs=1M

<br/>

**Торрент файл и инструкция взяты здесь:**

https://www.hackintoshshop.com/2810/hackintosh-big-sur-guide/

<br/>

**Ничего у меня не заработало, т.к. железо старое, мотивация слабая, а руки кривые**

<br/>

Полезный сайт, чтобы понять по железу и д.р.

https://dortania.github.io/OpenCore-Install-Guide/macos-limits.html
