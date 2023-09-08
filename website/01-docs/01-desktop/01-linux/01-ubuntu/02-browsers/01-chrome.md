---
layout: page
title: Инсталляция Google Chrome в Ubuntu
description: Инсталляция Google Chrome в Ubuntu
keywords: linux, ubuntu, browser, google chrome, инсталляция
permalink: /desktop/linux/ubuntu/browsers/chrome/
---

# Инсталляция Chrome в Ubuntu

Делаю:  
30.06.2023

<br/>

```
$ wget -q -O - https://dl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
$ sudo sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list'
$ sudo apt-get update -y
$ sudo apt-get install -y google-chrome-stable
```

<br/>

### Отключить всплывающие уведомления и всякую слежку

Settings --> Privacy and Security --> Site Settings:

<br/>

```
Location --> Blocked
Camera --> Blocked
Microphone --> Blocked
Notifications --> Blocked
```

<br/>

### Язык по умолчанию

Settings -> Advanced -> Language -> Order Languages based on your preferences: English, Russian

<br/>

### Дополнительные плагины

- EditThisCookie
- SetupVPN
- LanguageTool

<!--

- Nimbus Screenshoot & Screen Video Recorder

-->

- Доступ рутрекер
- SponsorBlock for YouTube

- uBlacklist
  https://chrome.google.com/webstore/detail/ublacklist/pncfbmialoiaghdehhbnbhkkgmjanfhe

<br/>

Правила для uBlacklist

<br/>

```
*://*.coderoad.ru/*
*://*.qastack.ru/*
*://*.question-it.com/*
*://*.gitmemory.com/*
```

<!--
hola vpn
-->

<br/>

### Могут понадобиться, но необязательные

- cookies.txt

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
