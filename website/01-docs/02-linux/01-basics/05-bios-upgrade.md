---
layout: page
title: Upgrade BIOS в linux
permalink: /linux/basics/bios-upgrade/
---

Мне понадобилось обновить BIOS на стром компьютере.
Простого способа прошить, выбрать Azeru или как он там называется не было.

Т.к. у меня на этом компьютере были проблемы с жесткими дисками, я для этого стартовал Ubuntu c флешки.


Смотрим какая версия биоса текущая:

    # dmidecode -s bios-version
    ASUS A8N-SLI ACPI BIOS Revision 1014

<br/>

### flashrom

В стандартном списке репо не было flashrom.

Вообщем я поставил, следующим способом.

    # add-apt-repository ppa:flashrom-developers/flashrom-daily
    # apt-get update
    # apt-get install -y flashrom

<br/>

### Upgrade

// Резервная копия текущего биос

    # flashrom --programmer internal -r MYBIOS.ROM


// Прошивка

    # flashrom --programmer internal -w 1303.BIN  -V
    flashrom v0.9.8-unknown on Linux 4.2.0-27-generic (x86_64)
    flashrom is free software, get the source code at https://flashrom.org

    flashrom was built with libpci 3.2.1, GCC 4.8.4, little endian
    Command line (5 args): flashrom --programmer internal -w 1303.BIN -V
    Calibrating delay loop... OS timer resolution is 2 usecs, 664M loops
    per second, 10 myus = 12 us, 100 myus = 101 us, 1000 myus = 990 us,
    10000 myus = 13759 us, 8 myus = 2286 us, OK.
    Initializing internal programmer
    No coreboot table found.
    Using Internal DMI decoder.
    DMI string chassis-type: "Desktop"
    DMI string system-manufacturer: "System manufacturer"
    DMI string system-product-name: "System Product Name"
    DMI string system-version: "System Version"
    DMI string baseboard-manufacturer: "ASUSTeK Computer INC."
    DMI string baseboard-product-name: "A8N-SLI"
    DMI string baseboard-version: "1.XX    "
    Found ITE Super I/O, ID 0x8712 on port 0x2e
    Found chipset "NVIDIA CK804" with PCI ID 10de:0050.
    Enabling flash write... OK.
    The following protocols are supported: Non-SPI.
    Probing for AMD Am29F010A/B, 128 kB: probe_jedec_common: id1 0x00, id2
    0xff, id1 parity violation, id1 is normal flash content, id2 is normal
    flash content
    Probing for AMD Am29F002(N)BB, 256 kB: probe_jedec_common: id1 0xd5,
    id2 0x8f, id1 is normal flash content, id2 is normal flash content
    Probing for AMD Am29F002(N)BT, 256 kB: probe_jedec_common: id1 0xd5,
    id2 0x8f, id1 is normal flash content, id2 is normal flash content
    Probing for AMD Am29F016D, 2048 kB: probe_jedec_common: id1 0xff, id2
    0xff, id1 parity violation, id1 is normal flash content, id2 is normal
    flash content
    Probing for AMD Am29F040B, 512 kB: probe_jedec_common: id1 0x21, id2
    0x30, id1 parity violation, id1 is normal flash content, id2 is normal
    flash content
    Probing for AMD Am29F080B, 1024 kB: probe_jedec_common: id1 0xff, id2
    0xff, id1 parity violation, id1 is normal flash content, id2 is normal
    flash content
    Probing for AMD Am29LV001BB, 128 kB: probe_jedec_common: id1 0x00, id2
    0xff, id1 parity violation, id1 is normal flash content, id2 is normal
    flash content
    Probing for AMD Am29LV001BT, 128 kB: probe_jedec_common: id1 0x00, id2
    0xff, id1 parity violation, id1 is normal flash content, id2 is normal
    flash content
    Probing for AMD Am29LV002BB, 256 kB: probe_jedec_common: id1 0xd5, id2
    0x8f, id1 is normal flash content, id2 is normal flash content
    Probing for AMD Am29LV002BT, 256 kB: probe_jedec_common: id1 0xd5, id2
    0x8f, id1 is normal flash content, id2 is normal flash content
    Probing for AMD Am29LV004BB, 512 kB: probe_jedec_common: id1 0x21, id2
    0x30, id1 parity violation, id1 is normal flash content, id2 is normal
    flash content
    Probing for AMD Am29LV004BT, 512 kB: probe_jedec_common: id1 0x21, id2
    0x30, id1 parity violation, id1 is normal flash content, id2 is normal
    flash content
    Probing for AMD Am29LV008BB, 1024 kB: probe_jedec_common: id1 0xff,
    id2 0xff, id1 parity violation, id1 is normal flash content, id2 is
    normal flash content
    Probing for AMD Am29LV008BT, 1024 kB: probe_jedec_common: id1 0xff,
    id2 0xff, id1 parity violation, id1 is normal flash content, id2 is
    normal flash content
    Probing for AMD Am29LV040B, 512 kB: probe_jedec_common: id1 0x21, id2
    0x30, id1 parity violation, id1 is normal flash content, id2 is normal
    flash content
    Probing for AMD Am29LV081B, 1024 kB: probe_jedec_common: id1 0xff, id2
    0xff, id1 parity violation, id1 is normal flash content, id2 is normal
    flash content
    Probing for AMIC A29002B, 256 kB: probe_jedec_common: id1 0xd5, id2
    0x8f, id1 is normal flash content, id2 is normal flash content
    Probing for AMIC A29002T, 256 kB: probe_jedec_common: id1 0xd5, id2
    0x8f, id1 is normal flash content, id2 is normal flash content
    Probing for AMIC A29040B, 512 kB: probe_jedec_common: id1 0x21, id2
    0x30, id1 parity violation, id1 is normal flash content, id2 is normal
    flash content
    Probing for AMIC A49LF040A, 512 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for Atmel AT29C512, 64 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for Atmel AT29C010A, 128 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for Atmel AT29C020, 256 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for Atmel AT29C040A, 512 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for Atmel AT49BV512, 64 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for Atmel AT49F002(N), 256 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for Atmel AT49F002(N)T, 256 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for Atmel AT49(H)F010, 128 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for Atmel AT49F020, 256 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for Atmel AT49F040, 512 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for Atmel AT49F080, 1024 kB: probe_jedec_common: id1 0xff, id2
    0xff, id1 parity violation, id1 is normal flash content, id2 is normal
    flash content
    Probing for Atmel AT49F080T, 1024 kB: probe_jedec_common: id1 0xff,
    id2 0xff, id1 parity violation, id1 is normal flash content, id2 is
    normal flash content
    Probing for Atmel AT49LH002, 256 kB: probe_82802ab: id1 0xd5, id2
    0x8f, id1 is normal flash content, id2 is normal flash content
    Probing for Atmel AT49LH00B4, 512 kB: probe_82802ab: id1 0x21, id2
    0x30, id1 parity violation, id1 is normal flash content, id2 is normal
    flash content
    Probing for Atmel AT49LH004, 512 kB: probe_82802ab: id1 0x21, id2
    0x30, id1 parity violation, id1 is normal flash content, id2 is normal
    flash content
    Probing for Catalyst CAT28F512, 64 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for Bright BM29F040, 512 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for ESMT F49B002UA, 256 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for Eon EN29F010, 128 kB: probe_jedec_common: id1 0x00, id2
    0xff, id1 parity violation, id1 is normal flash content, id2 is normal
    flash content
    Probing for Eon EN29F002(A)(N)B, 256 kB: probe_jedec_common: id1 0xd5,
    id2 0x8f, id1 is normal flash content, id2 is normal flash content
    Probing for Eon EN29F002(A)(N)T, 256 kB: probe_jedec_common: id1 0xd5,
    id2 0x8f, id1 is normal flash content, id2 is normal flash content
    Probing for Eon EN29LV040(A), 512 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for Eon EN29LV640B, 8192 kB: probe_en29lv640b: id1 0xffff, id2 0x00ff
    Probing for Eon EN29GL064(A)B, 8192 kB: probe_jedec_29gl: man_id 0xff,
    dev_id 0xffffff, man_id parity violation, man_id seems to be normal
    flash content, dev_id seems to be normal flash content
    Probing for Eon EN29GL064(A)T, 8192 kB: probe_jedec_29gl: man_id 0xff,
    dev_id 0xffffff, man_id parity violation, man_id seems to be normal
    flash content, dev_id seems to be normal flash content
    Probing for Eon EN29GL064H/L, 8192 kB: probe_jedec_29gl: man_id 0xff,
    dev_id 0xffffff, man_id parity violation, man_id seems to be normal
    flash content, dev_id seems to be normal flash content
    Probing for Eon EN29GL128, 16384 kB: probe_jedec_29gl: man_id 0xff,
    dev_id 0xffffff, man_id parity violation, man_id seems to be normal
    flash content, dev_id seems to be normal flash content
    Probing for Fujitsu MBM29F004BC, 512 kB: probe_jedec_common: id1 0x21,
    id2 0x30, id1 parity violation, id1 is normal flash content, id2 is
    normal flash content
    Probing for Fujitsu MBM29F004TC, 512 kB: probe_jedec_common: id1 0x21,
    id2 0x30, id1 parity violation, id1 is normal flash content, id2 is
    normal flash content
    Probing for Fujitsu MBM29F400BC, 512 kB: probe_jedec_common: id1 0x21,
    id2 0x2d, id1 parity violation, id1 is normal flash content, id2 is
    normal flash content
    Probing for Fujitsu MBM29F400TC, 512 kB: probe_jedec_common: id1 0x21,
    id2 0x2d, id1 parity violation, id1 is normal flash content, id2 is
    normal flash content
    Probing for Fujitsu MBM29LV160BE, 2048 kB: probe_jedec_common: id1
    0xff, id2 0xff, id1 parity violation, id1 is normal flash content, id2
    is normal flash content
    Probing for Fujitsu MBM29LV160TE, 2048 kB: probe_jedec_common: id1
    0xff, id2 0xff, id1 parity violation, id1 is normal flash content, id2
    is normal flash content
    Probing for Hyundai HY29F002T, 256 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for Hyundai HY29F002B, 256 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for Hyundai HY29F040A, 512 kB: probe_jedec_common: id1 0x21,
    id2 0x30, id1 parity violation, id1 is normal flash content, id2 is
    normal flash content
    Probing for Intel 28F001BN/BX-B, 128 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for Intel 28F001BN/BX-T, 128 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for Intel 28F002BC/BL/BV/BX-T, 256 kB: probe_82802ab: id1
    0xd5, id2 0x8f, id1 is normal flash content, id2 is normal flash
    content
    Probing for Intel 28F008S3/S5/SC, 512 kB: probe_82802ab: id1 0x21, id2
    0x30, id1 parity violation, id1 is normal flash content, id2 is normal
    flash content
    Probing for Intel 28F004B5/BE/BV/BX-B, 512 kB: probe_82802ab: id1
    0x21, id2 0x30, id1 parity violation, id1 is normal flash content, id2
    is normal flash content
    Probing for Intel 28F004B5/BE/BV/BX-T, 512 kB: probe_82802ab: id1
    0x21, id2 0x30, id1 parity violation, id1 is normal flash content, id2
    is normal flash content
    Probing for Intel 28F400BV/BX/CE/CV-B, 512 kB: probe_82802ab: id1
    0x21, id2 0x2d, id1 parity violation, id1 is normal flash content, id2
    is normal flash content
    Probing for Intel 28F400BV/BX/CE/CV-T, 512 kB: probe_82802ab: id1
    0x21, id2 0x2d, id1 parity violation, id1 is normal flash content, id2
    is normal flash content
    Probing for Intel 82802AB, 512 kB: probe_82802ab: id1 0x21, id2 0x30,
    id1 parity violation, id1 is normal flash content, id2 is normal flash
    content
    Probing for Intel 82802AC, 1024 kB: probe_82802ab: id1 0xff, id2 0xff,
    id1 parity violation, id1 is normal flash content, id2 is normal flash
    content
    Probing for ISSI IS29GL064B, 8192 kB: probe_jedec_29gl: man_id 0xff,
    dev_id 0xffffff, man_id parity violation, man_id seems to be normal
    flash content, dev_id seems to be normal flash content
    Probing for ISSI IS29GL064T, 8192 kB: probe_jedec_29gl: man_id 0xff,
    dev_id 0xffffff, man_id parity violation, man_id seems to be normal
    flash content, dev_id seems to be normal flash content
    Probing for ISSI IS29GL064H/L, 8192 kB: probe_jedec_29gl: man_id 0xff,
    dev_id 0xffffff, man_id parity violation, man_id seems to be normal
    flash content, dev_id seems to be normal flash content
    Probing for ISSI IS29GL128H/L, 16384 kB: probe_jedec_29gl: man_id
    0xff, dev_id 0xffffff, man_id parity violation, man_id seems to be
    normal flash content, dev_id seems to be normal flash content
    Probing for Macronix MX29F001B, 128 kB: probe_jedec_common: id1 0x00,
    id2 0xff, id1 parity violation, id1 is normal flash content, id2 is
    normal flash content
    Probing for Macronix MX29F001T, 128 kB: probe_jedec_common: id1 0x00,
    id2 0xff, id1 parity violation, id1 is normal flash content, id2 is
    normal flash content
    Probing for Macronix MX29F002(N)B, 256 kB: probe_jedec_common: id1
    0xd5, id2 0x8f, id1 is normal flash content, id2 is normal flash
    content
    Probing for Macronix MX29F002(N)T, 256 kB: probe_jedec_common: id1
    0xd5, id2 0x8f, id1 is normal flash content, id2 is normal flash
    content
    Probing for Macronix MX29F022(N)B, 256 kB: probe_jedec_common: id1
    0xd5, id2 0x8f, id1 is normal flash content, id2 is normal flash
    content
    Probing for Macronix MX29F022(N)T, 256 kB: probe_jedec_common: id1
    0xd5, id2 0x8f, id1 is normal flash content, id2 is normal flash
    content
    Probing for Macronix MX29F040, 512 kB: probe_jedec_common: id1 0x21,
    id2 0x30, id1 parity violation, id1 is normal flash content, id2 is
    normal flash content
    Probing for Macronix MX29GL320EB, 4096 kB: probe_jedec_29gl: man_id
    0xff, dev_id 0xffffff, man_id parity violation, man_id seems to be
    normal flash content, dev_id seems to be normal flash content
    Probing for Macronix MX29GL320ET, 4096 kB: probe_jedec_29gl: man_id
    0xff, dev_id 0xffffff, man_id parity violation, man_id seems to be
    normal flash content, dev_id seems to be normal flash content
    Probing for Macronix MX29GL320EH/L, 4096 kB: probe_jedec_29gl: man_id
    0xff, dev_id 0xffffff, man_id parity violation, man_id seems to be
    normal flash content, dev_id seems to be normal flash content
    Probing for Macronix MX29GL640EB, 8192 kB: probe_jedec_29gl: man_id
    0xff, dev_id 0xffffff, man_id parity violation, man_id seems to be
    normal flash content, dev_id seems to be normal flash content
    Probing for Macronix MX29GL640ET, 8192 kB: probe_jedec_29gl: man_id
    0xff, dev_id 0xffffff, man_id parity violation, man_id seems to be
    normal flash content, dev_id seems to be normal flash content
    Probing for Macronix MX29GL640EH/L, 8192 kB: probe_jedec_29gl: man_id
    0xff, dev_id 0xffffff, man_id parity violation, man_id seems to be
    normal flash content, dev_id seems to be normal flash content
    Probing for Macronix MX29GL128F, 16384 kB: probe_jedec_29gl: man_id
    0xff, dev_id 0xffffff, man_id parity violation, man_id seems to be
    normal flash content, dev_id seems to be normal flash content
    Probing for Macronix MX29LV040, 512 kB: probe_jedec_common: id1 0x21,
    id2 0x30, id1 parity violation, id1 is normal flash content, id2 is
    normal flash content
    Probing for MoselVitelic V29C51000B, 64 kB: probe_jedec_common: id1
    0xbf, id2 0x60
    Probing for MoselVitelic V29C51000T, 64 kB: probe_jedec_common: id1
    0xbf, id2 0x60
    Probing for MoselVitelic V29C51400B, 512 kB: probe_jedec_common: id1
    0xbf, id2 0x60
    Probing for MoselVitelic V29C51400T, 512 kB: probe_jedec_common: id1
    0xbf, id2 0x60
    Probing for MoselVitelic V29LC51000, 64 kB: probe_jedec_common: id1
    0xbf, id2 0x60
    Probing for MoselVitelic V29LC51001, 128 kB: probe_jedec_common: id1
    0xbf, id2 0x60
    Probing for MoselVitelic V29LC51002, 256 kB: probe_jedec_common: id1
    0xbf, id2 0x60
    Probing for PMC Pm29F002T, 256 kB: Chip lacks correct probe timing
    information, using default 10ms/40us. probe_jedec_common: id1 0xd5,
    id2 0x8f, id1 is normal flash content, id2 is normal flash content
    Probing for PMC Pm29F002B, 256 kB: Chip lacks correct probe timing
    information, using default 10ms/40us. probe_jedec_common: id1 0xd5,
    id2 0x8f, id1 is normal flash content, id2 is normal flash content
    Probing for PMC Pm39LV010, 128 kB: probe_jedec_common: id1 0x00, id2
    0xff, id1 parity violation, id1 is normal flash content, id2 is normal
    flash content
    Probing for PMC Pm39LV020, 256 kB: probe_jedec_common: id1 0xd5, id2
    0x8f, id1 is normal flash content, id2 is normal flash content
    Probing for PMC Pm39LV040, 512 kB: probe_jedec_common: id1 0x21, id2
    0x30, id1 parity violation, id1 is normal flash content, id2 is normal
    flash content
    Probing for PMC Pm39LV512, 64 kB: probe_jedec_common: id1 0xea, id2
    0x05, id1 is normal flash content, id2 is normal flash content
    Probing for PMC Pm49FL002, 256 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for PMC Pm49FL004, 512 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for Sharp LH28F008BJT-BTLZ1, 1024 kB: probe_82802ab: id1 0xff,
    id2 0xff, id1 parity violation, id1 is normal flash content, id2 is
    normal flash content
    Probing for Sharp LHF00L04, 1024 kB: probe_82802ab: id1 0xff, id2
    0xff, id1 parity violation, id1 is normal flash content, id2 is normal
    flash content
    Probing for SST SST28SF040A, 512 kB: probe_82802ab: id1 0x21, id2
    0x30, id1 parity violation, id1 is normal flash content, id2 is normal
    flash content
    Probing for SST SST29EE010, 128 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for SST SST29LE010, 128 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for SST SST29EE020A, 256 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for SST SST29LE020, 256 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for SST SST39SF512, 64 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for SST SST39SF010A, 128 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for SST SST39SF020A, 256 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for SST SST39SF040, 512 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for SST SST39VF512, 64 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for SST SST39VF010, 128 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for SST SST39VF020, 256 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for SST SST39VF040, 512 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for SST SST39VF080, 1024 kB: probe_jedec_common: id1 0xff, id2
    0xff, id1 parity violation, id1 is normal flash content, id2 is normal
    flash content
    Probing for SST SST49LF002A/B, 256 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for SST SST49LF003A/B, 384 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for SST SST49LF004A/B, 512 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Found SST flash chip "SST49LF004A/B" (512 kB, FWH) mapped at physical
    address 0x00000000fff80000.
    Lock status for 0x000000 (size 0x010000) is 00, full access
    Lock status for 0x010000 (size 0x010000) is 00, full access
    Lock status for 0x020000 (size 0x010000) is 00, full access
    Lock status for 0x030000 (size 0x010000) is 00, full access
    Lock status for 0x040000 (size 0x010000) is 00, full access
    Lock status for 0x050000 (size 0x010000) is 00, full access
    Lock status for 0x060000 (size 0x010000) is 00, full access
    Lock status for 0x070000 (size 0x010000) is 00, full access
    Probing for SST SST49LF004C, 512 kB: probe_82802ab: id1 0x21, id2
    0x30, id1 parity violation, id1 is normal flash content, id2 is normal
    flash content
    Probing for SST SST49LF008A, 1024 kB: probe_jedec_common: id1 0xff,
    id2 0xff, id1 parity violation, id1 is normal flash content, id2 is
    normal flash content
    Probing for SST SST49LF008C, 1024 kB: probe_82802ab: id1 0xff, id2
    0xff, id1 parity violation, id1 is normal flash content, id2 is normal
    flash content
    Probing for SST SST49LF016C, 2048 kB: probe_82802ab: id1 0xff, id2
    0xff, id1 parity violation, id1 is normal flash content, id2 is normal
    flash content
    Probing for SST SST49LF020, 256 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for SST SST49LF020A, 256 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for SST SST49LF040, 512 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for SST SST49LF040B, 512 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for SST SST49LF080A, 1024 kB: Chip lacks correct probe timing
    information, using default 10ms/40us. probe_jedec_common: id1 0xff,
    id2 0xff, id1 parity violation, id1 is normal flash content, id2 is
    normal flash content
    Probing for SST SST49LF160C, 2048 kB: probe_82802ab: id1 0xff, id2
    0xff, id1 parity violation, id1 is normal flash content, id2 is normal
    flash content
    Probing for ST M29F002B, 256 kB: probe_jedec_common: id1 0xd5, id2
    0x8f, id1 is normal flash content, id2 is normal flash content
    Probing for ST M29F002T/NT, 256 kB: probe_jedec_common: id1 0xd5, id2
    0x8f, id1 is normal flash content, id2 is normal flash content
    Probing for ST M29F040B, 512 kB: probe_jedec_common: id1 0x21, id2
    0x30, id1 parity violation, id1 is normal flash content, id2 is normal
    flash content
    Probing for ST M29F400BB, 512 kB: probe_jedec_common: id1 0x21, id2
    0x2d, id1 parity violation, id1 is normal flash content, id2 is normal
    flash content
    Probing for ST M29F400BT, 512 kB: probe_jedec_common: id1 0x21, id2
    0x2d, id1 parity violation, id1 is normal flash content, id2 is normal
    flash content
    Probing for ST M29W010B, 128 kB: probe_jedec_common: id1 0x00, id2
    0xff, id1 parity violation, id1 is normal flash content, id2 is normal
    flash content
    Probing for ST M29W040B, 512 kB: probe_jedec_common: id1 0x21, id2
    0x30, id1 parity violation, id1 is normal flash content, id2 is normal
    flash content
    Probing for ST M29W512B, 64 kB: probe_jedec_common: id1 0xea, id2
    0x05, id1 is normal flash content, id2 is normal flash content
    Probing for ST M50FLW040A, 512 kB: probe_82802ab: id1 0x21, id2 0x30,
    id1 parity violation, id1 is normal flash content, id2 is normal flash
    content
    Probing for ST M50FLW040B, 512 kB: probe_82802ab: id1 0x21, id2 0x30,
    id1 parity violation, id1 is normal flash content, id2 is normal flash
    content
    Probing for ST M50FLW080A, 1024 kB: probe_82802ab: id1 0xff, id2 0xff,
    id1 parity violation, id1 is normal flash content, id2 is normal flash
    content
    Probing for ST M50FLW080B, 1024 kB: probe_82802ab: id1 0xff, id2 0xff,
    id1 parity violation, id1 is normal flash content, id2 is normal flash
    content
    Probing for ST M50FW002, 256 kB: probe_82802ab: id1 0xd5, id2 0x8f,
    id1 is normal flash content, id2 is normal flash content
    Probing for ST M50FW016, 2048 kB: probe_82802ab: id1 0xff, id2 0xff,
    id1 parity violation, id1 is normal flash content, id2 is normal flash
    content
    Probing for ST M50FW040, 512 kB: probe_82802ab: id1 0x21, id2 0x30,
    id1 parity violation, id1 is normal flash content, id2 is normal flash
    content
    Probing for ST M50FW080, 1024 kB: probe_82802ab: id1 0xff, id2 0xff,
    id1 parity violation, id1 is normal flash content, id2 is normal flash
    content
    Probing for ST M50LPW080, 1024 kB: probe_82802ab: id1 0xff, id2 0xff,
    id1 parity violation, id1 is normal flash content, id2 is normal flash
    content
    Probing for ST M50LPW116, 2048 kB: probe_82802ab: id1 0xff, id2 0xff,
    id1 parity violation, id1 is normal flash content, id2 is normal flash
    content
    Probing for SyncMOS/MoselVitelic {F,S,V}29C51001B, 128 kB:
    probe_jedec_common: id1 0xbf, id2 0x60
    Probing for SyncMOS/MoselVitelic {F,S,V}29C51001T, 128 kB:
    probe_jedec_common: id1 0xbf, id2 0x60
    Probing for SyncMOS/MoselVitelic {F,S,V}29C51002B, 256 kB:
    probe_jedec_common: id1 0xbf, id2 0x60
    Probing for SyncMOS/MoselVitelic {F,S,V}29C51002T, 256 kB:
    probe_jedec_common: id1 0xbf, id2 0x60
    Probing for SyncMOS/MoselVitelic {F,S,V}29C51004B, 512 kB:
    probe_jedec_common: id1 0xbf, id2 0x60
    Probing for SyncMOS/MoselVitelic {F,S,V}29C51004T, 512 kB:
    probe_jedec_common: id1 0xbf, id2 0x60
    Probing for SyncMOS/MoselVitelic {S,V}29C31004B, 512 kB:
    probe_jedec_common: id1 0xbf, id2 0x60
    Probing for SyncMOS/MoselVitelic {S,V}29C31004T, 512 kB:
    probe_jedec_common: id1 0xbf, id2 0x60
    Probing for TI TMS29F002RB, 256 kB: probe_jedec_common: id1 0xd5, id2
    0x8f, id1 is normal flash content, id2 is normal flash content
    Probing for TI TMS29F002RT, 256 kB: probe_jedec_common: id1 0xd5, id2
    0x8f, id1 is normal flash content, id2 is normal flash content
    Probing for Winbond W29C512A/W29EE512, 64 kB: probe_jedec_common: id1
    0xbf, id2 0x60
    Probing for Winbond W29C010(M)/W29C011A/W29EE011/W29EE012-old, 128 kB:
    Old Winbond W29* probe method disabled because the probing sequence
    puts the AMIC A49LF040A in a funky state. Use 'flashrom -c
    W29C010(M)/W29C011A/W29EE011/W29EE012-old' if you have a board with
    such a chip.
    Probing for Winbond W29C010(M)/W29C011A/W29EE011/W29EE012, 128 kB:
    probe_jedec_common: id1 0xbf, id2 0x60
    Probing for Winbond W29C020(C)/W29C022, 256 kB: probe_jedec_common:
    id1 0xbf, id2 0x60
    Probing for Winbond W29C040/P, 512 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for Winbond W29GL032CB, 4096 kB: probe_jedec_29gl: man_id
    0xff, dev_id 0xffffff, man_id parity violation, man_id seems to be
    normal flash content, dev_id seems to be normal flash content
    Probing for Winbond W29GL032CT, 4096 kB: probe_jedec_29gl: man_id
    0xff, dev_id 0xffffff, man_id parity violation, man_id seems to be
    normal flash content, dev_id seems to be normal flash content
    Probing for Winbond W29GL032CH/L, 4096 kB: probe_jedec_29gl: man_id
    0xff, dev_id 0xffffff, man_id parity violation, man_id seems to be
    normal flash content, dev_id seems to be normal flash content
    Probing for Winbond W29GL064CB, 8192 kB: probe_jedec_29gl: man_id
    0xff, dev_id 0xffffff, man_id parity violation, man_id seems to be
    normal flash content, dev_id seems to be normal flash content
    Probing for Winbond W29GL064CT, 8192 kB: probe_jedec_29gl: man_id
    0xff, dev_id 0xffffff, man_id parity violation, man_id seems to be
    normal flash content, dev_id seems to be normal flash content
    Probing for Winbond W29GL064CH/L, 8192 kB: probe_jedec_29gl: man_id
    0xff, dev_id 0xffffff, man_id parity violation, man_id seems to be
    normal flash content, dev_id seems to be normal flash content
    Probing for Winbond W29GL128C, 16384 kB: probe_jedec_29gl: man_id
    0xff, dev_id 0xffffff, man_id parity violation, man_id seems to be
    normal flash content, dev_id seems to be normal flash content
    Probing for Winbond W39F010, 128 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for Winbond W39L010, 128 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for Winbond W39L020, 256 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for Winbond W39L040, 512 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for Winbond W39V040A, 512 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for Winbond W39V040B, 512 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for Winbond W39V040C, 512 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for Winbond W39V040FA, 512 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for Winbond W39V040FB, 512 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for Winbond W39V040FC, 512 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for Winbond W39V080A, 1024 kB: probe_jedec_common: id1 0xff,
    id2 0xff, id1 parity violation, id1 is normal flash content, id2 is
    normal flash content
    Probing for Winbond W49F002U/N, 256 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for Winbond W49F020, 256 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for Winbond W49V002A, 256 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for Winbond W49V002FA, 256 kB: probe_jedec_common: id1 0xbf, id2 0x60
    Probing for Winbond W39V080FA, 1024 kB: probe_jedec_common: id1 0xff,
    id2 0xff, id1 parity violation, id1 is normal flash content, id2 is
    normal flash content
    Probing for Winbond W39V080FA (dual mode), 512 kB: probe_jedec_common:
    id1 0xbf, id2 0x60
    Found SST flash chip "SST49LF004A/B" (512 kB, FWH).
    Lock status for 0x000000 (size 0x010000) is 00, full access
    Lock status for 0x010000 (size 0x010000) is 00, full access
    Lock status for 0x020000 (size 0x010000) is 00, full access
    Lock status for 0x030000 (size 0x010000) is 00, full access
    Lock status for 0x040000 (size 0x010000) is 00, full access
    Lock status for 0x050000 (size 0x010000) is 00, full access
    Lock status for 0x060000 (size 0x010000) is 00, full access
    Lock status for 0x070000 (size 0x010000) is 00, full access
    Flash image seems to be a legacy BIOS. Disabling coreboot-related checks.
    Reading old flash chip contents... done.
    Erasing and writing flash chip... Trying erase function 0...
    0x000000-0x000fff:EW, 0x001000-0x001fff:EW, 0x002000-0x002fff:EW,
    0x003000-0x003fff:EW, 0x004000-0x004fff:EW, 0x005000-0x005fff:EW,
    0x006000-0x006fff:EW, 0x007000-0x007fff:EW, 0x008000-0x008fff:EW,
    0x009000-0x009fff:EW, 0x00a000-0x00afff:EW, 0x00b000-0x00bfff:EW,
    0x00c000-0x00cfff:EW, 0x00d000-0x00dfff:EW, 0x00e000-0x00efff:EW,
    0x00f000-0x00ffff:EW, 0x010000-0x010fff:EW, 0x011000-0x011fff:EW,
    0x012000-0x012fff:EW, 0x013000-0x013fff:EW, 0x014000-0x014fff:EW,
    0x015000-0x015fff:EW, 0x016000-0x016fff:EW, 0x017000-0x017fff:EW,
    0x018000-0x018fff:EW, 0x019000-0x019fff:EW, 0x01a000-0x01afff:EW,
    0x01b000-0x01bfff:EW, 0x01c000-0x01cfff:EW, 0x01d000-0x01dfff:EW,
    0x01e000-0x01efff:EW, 0x01f000-0x01ffff:EW, 0x020000-0x020fff:EW,
    0x021000-0x021fff:EW, 0x022000-0x022fff:EW, 0x023000-0x023fff:EW,
    0x024000-0x024fff:EW, 0x025000-0x025fff:EW, 0x026000-0x026fff:EW,
    0x027000-0x027fff:EW, 0x028000-0x028fff:EW, 0x029000-0x029fff:EW,
    0x02a000-0x02afff:EW, 0x02b000-0x02bfff:EW, 0x02c000-0x02cfff:EW,
    0x02d000-0x02dfff:EW, 0x02e000-0x02efff:EW, 0x02f000-0x02ffff:EW,
    0x030000-0x030fff:EW, 0x031000-0x031fff:EW, 0x032000-0x032fff:EW,
    0x033000-0x033fff:EW, 0x034000-0x034fff:EW, 0x035000-0x035fff:EW,
    0x036000-0x036fff:EW, 0x037000-0x037fff:EW, 0x038000-0x038fff:EW,
    0x039000-0x039fff:EW, 0x03a000-0x03afff:EW, 0x03b000-0x03bfff:EW,
    0x03c000-0x03cfff:EW, 0x03d000-0x03dfff:EW, 0x03e000-0x03efff:EW,
    0x03f000-0x03ffff:EW, 0x040000-0x040fff:EW, 0x041000-0x041fff:EW,
    0x042000-0x042fff:EW, 0x043000-0x043fff:EW, 0x044000-0x044fff:EW,
    0x045000-0x045fff:EW, 0x046000-0x046fff:EW, 0x047000-0x047fff:EW,
    0x048000-0x048fff:EW, 0x049000-0x049fff:EW, 0x04a000-0x04afff:EW,
    0x04b000-0x04bfff:EW, 0x04c000-0x04cfff:EW, 0x04d000-0x04dfff:EW,
    0x04e000-0x04efff:EW, 0x04f000-0x04ffff:EW, 0x050000-0x050fff:EW,
    0x051000-0x051fff:EW, 0x052000-0x052fff:EW, 0x053000-0x053fff:EW,
    0x054000-0x054fff:EW, 0x055000-0x055fff:EW, 0x056000-0x056fff:EW,
    0x057000-0x057fff:EW, 0x058000-0x058fff:EW, 0x059000-0x059fff:EW,
    0x05a000-0x05afff:W, 0x05b000-0x05bfff:W, 0x05c000-0x05cfff:W,
    0x05d000-0x05dfff:W, 0x05e000-0x05efff:W, 0x05f000-0x05ffff:W,
    0x060000-0x060fff:EW, 0x061000-0x061fff:W, 0x062000-0x062fff:W,
    0x063000-0x063fff:W, 0x064000-0x064fff:W, 0x065000-0x065fff:W,
    0x066000-0x066fff:W, 0x067000-0x067fff:EW, 0x068000-0x068fff:EW,
    0x069000-0x069fff:EW, 0x06a000-0x06afff:EW, 0x06b000-0x06bfff:EW,
    0x06c000-0x06cfff:S, 0x06d000-0x06dfff:S, 0x06e000-0x06efff:EW,
    0x06f000-0x06ffff:S, 0x070000-0x070fff:EW, 0x071000-0x071fff:EW,
    0x072000-0x072fff:S, 0x073000-0x073fff:S, 0x074000-0x074fff:EW,
    0x075000-0x075fff:EW, 0x076000-0x076fff:EW, 0x077000-0x077fff:EW,
    0x078000-0x078fff:EW, 0x079000-0x079fff:EW, 0x07a000-0x07afff:EW,
    0x07b000-0x07bfff:S, 0x07c000-0x07cfff:S, 0x07d000-0x07dfff:S,
    0x07e000-0x07efff:EW, 0x07f000-0x07ffff:EW
    Erase/write done.
    Verifying flash... VERIFIED.
    Restoring PCI config space for 00:01:0 reg 0x6d


Далее зашел в биос и сбросил все параметры на значения по умолчанию.

Все прошилось, но мне к сожалению это никак не помогло.


Supported hardware:  
https://www.flashrom.org/Supported_hardware

Supported programmers:  
https://www.flashrom.org/Supported_programmers

Использованные материалы:  
http://forum.ubuntu.ru/index.php?topic=120108.0
