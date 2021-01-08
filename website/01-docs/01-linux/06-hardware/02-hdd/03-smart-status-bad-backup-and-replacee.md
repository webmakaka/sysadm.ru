---
layout: page
title: S.M.A.R.T. status bad backup and replace
description: S.M.A.R.T. status bad backup and replace
keywords: S.M.A.R.T. status bad backup and replace
permalink: /linux/hardware/hdd/smart-status-bad-backup-and-replace/
---

# S.M.A.R.T. status bad backup and replace

По содержанию все было понятно.
Но было непонятно о каком диске идеть речь.
Когда в компьютере 5 дисков и более, не так сразу и разберешься, с какого диска нужно спасать данные.

<br/>

    # apt-get install -y smartmontools

<br/>

**Вывод данных с диска без ошибок:**

    # smartctl -H /dev/sdd
    smartctl 6.2 2013-07-26 r3841 [x86_64-linux-4.4.0-53-generic] (local build)
    Copyright (C) 2002-13, Bruce Allen, Christian Franke, www.smartmontools.org

    === START OF READ SMART DATA SECTION ===
    SMART overall-health self-assessment test result: PASSED

<br/>

**Вывод данных с диска с ошибкой:**

    # smartctl -H /dev/sde
    smartctl 6.2 2013-07-26 r3841 [x86_64-linux-4.4.0-53-generic] (local build)
    Copyright (C) 2002-13, Bruce Allen, Christian Franke, www.smartmontools.org

    === START OF READ SMART DATA SECTION ===
    SMART overall-health self-assessment test result: FAILED!
    Drive failure expected in less than 24 hours. SAVE ALL DATA.
    Failed Attributes:
    ID# ATTRIBUTE_NAME          FLAG     VALUE WORST THRESH TYPE      UPDATED  WHEN_FAILED RAW_VALUE
      5 Reallocated_Sector_Ct   0x0033   019   019   036    Pre-fail  Always   FAILING_NOW 3339

<br/>

**Reallocated_Sector_Ct** - The SMART Attributes Data Structure section contains many useful parts. Reallocated_Sector_Ct is how many sectors have been reallocated to to errors. Some sector reallocations are OK, but if this number start to grow it is an indication that your disk is getting sick.

<br/>

### Так, чего делаем то?

    # mount | grep sde
    /dev/sde2 on /mnt/dsk3 type ext4 (rw)

<br/>

    $ cd /mnt/dsk3

<br/>

    $ du -h
    171G

<br/>

Перекидываем все содержимое на другой диск, а дальше посмотрим.

<br/>

Значит всетаки Seagate Barracuda 1 TB

    # smartctl -i /dev/sde
    smartctl 6.2 2013-07-26 r3841 [x86_64-linux-4.4.0-53-generic] (local build)
    Copyright (C) 2002-13, Bruce Allen, Christian Franke, www.smartmontools.org

    === START OF INFORMATION SECTION ===
    Model Family:     Seagate Barracuda 7200.12
    Device Model:     ST31000528AS
    Serial Number:    9VP5ST4F
    LU WWN Device Id: 5 000c50 020102afb
    Firmware Version: CC38
    User Capacity:    1,000,204,886,016 bytes [1.00 TB]
    Sector Size:      512 bytes logical/physical
    Rotation Rate:    7200 rpm
    Device is:        In smartctl database [for details use: -P show]
    ATA Version is:   ATA8-ACS T13/1699-D revision 4
    SATA Version is:  SATA 2.6, 3.0 Gb/s
    Local Time is:    Tue Jan 31 20:40:43 2017 MSK

    ==> WARNING: A firmware update for this drive may be available,
    see the following Seagate web pages:
    http://knowledge.seagate.com/articles/en_US/FAQ/207931en
    http://knowledge.seagate.com/articles/en_US/FAQ/213891en

    SMART support is: Available - device has SMART capability.
    SMART support is: Enabled

<br/>

### Более подробный вывод, может кому полезно будет

    # smartctl -a /dev/sde
    smartctl 6.2 2013-07-26 r3841 [x86_64-linux-4.4.0-53-generic] (local build)
    Copyright (C) 2002-13, Bruce Allen, Christian Franke, www.smartmontools.org

    === START OF INFORMATION SECTION ===
    Model Family:     Seagate Barracuda 7200.12
    Device Model:     ST31000528AS
    Serial Number:    9VP5ST4F
    LU WWN Device Id: 5 000c50 020102afb
    Firmware Version: CC38
    User Capacity:    1,000,204,886,016 bytes [1.00 TB]
    Sector Size:      512 bytes logical/physical
    Rotation Rate:    7200 rpm
    Device is:        In smartctl database [for details use: -P show]
    ATA Version is:   ATA8-ACS T13/1699-D revision 4
    SATA Version is:  SATA 2.6, 3.0 Gb/s
    Local Time is:    Tue Jan 31 20:42:29 2017 MSK

    ==> WARNING: A firmware update for this drive may be available,
    see the following Seagate web pages:
    http://knowledge.seagate.com/articles/en_US/FAQ/207931en
    http://knowledge.seagate.com/articles/en_US/FAQ/213891en

    SMART support is: Available - device has SMART capability.
    SMART support is: Enabled

    === START OF READ SMART DATA SECTION ===
    SMART overall-health self-assessment test result: FAILED!
    Drive failure expected in less than 24 hours. SAVE ALL DATA.
    See vendor-specific Attribute list for failed Attributes.

    General SMART Values:
    Offline data collection status:  (0x82)	Offline data collection activity
    					was completed without error.
    					Auto Offline Data Collection: Enabled.
    Self-test execution status:      (   0)	The previous self-test routine completed
    					without error or no self-test has ever
    					been run.
    Total time to complete Offline
    data collection: 		(  600) seconds.
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
    recommended polling time: 	 ( 183) minutes.
    Conveyance self-test routine
    recommended polling time: 	 (   2) minutes.
    SCT capabilities: 	       (0x103f)	SCT Status supported.
    					SCT Error Recovery Control supported.
    					SCT Feature Control supported.
    					SCT Data Table supported.

    SMART Attributes Data Structure revision number: 10
    Vendor Specific SMART Attributes with Thresholds:
    ID# ATTRIBUTE_NAME          FLAG     VALUE WORST THRESH TYPE      UPDATED  WHEN_FAILED RAW_VALUE
      1 Raw_Read_Error_Rate     0x000f   104   085   006    Pre-fail  Always       -       6970049
      3 Spin_Up_Time            0x0003   095   095   000    Pre-fail  Always       -       0
      4 Start_Stop_Count        0x0032   099   099   020    Old_age   Always       -       1051
      5 Reallocated_Sector_Ct   0x0033   017   017   036    Pre-fail  Always   FAILING_NOW 3432
      7 Seek_Error_Rate         0x000f   080   060   030    Pre-fail  Always       -       116598448
      9 Power_On_Hours          0x0032   064   064   000    Old_age   Always       -       31739
     10 Spin_Retry_Count        0x0013   100   100   097    Pre-fail  Always       -       0
     12 Power_Cycle_Count       0x0032   100   100   020    Old_age   Always       -       525
    183 Runtime_Bad_Block       0x0032   100   100   000    Old_age   Always       -       0
    184 End-to-End_Error        0x0032   100   100   099    Old_age   Always       -       0
    187 Reported_Uncorrect      0x0032   001   001   000    Old_age   Always       -       691
    188 Command_Timeout         0x0032   100   099   000    Old_age   Always       -       55835426829
    189 High_Fly_Writes         0x003a   032   032   000    Old_age   Always       -       68
    190 Airflow_Temperature_Cel 0x0022   061   050   045    Old_age   Always       -       39 (Min/Max 22/39)
    194 Temperature_Celsius     0x0022   039   050   000    Old_age   Always       -       39 (0 15 0 0 0)
    195 Hardware_ECC_Recovered  0x001a   045   017   000    Old_age   Always       -       6970049
    197 Current_Pending_Sector  0x0012   097   093   000    Old_age   Always       -       145
    198 Offline_Uncorrectable   0x0010   097   093   000    Old_age   Offline      -       145
    199 UDMA_CRC_Error_Count    0x003e   200   200   000    Old_age   Always       -       3
    240 Head_Flying_Hours       0x0000   100   253   000    Old_age   Offline      -       64841121300743
    241 Total_LBAs_Written      0x0000   100   253   000    Old_age   Offline      -       2686482257
    242 Total_LBAs_Read         0x0000   100   253   000    Old_age   Offline      -       1888900257

    SMART Error Log Version: 1
    ATA Error Count: 278 (device log contains only the most recent five errors)
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

    Error 278 occurred at disk power-on lifetime: 28078 hours (1169 days + 22 hours)
      When the command that caused the error occurred, the device was active or idle.

      After command completion occurred, registers were:
      ER ST SC SN CL CH DH
      -- -- -- -- -- -- --
      40 51 00 ff ff ff 0f  Error: UNC at LBA = 0x0fffffff = 268435455

      Commands leading to the command that caused the error were:
      CR FR SC SN CL CH DH DC   Powered_Up_Time  Command/Feature_Name
      -- -- -- -- -- -- -- --  ----------------  --------------------
      25 00 08 ff ff ff ef 00      03:52:28.008  READ DMA EXT
      27 00 00 00 00 00 e0 00      03:52:28.007  READ NATIVE MAX ADDRESS EXT [OBS-ACS-3]
      ec 00 00 00 00 00 a0 00      03:52:27.998  IDENTIFY DEVICE
      ef 03 46 00 00 00 a0 00      03:52:27.992  SET FEATURES [Set transfer mode]
      27 00 00 00 00 00 e0 00      03:52:27.971  READ NATIVE MAX ADDRESS EXT [OBS-ACS-3]

    Error 277 occurred at disk power-on lifetime: 28078 hours (1169 days + 22 hours)
      When the command that caused the error occurred, the device was active or idle.

      After command completion occurred, registers were:
      ER ST SC SN CL CH DH
      -- -- -- -- -- -- --
      40 51 00 ff ff ff 0f  Error: UNC at LBA = 0x0fffffff = 268435455

      Commands leading to the command that caused the error were:
      CR FR SC SN CL CH DH DC   Powered_Up_Time  Command/Feature_Name
      -- -- -- -- -- -- -- --  ----------------  --------------------
      25 00 08 ff ff ff ef 00      03:52:24.851  READ DMA EXT
      27 00 00 00 00 00 e0 00      03:52:24.850  READ NATIVE MAX ADDRESS EXT [OBS-ACS-3]
      ec 00 00 00 00 00 a0 00      03:52:24.842  IDENTIFY DEVICE
      ef 03 46 00 00 00 a0 00      03:52:24.837  SET FEATURES [Set transfer mode]
      27 00 00 00 00 00 e0 00      03:52:24.814  READ NATIVE MAX ADDRESS EXT [OBS-ACS-3]

    Error 276 occurred at disk power-on lifetime: 28078 hours (1169 days + 22 hours)
      When the command that caused the error occurred, the device was active or idle.

      After command completion occurred, registers were:
      ER ST SC SN CL CH DH
      -- -- -- -- -- -- --
      40 51 00 ff ff ff 0f  Error: UNC at LBA = 0x0fffffff = 268435455

      Commands leading to the command that caused the error were:
      CR FR SC SN CL CH DH DC   Powered_Up_Time  Command/Feature_Name
      -- -- -- -- -- -- -- --  ----------------  --------------------
      25 00 08 ff ff ff ef 00      03:52:21.715  READ DMA EXT
      27 00 00 00 00 00 e0 00      03:52:21.714  READ NATIVE MAX ADDRESS EXT [OBS-ACS-3]
      ec 00 00 00 00 00 a0 00      03:52:21.706  IDENTIFY DEVICE
      ef 03 46 00 00 00 a0 00      03:52:21.698  SET FEATURES [Set transfer mode]
      27 00 00 00 00 00 e0 00      03:52:21.678  READ NATIVE MAX ADDRESS EXT [OBS-ACS-3]

    Error 275 occurred at disk power-on lifetime: 28078 hours (1169 days + 22 hours)
      When the command that caused the error occurred, the device was active or idle.

      After command completion occurred, registers were:
      ER ST SC SN CL CH DH
      -- -- -- -- -- -- --
      40 51 00 ff ff ff 0f  Error: UNC at LBA = 0x0fffffff = 268435455

      Commands leading to the command that caused the error were:
      CR FR SC SN CL CH DH DC   Powered_Up_Time  Command/Feature_Name
      -- -- -- -- -- -- -- --  ----------------  --------------------
      25 00 08 ff ff ff ef 00      03:52:18.620  READ DMA EXT
      25 00 08 ff ff ff ef 00      03:52:18.619  READ DMA EXT
      25 00 08 ff ff ff ef 00      03:52:18.619  READ DMA EXT
      25 00 08 ff ff ff ef 00      03:52:18.619  READ DMA EXT
      25 00 08 ff ff ff ef 00      03:52:18.619  READ DMA EXT

    Error 274 occurred at disk power-on lifetime: 28078 hours (1169 days + 22 hours)
      When the command that caused the error occurred, the device was active or idle.

      After command completion occurred, registers were:
      ER ST SC SN CL CH DH
      -- -- -- -- -- -- --
      40 51 00 ff ff ff 0f  Error: UNC at LBA = 0x0fffffff = 268435455

      Commands leading to the command that caused the error were:
      CR FR SC SN CL CH DH DC   Powered_Up_Time  Command/Feature_Name
      -- -- -- -- -- -- -- --  ----------------  --------------------
      25 00 08 ff ff ff ef 00      03:52:15.362  READ DMA EXT
      e5 00 00 00 00 00 00 00      03:52:15.361  CHECK POWER MODE
      27 00 00 00 00 00 e0 00      03:52:15.361  READ NATIVE MAX ADDRESS EXT [OBS-ACS-3]
      ec 00 00 00 00 00 a0 00      03:52:15.353  IDENTIFY DEVICE
      ef 03 46 00 00 00 a0 00      03:52:15.347  SET FEATURES [Set transfer mode]

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
