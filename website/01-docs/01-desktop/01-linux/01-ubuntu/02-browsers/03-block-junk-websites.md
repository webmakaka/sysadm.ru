---
layout: page
title: Шаги после инсталляции Ubuntu 20.04 LTS (для себя)
description: Шаги после инсталляции Ubuntu 20.04 LTS (для себя)
keywords: ubuntu, install
permalink: /desktop/linux/ubuntu/browsers/block-junk-websites/
---

# Заблокировать дерьмовые сайты с рекламой казино, ставок и т.д.

<br/>

```
$ sudo vi /etc/hosts
```

<br/>

```
81.17.30.22 nnm-club.me

0.0.0.0 blackhole.beeline.ru
0.0.0.0 mailtrack.io
0.0.0.0 metrika.yandex.ru
0.0.0.0 informer.yandex.ru
0.0.0.0 naydex.net
0.0.0.0 *.naydex.net

0.0.0.0 newsru.com
0.0.0.0 rbc.ru
0.0.0.0 lenta.ru


0.0.0.0 jivosite.ru
0.0.0.0 www.jivosite.ru
0.0.0.0 content.mql5.com

0.0.0.0 onlinefreecourse.net
0.0.0.0 downloadtutorials.net
0.0.0.0 ebookie.org

0.0.0.0 betcity.ru
0.0.0.0 winline.ru

0.0.0.0 downloadtutorials.net


```

-

Отсюда добавим кучу сайтов:  
https://github.com/michaeltrimm/hosts-blocking/blob/master/_hosts.txt

<br/>

**Для этого:**

<br/>

    $ sudo su -
    # cd /tmp

    # curl https://raw.githubusercontent.com/michaeltrimm/hosts-blocking/master/_hosts.txt --output badwebsites.txt

    # cat ./badwebsites.txt >> /etc/hosts

<br/>

И отсюда:

    # curl https://gitlab.com/quidsup/ntrk-tracker-radar/-/raw/master/ddg_tracker_radar_confirmed.hosts --output badwebsites.txt

    # cat ./badwebsites.txt >> /etc/hosts
