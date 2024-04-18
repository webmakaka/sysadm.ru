---
layout: page
title: Выбор загружаемого при старте kernel в Ubuntu 20 с помощью GRUB
description: Выбор загружаемого при старте kernel в Ubuntu 20 с помощью GRUB
keywords: desktop, linux, ubuntu, setup, grub
permalink: /desktop/linux/grub/
---

# Выбор загружаемого при старте kernel в Ubuntu 20 с помощью GRUB

<br/>

### С помощью командной строки

<br/>

**Делаю:**  
2024.04.18

<br/>

```
$  sudo grub-mkconfig | grep -iE "menuentry 'Ubuntu, with Linux" | awk '{print i++ " : "$1, $2, $3, $4, $5, $6, $7}'
```

```
***
0 : menuentry 'Ubuntu, with Linux 6.5.0-1019-oem' --class ubuntu
1 : menuentry 'Ubuntu, with Linux 6.5.0-1019-oem (recovery mode)'
2 : menuentry 'Ubuntu, with Linux 6.5.0-27-generic' --class ubuntu
3 : menuentry 'Ubuntu, with Linux 6.5.0-27-generic (recovery mode)'
4 : menuentry 'Ubuntu, with Linux 6.2.0-39-generic' --class ubuntu
5 : menuentry 'Ubuntu, with Linux 6.2.0-39-generic (recovery mode)'
6 : menuentry 'Ubuntu, with Linux 6.1.0-1036-oem' --class ubuntu
7 : menuentry 'Ubuntu, with Linux 6.1.0-1036-oem (recovery mode)'
8 : menuentry 'Ubuntu, with Linux 5.17.0-1035-oem' --class ubuntu
9 : menuentry 'Ubuntu, with Linux 5.17.0-1035-oem (recovery mode)'
```

```
// Текущее
$ uname -srn
Linux workstation 6.5.0-1019-oem
```

<br/>

```
$ grep GRUB_DEFAULT /etc/default/grub
GRUB_DEFAULT=0
```

<br/>

```
$ sudo vi /etc/default/grub
```

<br/>

```
GRUB_DEFAULT="1>2"
```

```
sudo update-grub
```

<br/>

```
$ grep GRUB_DEFAULT /etc/default/grub
GRUB_DEFAULT="1>2"
```

<br/>

```
$ sudo reboot
```

<br/>

```
$ uname -srn
Linux workstation 6.5.0-27-generic
```

<br/>

https://askubuntu.com/questions/82140/how-can-i-boot-with-an-older-kernel-version

<br/>

### С помощью GUI

**Делаю:**  
2024.04.18

Не установилось. Репо не подключилось. Ошибка какая-то.

<br/>

**Делаю:**  
2024.01.25

<br/>

```
$ sudo apt install -y \
    grub-customizer
```

**Выпилено в версии 22.04**

Пишут, что можно установить следующим образом в 22.04

<br/>

https://itsubuntu.com/how-to-install-grub-customizer-on-ubuntu-22-04-lts/

<br/>

```
$ sudo add-apt-repository ppa:trebelnik-stefina/grub-customizer
$ sudo apt update
$ sudo apt install grub-customizer
```

или

```
$ sudo add-apt-repository ppa:danielrichter2007/grub-customizer
$ sudo apt-get update
$ sudo apt-get install -y grub-customizer
```

<br/>

```
$ grub-customizer
```

<br/>

General settings -> default entry -> predefined -> Ubuntu with linux 6.5.0-14-generic
