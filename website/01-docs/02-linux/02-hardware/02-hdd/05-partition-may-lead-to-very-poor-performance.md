---
layout: page
title: Ошибка при попытке установить Ubuntu на внешний жесткий диск
permalink: /linux/hardware/hdd/partition-may-lead-to-very-poor-performance/
---

# Ошибка при попытке установить Ubuntu на внешний жесткий диск

### The partition /dev/sdb1 assigned to / starts at an offset of 3584 bytes from the minimum alignment for this disk, which may lead to very poor performance

<br/>

<div align="center">
	<img src="//files.sysadm.ru/img/linux/hardware/hdd/partition-may-lead-to-very-poor-performance.jpg" alt="The partition /dev/sdb1 assigned to / starts at an offset of 3584 bytes from the minimum alignment for this disk, which may lead to very poor performance" border="0" />
</div>


<br/>


Загрузился в Ubuntu, установил gparted. С помощью gparted создал разделы следующим образом.



    After booting into live LL, open GParted.

    Go to Device -> Create partition table -> "msdos" -> Apply

    Highlight unallocated space, click "New" button to create a Root partition
    Free space preceding = leave whatever is pre-filled there
    New size = 25600
    Free space following = leave what gets auto-filled
    Create as = Primary Partition
    File system = ext4
    Label = leave blank
    Click "Add" button when done

    Highlight unallocated space, click "New" button to create a Swap partition
    Free space preceding = leave whatever is pre-filled there
    New size = 4096
    Free space following = leave what gets auto-filled
    Create as = Primary Partition
    File system = linux-swap
    Label = leave blank
    Click "Add" button when done

    Highlight unallocated space, click "New" button to create a Home partition
    Free space preceding = leave whatever is pre-filled there
    New size = rest of available space (or less if you want)
    Free space following = leave what gets auto-filled
    Create as = Primary Partition
    File system = ext4
    Label = leave blank
    Click "Add" button when done

    Hit "Apply" button along top of GParted interface to finalize the changes.

    Close GParted when done.



    Start the installation and choose "Something else" option.  On partition selection page do the following.

    Highlight the Root partition you made, then click the "Change" button (near bottom-left)
    Use as = Ext4 file system
    Size = leave as is
    Mount Point = /
    Format = check box to format
    Click "Done". (If you get a message complaining that you changed the size of the partition -- even though you did not -- ignore it and click "Go Back" button. All will be fine.)

    Highlight the Home partition you made, then click the "Change" button
    Use as = Ext4 file system
    Size = leave as is
    Mount Point = /home
    Format = check box to format
    Click "Done". (If you get a message complaining that you changed the size of the partition -- even though you did not -- ignore it and click "Go Back" button. All will be fine.)

    No need to do anything with Swap partition -- it will be automatically used by installer.

    Near bottom of window, "Device for boot loader installation" should be set to "/dev/sdb".  I'm assuming that the external drive is /dev/sdb here.  If it is something else (eg. /dev/sdc, or sdd) change that "b" to whatever it should be.  Also note, there is no partition number after the "b" in "/dev/sdb".  That will aim grub's first stage of boot loader to the MBR of the drive making it bootable when you tell computer to boot from it.

    Click "Finish" button to complete the installation


Повторил. Все установилось.

https://www.linuxliteos.com/forums/installing-linux-lite/partition-alignment-prompt-at-install/
