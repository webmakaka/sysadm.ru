---
layout: page
title: Инсталляция Skype в Ubuntu 22.04
description: Инсталляция Skype в Ubuntu 22.04
keywords: desktop, linux, ubuntu, skype, инсталляция
permalink: /desktop/linux/ubuntu/skype/
---

# Инсталляция Skype в Ubuntu 22.04

<br/>

**Последний раз делаю:**  
2025.02.06

<br/>

### С помощью flatpak (впервые работаю с flatpak)

Вроде установилось и норм работает.

```
$ sudo apt install -y flatpak

$ flatpak remote-add --user --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo


$ flatpak install flathub com.skype.Client

$ flatpak run com.skype.Client
```

<br/>

https://flathub.org/apps/com.skype.Client

<br/>

### С помощью Snap

<br/>

**Последний раз делаю:**  
2025.02.06

На ноуте с процессором AMD, по правой кнопке не появляется меню, при этом подвисает skype.

Версия которая устанавливается, криво работает. За более чем год так и не поправили!
Работающую версию Microsoft запретил использовать.

<br/>

```
// $ sudo snap remove skype
$ sudo snap install skype
```

<br/>

### Deb пакет

**Последний раз делаю:**  
2025.02.06

<br/>

Устанавливается версия 8.109.0.209, которая отказывается стартовать и требует обновиться

<br/>

```
// Прямая ссылка на deb пакет
$ wget https://go.skype.com/skypeforlinux-64.deb
```

<br/>

```
// инсталлировать .deb пакет с skype
$ sudo dpkg -i ./skypeforlinux-64.deb
```

<br/>

```
// Найти ранее установленный skype
$ dpkg --list | grep skype
```

<br/>

```
// Удалить skype
$ sudo dpkg --remove skypeforlinux
```
