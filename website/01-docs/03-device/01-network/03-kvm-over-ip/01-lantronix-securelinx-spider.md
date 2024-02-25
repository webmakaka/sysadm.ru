---
layout: page
title: KVM over IP - Lantronix SecureLinx Spider
description: KVM over IP - Lantronix SecureLinx Spider
keywords: devices, KVM over IP, Lantronix SecureLinx Spider
permalink: /device/network/kvm-over-ip/lantronix-securelinx-spider/
---

# [KVM over IP] Lantronix SecureLinx Spider

<br/><br/>

**Upd. Не рекомендую сейчас покупать. Всякие проблемы с java.** Хотя и карты наши уже не действуют. Лично я уже лет 5 как к нему не подходил.

<br/>

Вообще админы работают на серверах удаленно, зад находится в теплом и привычном для работы кресле. Встречаются задачи, которые требуют присутствия в холодной серверной комнате. Есть устройства, которые позволяют подключиться к серверу удаленно для задач связанных с настройкой биоса, переустановкой операционной системы. Ниже представлен пример такого рода устройств.

Ранее мне приходилось работать только с Dlink kvm over ip. Очень удобно, да и в тех условиях где я работал, никаких других вариантов (подключить монитор или вытащить сервер - не вариант) не было. С тех пор себе домой я хотел купить такого рода устройство, но цены на них очень высокие.

Вообщем посмотрев обзоры, решил купить себе устройство от Lantronix на амазоне за 315$ с учетом доставки.

<br/>

### Lantronix SecureLinx Spider and SpiderDuo

<br/><br/>

<div align="center">
	<img src="/img/device/network/kvm-over-ip/lantronix-securelinx/Lantronix-SecureLinx-Spider.jpg" border="0" alt="Lantronix SecureLinx Spider">
</div>

<br/>

<ul>
	<li><a href="/files/devices/networks/kvm-over-ip/lantronix-securelinx/Lantronix_SecureLinx_Spider.pdf">Lantronix SecureLinx Spider - KVM over IP</a></li>
	<li><a href="/files/devices/networks/kvm-over-ip/lantronix-securelinx/SpiderDuo_QS.pdf">Lantronix SecureLinx SpiderDuo Quick Start Guide</a><br/></li>
	<li><a href="/files/devices/networks/kvm-over-ip/lantronix-securelinx/Spiders_UG.pdf">Lantronix SecureLinx Spider and SpiderDuo User Guide</a></li>
</ul>

<br/><br/>

<div align="center">

<img src="/img/device/network/kvm-over-ip/lantronix-securelinx/Lantronix_SecureLinx_Spider_pic1.png" border="0" alt="Lantronix SecureLinx Spider Window">

<br/><br/>

<img src="/img/device/network/kvm-over-ip/lantronix-securelinx/Lantronix_SecureLinx_Spider_pic2.png" border="0" alt="Lantronix SecureLinx Spider Window">

<br/><br/>

<img src="/img/device/network/kvm-over-ip/lantronix-securelinx/Lantronix_SecureLinx_Spider_pic3.png" border="0" alt="Lantronix SecureLinx Spider Window">

<br/><br/>

<img src="/img/device/network/kvm-over-ip/lantronix-securelinx/Lantronix_SecureLinx_Spider_pic4.png" border="0" alt="Lantronix SecureLinx Spider Window">

<br/><br/>

<img src="/img/device/network/kvm-over-ip/lantronix-securelinx/Lantronix_SecureLinx_Spider_pic5.png" border="0" alt="Lantronix SecureLinx Spider Window">

</div>

<br/><br/>

В комплект поставки включен кабель с помощью которого можно подключиться к консольному порту устройства, для того, чтобы назначить устройству параметры подключения по сети (в документации, прилагаемой к устройству, описан именно такой способ). Вы также можете это сделать, если установите ПО с компакт диска, которое поможет найти устройство и задать ему параметры подключения по сети (именно так я и сделал).

<br/><br/>

### Для запуска под Linux в firefox мне пришлось обновить поддержку java апплетов в браузере

Если не запускается Java Applet в браузере Firefox в Ubuntu

<br/>

**В ubuntu 14**

```
В ubuntu 14 решил проблему следующим образом:

$ sudo apt-get install icedtea-plugin

Возможно, что этого будет достаточно.

Если нет, можно попробовать:


$ cd /usr/bin
$ ./ControlPanel

Security --> Security Level --> Medium

Добавляем в исключения ip адрес, назначенный lantronix.
```

<br/>

**В ubuntu 12**

Ранее на ubuntu 12 проблемы решались следующим образом:

```

$ sudo apt-get install icedtea-plugin

Если не поможет:
$ sudo mkdir -p /usr/lib/mozilla/plugins
$ cd /usr/lib/mozilla/plugins

$ ls /usr/lib/jvm/java-6-openjdk-amd64/jre/lib/amd64/IcedTeaPlugin.so

$ sudo ln -s /usr/lib/jvm/java-6-openjdk-amd64/jre/lib/amd64/IcedTeaPlugin.so
```

<br/>

См также:

```
http://www.java.com/ru/download/help/enable_browser_ubuntu.xml
http://www.youtube.com/watch?v=1Q_ivIjn5ww
```
