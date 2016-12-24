---
layout: page
title: Cisco Router 1941 данные о железе
permalink: /devices/cisco/routers/1941/device-hardware/
---


# Cisco Router 1941 данные о железе

    # sh version


Объем оперативной памяти - выводится в виде двух чисел: объема процессорной памяти (491520K) и памяти ввода-вывода (32768K). Общий размер RAM равен сумме этих двух (512M);




    cisco-router-1941#show file system
    File Systems:

           Size(b)       Free(b)      Type  Flags  Prefixes
                 -             -    opaque     rw   archive:
                 -             -    opaque     rw   system:
                 -             -    opaque     rw   tmpsys:
                 -             -    opaque     rw   null:
                 -             -   network     rw   tftp:
    *    256487424     127959040      disk     rw   flash0: flash:#
                 -             -      disk     rw   flash1:
            262136        249595     nvram     rw   nvram:
                 -             -    opaque     wo   syslog:
                 -             -    opaque     rw   xmodem:
                 -             -    opaque     rw   ymodem:
                 -             -   network     rw   rcp:
                 -             -   network     rw   pram:
                 -             -   network     rw   http:
                 -             -   network     rw   ftp:
                 -             -   network     rw   scp:
                 -             -    opaque     ro   tar:
                 -             -   network     rw   https:
                 -             -    opaque     ro   cns:




<br/>

    router# show processes
    router# show processes cpu
    router# show processes memory

<br/>

    # show memory
    # show memory summary
