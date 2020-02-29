---
layout: page
title: Инсталляция Google Chrome в Ubuntu
permalink: /linux/ubuntu/browsers/chrome/
---

# Инсталляция Chrome в Ubuntu

Данный способ работает в: 12.04, 14.04, 16.04, 18.04

    $ wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
    $ sudo sh -c 'echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list'
    $ sudo apt-get update -y
    $ sudo apt-get install -y google-chrome-stable

<br/>

### Дополнительные Плагины

* Quick JavaScript Switcher
* EditThisCookie
* SetupVPN
* Доступ рутрекер
* LanguageTool
* Nimbus Screenshoot & Screen Video Recorder

<!-- 

Data Saver
LanguageToolPlus.com

-->

<br/>

### Отключение рекламы, кнопок соц.сетей, отслеживания

https://adblockplus.org/

В настройках:

    + ruAdList+EasyList
    - Allow some non-intrusive advertising

<br/>

**Advanced --> MyFilterList:**

    *jivosite.com*
    ||supervisor.ext-twitch.tv/supervisor/v1/index.html

<br/>

### Отключить всплывающие уведомления

Edit --> Settings --> Advanced --> Site Settings --> Notifications --> Disable
