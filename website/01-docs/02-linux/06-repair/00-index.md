---
layout: page
title: Попытка исключения использования бэд блоков файловой системой в Ubuntu
permalink: /linux/ubuntu/get-info-about-hdd/
---


# Советую внимательно смотреть на название диска и его разделы.


    # apt-get install -y smartmontools

<br/>

    # smartctl -a /dev/sdb
    smartctl 6.2 2013-07-26 r3841 [x86_64-linux-3.19.0-42-generic] (local build)
    Copyright (C) 2002-13, Bruce Allen, Christian Franke, www.smartmontools.org

    === START OF INFORMATION SECTION ===
    Model Family:     Seagate Barracuda 7200.14 (AF)
    Device Model:     ST2000DM001-9YN164
    Serial Number:    S1E02CG0
    LU WWN Device Id: 5 000c50 04a892e6f
    Firmware Version: CC4H
    User Capacity:    2 000 398 934 016 bytes [2,00 TB]
    Sector Sizes:     512 bytes logical, 4096 bytes physical
    Rotation Rate:    7200 rpm
    Device is:        In smartctl database [for details use: -P show]
    ATA Version is:   ATA8-ACS T13/1699-D revision 4
    SATA Version is:  SATA 3.0, 6.0 Gb/s (current: 3.0 Gb/s)
    Local Time is:    Sat Jan 30 23:12:53 2016 MSK
    SMART support is: Available - device has SMART capability.
    SMART support is: Enabled

    === START OF READ SMART DATA SECTION ===
    SMART overall-health self-assessment test result: PASSED

    General SMART Values:
    Offline data collection status:  (0x82)	Offline data collection activity
    					was completed without error.
    					Auto Offline Data Collection: Enabled.
    Self-test execution status:      (   0)	The previous self-test routine completed
    					without error or no self-test has ever
    					been run.
    Total time to complete Offline
    data collection: 		(  584) seconds.
    Offline data collection
    capabilities: 			 (0x7b) SMART execute Offline immediate.
    					Auto Offline data collection on/off support.
    					Suspend Offline collection upon new
    					command.
    					Offline surface scan supported.
    					Self-test supported.
    					Conveyance Self-test supported.
    					Selective Self-test supported.
    SMART capabilities:            (0x0003)	Saves SMART data before entering
    					power-saving mode.
    					Supports SMART auto save timer.
    Error logging capability:        (0x01)	Error logging supported.
    					General Purpose Logging supported.
    Short self-test routine
    recommended polling time: 	 (   1) minutes.
    Extended self-test routine
    recommended polling time: 	 ( 231) minutes.
    Conveyance self-test routine
    recommended polling time: 	 (   2) minutes.
    SCT capabilities: 	       (0x3085)	SCT Status supported.

    SMART Attributes Data Structure revision number: 10
    Vendor Specific SMART Attributes with Thresholds:
    ID# ATTRIBUTE_NAME          FLAG     VALUE WORST THRESH TYPE      UPDATED  WHEN_FAILED RAW_VALUE
      1 Raw_Read_Error_Rate     0x000f   085   085   006    Pre-fail  Always       -       159962165
      3 Spin_Up_Time            0x0003   095   094   000    Pre-fail  Always       -       0
      4 Start_Stop_Count        0x0032   100   100   020    Old_age   Always       -       94
      5 Reallocated_Sector_Ct   0x0033   063   054   036    Pre-fail  Always       -       49456
      7 Seek_Error_Rate         0x000f   084   060   030    Pre-fail  Always       -       280601610
      9 Power_On_Hours          0x0032   067   067   000    Old_age   Always       -       29241
     10 Spin_Retry_Count        0x0013   100   100   097    Pre-fail  Always       -       0
     12 Power_Cycle_Count       0x0032   100   100   020    Old_age   Always       -       93
    183 Runtime_Bad_Block       0x0032   100   100   000    Old_age   Always       -       0
    184 End-to-End_Error        0x0032   100   100   099    Old_age   Always       -       0
    187 Reported_Uncorrect      0x0032   032   032   000    Old_age   Always       -       68
    188 Command_Timeout         0x0032   100   096   000    Old_age   Always       -       15 15 18
    189 High_Fly_Writes         0x003a   100   100   000    Old_age   Always       -       0
    190 Airflow_Temperature_Cel 0x0022   060   051   045    Old_age   Always       -       40 (Min/Max 31/40)
    191 G-Sense_Error_Rate      0x0032   100   100   000    Old_age   Always       -       0
    192 Power-Off_Retract_Count 0x0032   100   100   000    Old_age   Always       -       91
    193 Load_Cycle_Count        0x0032   097   097   000    Old_age   Always       -       7727
    194 Temperature_Celsius     0x0022   040   049   000    Old_age   Always       -       40 (0 18 0 0 0)
    197 Current_Pending_Sector  0x0012   100   100   000    Old_age   Always       -       128
    198 Offline_Uncorrectable   0x0010   100   100   000    Old_age   Offline      -       128
    199 UDMA_CRC_Error_Count    0x003e   200   200   000    Old_age   Always       -       1
    240 Head_Flying_Hours       0x0000   100   253   000    Old_age   Offline      -       29025h+28m+50.931s
    241 Total_LBAs_Written      0x0000   100   253   000    Old_age   Offline      -       81735190510632
    242 Total_LBAs_Read         0x0000   100   253   000    Old_age   Offline      -       123261680378241

    SMART Error Log Version: 1
    ATA Error Count: 20 (device log contains only the most recent five errors)
    	CR = Command Register [HEX]
    	FR = Features Register [HEX]
    	SC = Sector Count Register [HEX]
    	SN = Sector Number Register [HEX]
    	CL = Cylinder Low Register [HEX]
    	CH = Cylinder High Register [HEX]
    	DH = Device/Head Register [HEX]
    	DC = Device Command Register [HEX]
    	ER = Error register [HEX]
    	ST = Status register [HEX]
    Powered_Up_Time is measured from power on, and printed as
    DDd+hh:mm:SS.sss where DD=days, hh=hours, mm=minutes,
    SS=sec, and sss=millisec. It "wraps" after 49.710 days.

    Error 20 occurred at disk power-on lifetime: 29241 hours (1218 days + 9 hours)
      When the command that caused the error occurred, the device was active or idle.

      After command completion occurred, registers were:
      ER ST SC SN CL CH DH
      -- -- -- -- -- -- --
      40 51 00 c8 8c 1c 00  Error: UNC at LBA = 0x001c8cc8 = 1871048

      Commands leading to the command that caused the error were:
      CR FR SC SN CL CH DH DC   Powered_Up_Time  Command/Feature_Name
      -- -- -- -- -- -- -- --  ----------------  --------------------
      c8 00 08 c8 8c 1c e0 00      02:28:59.941  READ DMA
      c8 00 08 c0 8c 1c e0 00      02:28:59.541  READ DMA
      c8 00 08 b8 8c 1c e0 00      02:28:59.216  READ DMA
      c8 00 08 b0 8c 1c e0 00      02:28:59.151  READ DMA
      c8 00 08 a8 8c 1c e0 00      02:28:59.095  READ DMA

    Error 19 occurred at disk power-on lifetime: 29238 hours (1218 days + 6 hours)
      When the command that caused the error occurred, the device was active or idle.

      After command completion occurred, registers were:
      ER ST SC SN CL CH DH
      -- -- -- -- -- -- --
      40 51 00 ff ff ff 0f  Error: UNC at LBA = 0x0fffffff = 268435455

      Commands leading to the command that caused the error were:
      CR FR SC SN CL CH DH DC   Powered_Up_Time  Command/Feature_Name
      -- -- -- -- -- -- -- --  ----------------  --------------------
      25 00 08 ff ff ff ef 00      00:01:23.261  READ DMA EXT
      25 00 08 ff ff ff ef 00      00:01:23.260  READ DMA EXT
      25 00 08 ff ff ff ef 00      00:01:23.260  READ DMA EXT
      25 00 08 ff ff ff ef 00      00:01:23.259  READ DMA EXT
      25 00 08 ff ff ff ef 00      00:01:23.259  READ DMA EXT

    Error 18 occurred at disk power-on lifetime: 29238 hours (1218 days + 6 hours)
      When the command that caused the error occurred, the device was active or idle.

      After command completion occurred, registers were:
      ER ST SC SN CL CH DH
      -- -- -- -- -- -- --
      40 51 00 ff ff ff 0f  Error: UNC at LBA = 0x0fffffff = 268435455

      Commands leading to the command that caused the error were:
      CR FR SC SN CL CH DH DC   Powered_Up_Time  Command/Feature_Name
      -- -- -- -- -- -- -- --  ----------------  --------------------
      25 00 00 ff ff ff ef 00      00:01:20.307  READ DMA EXT
      25 00 00 ff ff ff ef 00      00:01:20.289  READ DMA EXT
      25 00 00 ff ff ff ef 00      00:01:20.253  READ DMA EXT
      25 00 00 ff ff ff ef 00      00:01:20.252  READ DMA EXT
      25 00 e0 ff ff ff ef 00      00:01:20.250  READ DMA EXT

    Error 17 occurred at disk power-on lifetime: 29238 hours (1218 days + 6 hours)
      When the command that caused the error occurred, the device was active or idle.

      After command completion occurred, registers were:
      ER ST SC SN CL CH DH
      -- -- -- -- -- -- --
      40 51 00 ff ff ff 0f  Error: UNC at LBA = 0x0fffffff = 268435455

      Commands leading to the command that caused the error were:
      CR FR SC SN CL CH DH DC   Powered_Up_Time  Command/Feature_Name
      -- -- -- -- -- -- -- --  ----------------  --------------------
      25 00 08 ff ff ff ef 00      00:01:17.009  READ DMA EXT
      25 00 08 ff ff ff ef 00      00:01:17.008  READ DMA EXT
      25 00 08 ff ff ff ef 00      00:01:17.008  READ DMA EXT
      25 00 08 ff ff ff ef 00      00:01:17.008  READ DMA EXT
      25 00 08 ff ff ff ef 00      00:01:17.008  READ DMA EXT

    Error 16 occurred at disk power-on lifetime: 29238 hours (1218 days + 6 hours)
      When the command that caused the error occurred, the device was active or idle.

      After command completion occurred, registers were:
      ER ST SC SN CL CH DH
      -- -- -- -- -- -- --
      40 51 00 ff ff ff 0f  Error: UNC at LBA = 0x0fffffff = 268435455

      Commands leading to the command that caused the error were:
      CR FR SC SN CL CH DH DC   Powered_Up_Time  Command/Feature_Name
      -- -- -- -- -- -- -- --  ----------------  --------------------
      25 00 00 ff ff ff ef 00      00:01:14.031  READ DMA EXT
      25 00 00 ff ff ff ef 00      00:01:14.013  READ DMA EXT
      25 00 00 ff ff ff ef 00      00:01:13.970  READ DMA EXT
      25 00 00 ff ff ff ef 00      00:01:13.969  READ DMA EXT
      25 00 00 ff ff ff ef 00      00:01:13.969  READ DMA EXT

    SMART Self-test log structure revision number 1
    No self-tests have been logged.  [To run self-tests, use: smartctl -t]


    SMART Selective self-test log data structure revision number 1
     SPAN  MIN_LBA  MAX_LBA  CURRENT_TEST_STATUS
        1        0        0  Not_testing
        2        0        0  Not_testing
        3        0        0  Not_testing
        4        0        0  Not_testing
        5        0        0  Not_testing
    Selective self-test flags (0x0):
      After scanning selected spans, do NOT read-scan remainder of disk.
    If Selective self-test is pending on power-up, resume after 0 minute delay.



<br/><br/>

Если верить написанному в интернете:

    1    Raw_Read_Error_Rate

    VALUE это текущее значение (остаток здоровья)
    THRESH - это значение при котором умрет


<br/>


Почитать, если что:

https://www.opennet.ru/base/sys/smart_hdd_mon.txt.html

<br/><br/>


### Может DD сможет забить диск нулями? И вдруг окажется, что никаких критических ошибок на диске на самом деле нет?


Хотя на таком диске ничего уже хранить важного я бы не стал. С него можно, например, торренты раздавать, хранить индийские фильмы.

Я в этом также разбираюсь, как свинья в апельсинах!


    # dd if=/dev/zero of=/dev/sdb
    dd: writing to ‘/dev/sdb’: Input/output error
    1871049+0 records in
    1871048+0 records out
    957976576 bytes (958 MB) copied, 8600,9 s, 111 kB/s


Лучше запускать, чтобы было видно прогресс.

    # apt-get install -y pv

    # dd if=/dev/zero | pv | dd of=/dev/sdb

<br/>

Вообщем команда dd только подтвердила, что диску не хорошо. Впринципе, она ничем и не помогла.



### Попытка исключения использования бэд блоков файловой системой

    # fdisk /dev/sdb
    Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
    Building a new DOS disklabel with disk identifier 0x3ee07305.
    Changes will remain in memory only, until you decide to write them.
    After that, of course, the previous content won't be recoverable.

    Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

    The device presents a logical sector size that is smaller than
    the physical sector size. Aligning to a physical sector (or optimal
    I/O) size boundary is recommended, or performance may be impacted.

    Command (m for help): n
    Partition type:
       p   primary (0 primary, 0 extended, 4 free)
       e   extended
    Select (default p): p
    Partition number (1-4, default 1): 1
    First sector (2048-3907029167, default 2048):
    Using default value 2048
    Last sector, +sectors or +size{K,M,G} (2048-3907029167, default 3907029167):
    Using default value 3907029167

    Command (m for help):
    Command (m for help): w
    The partition table has been altered!


<br/>

    # mkfs.ext4 /dev/sdb1
    mke2fs 1.42.9 (4-Feb-2014)
    Filesystem label=
    OS type: Linux
    Block size=4096 (log=2)
    Fragment size=4096 (log=2)
    Stride=0 blocks, Stripe width=0 blocks
    122101760 inodes, 488378390 blocks
    24418919 blocks (5.00%) reserved for the super user
    First data block=0
    Maximum filesystem blocks=4294967296
    14905 block groups
    32768 blocks per group, 32768 fragments per group
    8192 inodes per group
    Superblock backups stored on blocks:
    	32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
    	4096000, 7962624, 11239424, 20480000, 23887872, 71663616, 78675968,
    	102400000, 214990848

    Allocating group tables: done                            
    Writing inode tables: done                            
    Creating journal (32768 blocks): done
    Writing superblocks and filesystem accounting information:            

    done


<br/>

Создаем список бедблоков:

    # badblocks -s /dev/sdb1 > ~/badblocks_sdb1.txt


Пометка бэд блоков (в дальнейшем помеченные блоки будут игнорироваться):

    # e2fsck -l ~/badblocks_sdb1.txt /dev/sdb1



<br/>

### Результаты

В одном случае получилось. В другом нет.

В том котором не получилось, диск наткнулся на ошибку, по которой он вообще перестал быть доступен системе.
Так как объем у него аж 2 TB, попробую позднее разбить его на меньшие файловые системы по объему и проделать теже операции. 
