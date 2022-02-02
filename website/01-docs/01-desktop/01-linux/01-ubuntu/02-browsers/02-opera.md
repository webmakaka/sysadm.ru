---
layout: page
title: Инсталляция Opera в Ubuntu
description: Инсталляция Opera в Ubuntu
keywords: linux, ubuntu, opera, browser, инсталляция
permalink: /desktop/linux/ubuntu/browsers/opera/
---

# Инсталляция Opera в Ubuntu

Делаю:  
22.07.2021

<br/>

Бесплатный VPN из коробки перестал работать. 

<br/>

```
$ wget -qO- https://deb.opera.com/archive.key | sudo apt-key add -

$ sudo add-apt-repository "deb [arch=i386,amd64] https://deb.opera.com/opera-stable/ stable non-free"

$ sudo apt install -y opera-stable
```

<br/>

https://addons.opera.com/ru/extensions/details/opera-vpn/

<br/>

Settings --> Dark theme
