---
layout: page
title: Cisco Router 1941 подключение по mini usb и первичная настройка в консоли Ubuntu 12.04 x64
permalink: /devices/cisco/routers/1941/connetc-to-mini-usb/
---



Подключил Cisco с помощью голубого консольного кабеля, что шел в комплекте с устройством. (К интерфейсам RJ-45 ничего не подключал.) к компьютеру.

<br/>
<br/>

<div align="center">
	<img src="https://raw.githubusercontent.com/sysadm-ru/sysadm-ru.github.io/master/website/docs/devices/routers/cisco/1941/cicso-mini-usb-console-cable.jpg" border="0" alt="cicso mini usb console cable">
</div>

<br/>
<br/>



ВНИМАНИЕ !!!

Перед тем как работать с новыми Cisco надо помнить, что изначально они идут со стандартным паролем для первого входа:

Логин:cisco  
Пароль:cisco  

Если после первого входа, вы не укажете новый логин и пароль командой username User privilage 15 password Pass, либо не удалите команду login в настройках консоли,  то произойдет маленькая неприятность - стандартного пароля уже не будет, нового вы не создали, а маршрутизатор спрашивает пароль, даже не зная с чем его сравнивать.

Перед началом работы, я рекомендую предварительно узнать как и что сделать, чтобы избежать вожможных дополнительных проблем.
Впрочем, у меня самого возникли проблемы такого рода и мне без особых проблем удалось их решить.
После авторизации, мою сессию разорвал роутер по таймауту, и войти под старым паролем мне не удавалось. Так происходили мои первые знакомства с оборудованием от компании cisco.

(Если не ошибаюсь, я перезагружал роутер и вводит в консоли какие-то команды, которые легко гуглятся на хабре, вроде сброс пароля на циске. После этого прошло значительное время, поэтому совсем не помню, что за команды я вводил.)



	# apt-get install -y screen

<br/>

	# lsusb
	Bus 009 Device 003: ID 05a6:0009 Cisco Systems, Inc.

<br/>

	# ls /dev/*ACM0*
	/dev/ttyACM0

<br/>

	# screen /dev/ttyACM0 9600


<br/>

Подключаюсь под учетной записью root, под учетной записью обычного пользователя получаю сообщение об ошибке:
Sorry, could not find a PTY.


Иногда в консоли появляется всякий мусор, приходится откючать и включать usb кабель заново.


Не удавалось подключиться с помощью telnet к маршрутизатору.


Проблема решилась сбросом всех настроек.


<br/>

Для этого выполнил команды:

	cisco-router-1941>enable

<br/>

	cisco-router#erase startup-config
	Erasing the nvram filesystem will remove all configuration files! Continue? [confirm]y[OK]
	Erase of nvram: complete
	<strong>cisco-router#reload</strong>
	Proceed with reload? [confirm]y

	******

	Cisco CISCO1941/K9 (revision 1.0) with 491520K/32768K bytes of memory.
	Processor board ID FGL162612RB
	2 Gigabit Ethernet interfaces
	1 terminal line
	DRAM configuration is 64 bits wide with parity disabled.
	255K bytes of non-volatile configuration memory.
	250880K bytes of ATA System CompactFlash 0 (Read/Write)

	%Error opening tftp://192.168.1.1/network-confg (Timed out)

	         --- System Configuration Dialog ---

	Would you like to enter the initial configuration dialog? [yes/no]: no
	Router>


<br/>

### Собственно настройки


	Router> enable
	Router# configure terminal
	Router(config)# no logging console

<br/>

	Router(config)# hostname cisco-router-1941
	cisco-router-1941(config)# ip domain-name marley.local

<br/>

	cisco-router-1941(config)# ip default-gateway 192.168.1.1
	cisco-router-1941(config)# ip name-server 192.168.1.1
	cisco-router-1941(config)# ip domain lookup

<!--
int loopback 0
cisco-router-1941(config-if)# ip address 192.168.1.100 255.255.255.0

-->

<br/>

	cisco-router-1941(config)# interface GigabitEthernet0/0
	cisco-router-1941(config-if)# ip address 192.168.1.100 255.255.255.0
	cisco-router-1941(config-if)# no shutdown


<br/>

	cisco-router-1941(config-if)# interface GigabitEthernet0/1
	cisco-router-1941(config-if)# ip address 192.168.2.100 255.255.255.0
	cisco-router-1941(config-if)# no shutdown

<br/>

	cisco-router-1941(config-if)# end

<br/>

	cisco-router-1941# show ip interface brief
	Interface                  IP-Address      OK? Method Status                Protocol
	Embedded-Service-Engine0/0 unassigned      YES NVRAM  administratively down down
	GigabitEthernet0/0         192.168.1.100   YES NVRAM  up                    up
	GigabitEthernet0/1         192.168.2.100   YES NVRAM  up                    up


<br/>

### Настройка подключения по telnet

	cisco-router-1941# conf t

<br/>

	cisco-router-1941(config)#service password-encryption

<br/>

	cisco-router(config)# line vty 0 4
	cisco-router-1941(config-line)# password your_password
	cisco-router-1941(config-line)# enable secret your_password

<br/>

	// чтобы работало ctrl + c
	cisco-router-1941(config-line)# escape-character 3

<!--
Router(config)# line con 0
Router(config-line)# escape-character 3
-->

<br/>

	cisco-router(config-line)# login
	cisco-router(config)# end



### Проверка возможности подключиться по telnet к маршрутизатору Cisco 1941 в консоли Ubuntu:

	$ telnet 192.168.1.100
	Trying 192.168.1.100...
	Connected to 192.168.1.100.
	Escape character is '^]'.


	User Access Verification

	Password:
	cisco-router-1941>enable
	Password:
	cisco-router-1941#



Сохраняю конфиг:

	cisco-router-1941# copy running-config startup-config
	Destination filename [startup-config]? startup-config
	Building configuration...
	[OK]
	cisco-router-1941#



<br/>


http://www.cisco1900router.com/full-overview-of-cisco-1941-k9-router-cisco-1941-sec-k9-router.html  
http://www.cisco.com/en/US/docs/routers/access/1900/hardware/installation/guide/19pwrup.html  
http://www.networkworld.com/community/node/22066
