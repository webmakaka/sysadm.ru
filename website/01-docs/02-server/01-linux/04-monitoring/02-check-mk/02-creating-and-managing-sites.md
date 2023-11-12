---
layout: page
title: Creating and Managing check_mk sites
description: Creating and Managing check_mk sites
keywords: adm, monitoring, check-mk, creating-and-managing-sites
permalink: /server/linux/monitoring/check-mk/creating-and-managing-sites/
---

# Creating and Managing check_mk sites

Делаю  
12.05.2019

По материалам видео индуса:  
https://www.youtube.com/watch?v=VpjEZ_w4Nrw&list=PL34sAs7_26wODGPEgaHrhRiXkax0q7_v5&index=2

<br/>

Предполагается что уже выполнены шаги по инсталляции <a href="/server/linux/monitoring/check-mk/setup/">check_mk</a>.

<br/>

### Ubuntu

    $ sudo omd create production
    $ sudo omd sites
    $ sudo omd omd start
    $ sudo omd status production

http://192.168.0.11/production

    username: cmkadmin
    password: см. консоли при выполнении команды sudo omd create production

<br/>

Далее какие-то скучные рутинные действия по копированию, удалению и т.д.
