---
layout: page
title: Cisco Router 1941 обновление IOS
permalink: /devices/cisco/routers/1941/cisco-how-to-update-ios/
---



<pre>

Последняя версия на сайте cisco для меня Release 15.4.1T ED

Description:	UNIVERSAL
Release:	15.4.1T
File Name:	c1900-universalk9-mz.SPA.154-1.T.bin
MD5 Checksum:	1c7c1b1ba2bb4f04cb081f069e8f5523


========
<strong>UPD: с этим ios возникли проблемы!!!</strong>
При попытке выполнить команду ip nat inside / outside роутер перезагружался, на флеше появлялись краш дампы.

Со следующими ios у меня были <a href="/devices/cisco/routers/1941/crash-dump-example/">краши</a>:
c1900-universalk9-mz.SPA.154-1.T.bin
c1900-universalk9-mz.SPA.153-3.M.bin


С этим ios команда выполнилась нормально:
c1900-universalk9-mz.SPA.152-4.M2.bin

===========================


С официально сайта скачать образы могут только те у кого имеется активный контракт. Разумеется, у меня такого нет.

Есть вариант скачать ios с рутрекера:
http://rutracker.org/forum/viewtopic.php?t=2989354

<strong>magnet</strong>

magnet:?xt=urn:btih:1f0225cda3a4885e5be93d3e7cef4f75e43be7b9&dn=ios&tr=http%3A%2F%2Fbt3.rutracker.org%2Fann%3Fuk%3D8x9BZLktSN&tr=http%3A%2F%2Fretracker.local%2Fannounce


===========================

Мне повезло, я нашел в гугле нужную мне версии, просто вбив название файла.

$ md5sum /tmp/c1900-universalk9-mz.SPA.154-1.T.bin
1c7c1b1ba2bb4f04cb081f069e8f5523  /tmp/c1900-universalk9-mz.SPA.154-1.T.bin

Может кому понадобится.
Выкладываю в облаках
https://mega.co.nz/#F!KIlWTC7Y!VMOFqwT6XuJQVq6pNUv3RQ

===========================


cisco-router-1941#sh ver
EASE SOFTWARE (fc1)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2012 by Cisco Systems, Inc.
Compiled Tue 20-Mar-12 17:58 by prod_rel_team

ROM: System Bootstrap, Version 15.0(1r)M15, RELEASE SOFTWARE (fc1)

cisco-router-1941 uptime is 2 hours, 41 minutes
System returned to ROM by reload at 01:28:14 UTC Sun Jan 12 2014
System restarted at 01:29:34 UTC Sun Jan 12 2014
System image file is "flash0:<strong>c1900-universalk9-mz.SPA.151-4.M4.bin</strong>"
Last reload type: Normal Reload
Last reload reason: Reload Command


Cisco CISCO1941/K9 (revision 1.0) with 491520K/32768K bytes of memory.
Processor board ID FGL162612RB
2 Gigabit Ethernet interfaces
1 terminal line
DRAM configuration is 64 bits wide with parity disabled.
255K bytes of non-volatile configuration memory.
250880K bytes of ATA System CompactFlash 0 (Read/Write)




=============


15 M (Extended Maintenance) releases delivered on a more frequent basis (approximately every 16 months), enabling customers to qualify, deploy, and remain on a release longer with active support. Release 15.0(1)M (FCS November, 2009) is the first 15 M release.
15 T (Standard Maintenance) releases (in between 15 M releases) ideal for the very latest features and hardware support before the next 15 M release becomes available. Each 15 T release receives bug-fix rebuild support for 13 months, plus an additional 6 months support for security/vulnerability issues such as Product Security Incident Response Team (PSIRT) advisories. Release 15.1(1)T (FCS March, 2010) is the first 15 T release.


Должно поменяться
c1900-universalk9-mz.SPA.151-4.M4.bin
на
c1900-universalk9-mz.SPA.154-1.T.bin


</pre>


<br/>

### Вариант с использованием USB-FLASH карты

<pre>

Возникли какие-то проблемы с первой флешкой, она не определялась.
Воткнул другую в циску и отформатировал ее.

Некоторые флешки определяются, но не форматируются. Особенно новые. Перед использованием необходимо их отформатировать в операционной системе.

cisco-router-1941#format usbflash0:
Format operation may take a while. Continue? [confirm]y
Format operation will destroy all data in "usbflash0:".  Continue? [confirm]y
Format: All system sectors written. OK...

Format: Total sectors in formatted partition: 15508928
Format: Total bytes in formatted partition: 7940571136
Format: Operation completed successfully.

Format of usbflash0: complete


=====


cisco-router-1941#show file system
File Systems:

       Size(b)       Free(b)      Type  Flags  Prefixes
             -             -    opaque     rw   archive:
             -             -    opaque     rw   system:
             -             -    opaque     rw   tmpsys:
             -             -    opaque     rw   null:
             -             -   network     rw   tftp:
*    256487424     196145152      disk     rw   flash0: flash:#
             -             -      disk     rw   flash1:
        262136        250790     nvram     rw   nvram:
             -             -    opaque     wo   syslog:
             -             -    opaque     rw   xmodem:
             -             -    opaque     rw   ymodem:
             -             -   network     rw   rcp:
             -             -   network     rw   http:
             -             -   network     rw   ftp:
             -             -   network     rw   scp:
             -             -    opaque     ro   tar:
             -             -   network     rw   https:
             -             -    opaque     ro   cns:
    7925055488    7925018624  usbflash     rw   usbflash0:




==============


cisco-router-1941#dir flash:
Directory of flash0:/

    1  -rw-    55088360  Jun 28 2012 23:19:58 +00:00  c1900-universalk9-mz.SPA.151-4.M4.bin
    2  -rw-        2903  Jun 28 2012 23:27:34 +00:00  cpconfig-19xx.cfg
    3  -rw-     3000320  Jun 28 2012 23:27:48 +00:00  cpexpress.tar
    4  -rw-        1038  Jun 28 2012 23:27:58 +00:00  home.shtml
    5  -rw-      122880  Jun 28 2012 23:28:08 +00:00  home.tar
    6  -rw-     1697952  Jun 28 2012 23:28:22 +00:00  securedesktop-ios-3.1.1.45-k9.pkg
    7  -rw-      415956  Jun 28 2012 23:28:36 +00:00  sslclient-win-1.1.4.176.pkg

256487424 bytes total (196145152 bytes free)


==============

cisco-router-1941# copy flash:c1900-universalk9-mz.SPA.151-4.M4.bin usbflash0: [Enter]
cisco-router-1941# copy flash:cpconfig-19xx.cfg usbflash0: [Enter]
cisco-router-1941# copy flash:cpexpress.tar usbflash0: [Enter]
cisco-router-1941# copy flash:home.shtml usbflash0: [Enter]
cisco-router-1941# copy flash:home.tar usbflash0: [Enter]
cisco-router-1941# copy flash:securedesktop-ios-3.1.1.45-k9.pkg usbflash0: [Enter]
cisco-router-1941# copy flash:sslclient-win-1.1.4.176.pkg usbflash0: [Enter]


==============

cisco-router-1941#dir usbflash0:
Directory of usbflash0:/

    2  -rw-    55088360  Jan 13 2014 20:49:08 +00:00  c1900-universalk9-mz.SPA.151-4.M4.bin
    3  -rw-        2903  Jan 13 2014 20:54:24 +00:00  cpconfig-19xx.cfg
    4  -rw-     3000320  Jan 13 2014 20:54:36 +00:00  cpexpress.tar
    5  -rw-        1038  Jan 13 2014 20:54:44 +00:00  home.shtml
    6  -rw-      122880  Jan 13 2014 20:54:54 +00:00  home.tar
    7  -rw-     1697952  Jan 13 2014 20:55:04 +00:00  securedesktop-ios-3.1.1.45-k9.pkg
    8  -rw-      415956  Jan 13 2014 20:55:14 +00:00  sslclient-win-1.1.4.176.pkg

7925055488 bytes total (7864676352 bytes free)


=========================

// Если нужно, что-то удалить с flash
#del flash:/c1900-universalk9-mz.spa.153-3.m.bi
=========================


cisco-router-1941# copy usbflash0:c1900-universalk9-mz.SPA.154-1.T.bin flash: [Enter]

=========================
=========================

// Проверяем md5 скопированного ios
cisco-router-1941#verify /md5 flash0:/c1900-universalk9-mz.SPA.154-1.T.bin 1c7c1b1ba2bb4f04cb081f069e8f5523

MD5 of flash0:/c1900-universalk9-mz.SPA.154-1.T.bin Done!
Verified (flash0:/c1900-universalk9-mz.SPA.154-1.T.bin) = 1c7c1b1ba2bb4f04cb081f069e8f5523


=========================

cisco-router-1941# conf t
cisco-router-1941(config)# boot system flash c1900-universalk9-mz.SPA.154-1.T.bin

cisco-router-1941(config)# end

cisco-router-1941# copy running-config startup-config
Destination filename [startup-config]? startup-config
Warning: Attempting to overwrite an NVRAM configuration previously written
by a different version of the system image.
Overwrite the previous NVRAM configuration?[confirm]
Building configuration...
[OK]


cisco-router-1941#reload
Proceed with reload? [confirm]Connection to 192.168.1.100 closed by remote host.
Connection to 192.168.1.100 closed.


==========================
=========================

cisco-router-1941>en
Password:
cisco-router-1941#sh ver
Cisco IOS Software, C1900 Software <strong>(C1900-UNIVERSALK9-M), Version 15.4(1)T, RELEASE SOFTWARE (fc2)</strong>
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2013 by Cisco Systems, Inc.
Compiled Fri 22-Nov-13 20:40 by prod_rel_team

ROM: System Bootstrap, Version 15.0(1r)M15, RELEASE SOFTWARE (fc1)

cisco-router-1941 uptime is 1 minute
System returned to ROM by reload at 22:21:08 UTC Mon Jan 13 2014
System image file is "flash:<strong>c1900-universalk9-mz.SPA.154-1.T.bin</strong>"
Last reload type: Normal Reload
Last reload reason: Reload Command


</pre>

<br/>

### Вариант с использованием TFTP сервера

<pre>

Буду ставить:

Description:	UNIVERSAL
Release:	15.3.3M
Release Date:	23/Jul/2013
File Name:	c1900-universalk9-mz.SPA.153-3.M.bin
Min Memory:	DRAM 512 MB Flash 256 MB
Size:	64.09 MB (67200372 bytes)
MD5 Checksum:	52f6246201803f0345b97c82cad6ece3


===================================

<strong>Сначала на ubuntu подниму tfpf сервер</strong>

sudo apt-get install xinetd tftpd tftp
sudo vi /etc/xinetd.d/tftp

service tftp
{
protocol = udp
port = 69
socket_type = dgram
wait = yes
user = nobody
server = /usr/sbin/in.tftpd
server_args = /tftpboot
disable = no
}


$ sudo mkdir /tftpboot
$ sudo chmod 777 /tftpboot
$ sudo chown nobody /tftpboot
$ sudo /etc/init.d/xinetd restart


Копирую ios в каталог /tftpboot/

$ ls /tftpboot/c1900-universalk9-mz.SPA.153-3.M.bin
/tftpboot/c1900-universalk9-mz.SPA.153-3.M.bin


Скачиваю файл на Cisco:

cisco-router-1941#copy tftp://192.168.1.5/c1900-universalk9-mz.SPA.153-3.M.bin flash:
Destination filename [c1900-universalk9-mz.SPA.153-3.M.bin]?
Accessing tftp://192.168.1.5/c1900-universalk9-mz.SPA.153-3.M.bin...
Loading c1900-universalk9-mz.SPA.153-3.M.bin from 192.168.1.5 (via GigabitEthernet0/1): !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
[OK - 67200372 bytes]

67200372 bytes copied in 96.740 secs (694649 bytes/sec)

==================


cisco-router-1941#dir flash:
Directory of flash0:/

    1  -rw-    55088360  Jun 28 2012 23:19:58 +00:00  c1900-universalk9-mz.SPA.151-4.M4.bin
    2  -rw-        2903  Jun 28 2012 23:27:34 +00:00  cpconfig-19xx.cfg
    3  -rw-     3000320  Jun 28 2012 23:27:48 +00:00  cpexpress.tar
    4  -rw-        1038  Jun 28 2012 23:27:58 +00:00  home.shtml
    5  -rw-      122880  Jun 28 2012 23:28:08 +00:00  home.tar
    6  -rw-     1697952  Jun 28 2012 23:28:22 +00:00  securedesktop-ios-3.1.1.45-k9.pkg
    7  -rw-      415956  Jun 28 2012 23:28:36 +00:00  sslclient-win-1.1.4.176.pkg
    8  -rw-    68660044  Jan 13 2014 22:13:08 +00:00  c1900-universalk9-mz.SPA.154-1.T.bin
    9  -rw-      272337  Jan 15 2014 05:41:22 +00:00  crashinfo_20140115-054123-UTC
   10  -rw-      242502  Jan 15 2014 05:48:22 +00:00  crashinfo_20140115-054823-UTC
   11  -rw-      243196  Jan 15 2014 23:43:34 +00:00  crashinfo_20140115-234335-UTC
   12  -rw-      234635  Jan 15 2014 23:51:38 +00:00  crashinfo_20140115-235138-UTC
   13  -rw-      233738  Jan 16 2014 01:09:48 +00:00  crashinfo_20140116-010948-UTC
   14  -rw-      227603  Jan 16 2014 01:40:22 +00:00  crashinfo_20140116-014023-UTC
   15  -rw-    67200372  Jan 16 2014 02:24:50 +00:00  c1900-universalk9-mz.SPA.153-3.M.bin

256487424 bytes total (58810368 bytes free)

=====================

Я так понимаю, что это и есть те самые crash дампы, которые образовались после выполнения команд nat inside / nat outsice.

    9  -rw-      272337  Jan 15 2014 05:41:22 +00:00  crashinfo_20140115-054123-UTC
   10  -rw-      242502  Jan 15 2014 05:48:22 +00:00  crashinfo_20140115-054823-UTC
   11  -rw-      243196  Jan 15 2014 23:43:34 +00:00  crashinfo_20140115-234335-UTC
   12  -rw-      234635  Jan 15 2014 23:51:38 +00:00  crashinfo_20140115-235138-UTC
   13  -rw-      233738  Jan 16 2014 01:09:48 +00:00  crashinfo_20140116-010948-UTC
   14  -rw-      227603  Jan 16 2014 01:40:22 +00:00  crashinfo_20140116-014023-UTC

=====================

#copy flash0:crashinfo_20140115-054123-UTC tftp://192.168.1.5
Address or name of remote host [192.168.1.5]?
Destination filename [crashinfo_20140115-054123-UTC]?
%Error opening tftp://192.168.1.5/crashinfo_20140115-054123-UTC (Permission denied)

Ерунда какая-то, для того, чтобы скопировать файл.
Я должен сначала его создать с правами 777.

Ubuntu:

$ touch /tftpboot/crashinfo_20140115-054123-UTC
$ chmod 777 /tftpboot/crashinfo_20140115-054123-UTC

$ touch /tftpboot/crashinfo_20140115-054823-UTC
$ chmod 777 /tftpboot/crashinfo_20140115-054823-UTC

$ touch /tftpboot/crashinfo_20140115-234335-UTC
$ chmod 777 /tftpboot/crashinfo_20140115-234335-UTC


$ touch /tftpboot/crashinfo_20140115-235138-UTC
$ chmod 777 /tftpboot/crashinfo_20140115-235138-UTC


$ touch /tftpboot/crashinfo_20140116-010948-UTC
$ chmod 777 /tftpboot/crashinfo_20140116-010948-UTC


$ touch /tftpboot/crashinfo_20140116-014023-UTC
$ chmod 777 /tftpboot/crashinfo_20140116-014023-UTC




Cisco
copy flash0:crashinfo_20140115-054123-UTC tftp://192.168.1.5/crashinfo_20140115-054123-UTC
copy flash0:crashinfo_20140115-054823-UTC tftp://192.168.1.5/crashinfo_20140115-054823-UTC
copy flash0:crashinfo_20140115-234335-UTC tftp://192.168.1.5/crashinfo_20140115-234335-UTC
copy flash0:crashinfo_20140115-235138-UTC tftp://192.168.1.5/crashinfo_20140115-235138-UTC
copy flash0:crashinfo_20140116-010948-UTC tftp://192.168.1.5/crashinfo_20140116-010948-UTC
copy flash0:crashinfo_20140116-014023-UTC tftp://192.168.1.5/crashinfo_20140116-014023-UTC

Отправил крашдампы в техподдержку cisco.
Так как контракта у меня нет, мой сервис реквест быстро закрыли.
Так что посоны, ошибка будет исправлена но не моими усилиями.

=====================

Скопирую running-config к себе на локальную машину:

Ubuntu:
$ touch /tftpboot/running-config
$ chmod 777 /tftpboot/running-config


Cisco:
cisco-router-1941#copy system:running-config tftp://192.168.1.5/running-config
Address or name of remote host [192.168.1.5]?
Destination filename [running-config]?
!!
1488 bytes copied in 0.120 secs (12400 bytes/sec)


// Удаляю крашдампы
cisco-router-1941#delete flash0:crashinfo_20140115-054823-UTC

</pre>

<br/>

### Небольшая профилактика

<pre>

// Видим несколько записей
cisco-router-1941#sh run | i boot system
boot system flash c1900-universalk9-mz.spa.153-3.m.bin
boot system flash c1900-universalk9-mz.SPA.154-1.T.bin

// Удалаяю ненужную

cisco-router-1941#conf t
cisco-router-1941(config)# no boot system flash c1900-universalk9-mz.spa.153-3.m.bin
cisco-router-1941(config)# end
cisco-router-1941# write


</pre>
