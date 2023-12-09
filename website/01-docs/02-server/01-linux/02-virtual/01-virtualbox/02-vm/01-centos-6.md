---
layout: page
title: Создание виртуальной машины VirtualBox с Centos 6.X. в командной строке linux
description: Создание виртуальной машины VirtualBox с Centos 6.X. в командной строке linux
keywords: Создание виртуальной машины VirtualBox с Centos 6.X. в командной строке linux
permalink: /server/linux/virtual/virtualbox/vm/centos-6/
---

# Создание виртуальной машины VirtualBox с Centos 6.X. в командной строке linux

Имеем ноутбук, подключенный по WI-FI.
На него нужно установить виртуальную машину с Centos.
Доступа к управлению WI-FI точкой доступа нет.

Задаем переменную с именем создаваемой виртуальной машины, чтобы в дальнейшем лишний раз не подставлять данное значение в команды.

    # su - vmadm

<br/>

    $ vboxmanage --version
    4.3.28r100309

<br/>

    $ vm=vm_serv_1_centos_66

    $ echo $vm
    vm_serv_1_centos_66

Создаем каталоги для виртуальной машины и для snapshots

    $ mkdir -p ${VM_HOME}/${vm}/snapshots

<br/>

### Создание и регистрация виртуальной машины:

Узнать список поддерживаемых операционных систем

    $ VBoxManage list ostypes

    $ VBoxManage createvm \
    --name ${vm} \
    --ostype RedHat_64 \
    --basefolder ${VM_HOME}/${vm} \
    --register

<br/>

**Должно появиться сообщение:**

    Virtual machine 'vm_serv_1_centos_66' is created and registered.

### Устанавливаем планку оперативной памяти:

    $ VBoxManage modifyvm ${vm} --memory 1024

### Подключаю видеокарту на 32 MB:

    $ VBoxManage modifyvm ${vm} --vram 32

### Снимаю sound карту, вытаскиваем дисковвод:

    $ VBoxManage modifyvm ${vm} --floppy disabled --audio none

<br/>

### Подключаю контроллер жестких дисков (SATA):

```
$ VBoxManage storagectl ${vm} \
    --name "SATA Controller" \
    --add sata \
    --controller IntelAhci
```

**Если понадобится удалить:**

    $ VBoxManage storagectl ${vm} --name "SATA Controller" --remove

### Создание и подключение жестких дисков:

Создаю виртуальные жесткие диски. Размер (size), рекомендуется задавать согласно имеющихся ресурсов. Иначе возможны проблемы и крах виртуальной машины):

    $ cd ${VM_HOME}/${vm}/${vm}

    $ VBoxManage createhd \
    --filename ${vm}_dsk1.vdi \
    --size 40960 \
    --format VDI \
    --variant Standard

### Подключаю диски к SAS контроллеру:

```
$ VBoxManage storageattach ${vm} \
--storagectl "SATA Controller" \
--port 0 \
--type hdd \
--medium ${vm}_dsk1.vdi
```

<br/>

### Подключаю IDE контроллер к которому будет позднее подключен DVD-ROM:

    $ VBoxManage storagectl ${vm} \
    --add ide \
    --name "IDE Controller"

### Подключаю к IDE контроллеру DVD образ инсталлируемой операционной системы:

    $ VBoxManage storageattach ${vm} \
    --storagectl "IDE Controller" \
    --port 0 \
    --device 0 \
    --type dvddrive \
    --medium  ~/ISO/CentOS-6.6-x86_64-bin-DVD1to2/CentOS-6.6-x86_64-bin-DVD1.iso

<br/>

### Определяем порядок устройств, с которых будет произведена попытка стартовать систему:

    $ VBoxManage modifyvm ${vm} \
    --boot1 disk \
    --boot2 dvd

### Определяем каталог для снапшотов:

    $ VBoxManage modifyvm ${vm} \
    --snapshotfolder ${VM_HOME}/${vm}/snapshots

<br/>

### Подключение сетевых интерфейсов:

Мне понадобился 1 адаптер с NAT, чтобы компьютер мог выходить в интернет.

    $ VBoxManage modifyvm ${vm} \
    --nictype1 82540EM \
    --nic1 nat

И понадобился 1 hostonly адаптер для подключения к виртуальной машине по SSH с хоста.

    $ VBoxManage modifyvm ${vm} \
    --nictype2 82540EM \
    --nic2 hostonly \
    --hostonlyadapter2 vboxnet0

<br/>

ifconfig на хост машине должен выводить vboxnet0.

vboxnet0 - виртуальный адаптер хостовой машины.

Если виртуального адаптера нет, нуно его самостоятельно создать.

    $ VBoxManage hostonlyif create

<br/>

    $ vboxmanage list hostonlyifs
    Name:            vboxnet0
    GUID:            786f6276-656e-4074-8000-0a0027000000
    DHCP:            Disabled
    IPAddress:       192.168.56.1
    NetworkMask:     255.255.255.0
    IPV6Address:
    IPV6NetworkMaskPrefixLength: 0
    HardwareAddress: 0a:00:27:00:00:00
    MediumType:      Ethernet
    Status:          Down
    VBoxNetworkName: HostInterfaceNetworking-vboxnet0

Присвоить ip, если у него нет.

    $ VBoxManage hostonlyif ipconfig vboxnet0 --ip 192.168.56.1

Стартовать, если нужно

    $ sudo ifconfig vboxnet0 up

Если что-то пошло не так, можно удалить созданный интерфейс командой:

    $ VBoxManage modifyvm ${vm} --nic2 none

<br/>

### Предоставим возможность подключения к машине по RDP:

```
$ VBoxManage modifyvm ${vm} \
    --vrde on \
    --vrdemulticon on \
    --vrdeauthtype null \
    --vrdeaddress 192.168.1.5 \
    --vrdeport 3389
```

<br/>

**Здесь мы указываем:**

--vrdeaddress - ip адрес машины, на которой установлен vitrualbox  
--vrdeauthtype null - аутентификация не требуется.  
--vrdemulticon on - разрешено множественное подключение к виртуальным машинам.  
--vrdeport порт к которому можно будет подключиться при старте виртуальной машины.

<br/>

### Показать результат созданной виртаульной машины:

    $ VBoxManage showvminfo ${vm}

<br/>

## ВИРТУАЛЬНАЯ МАШИНА ГОТОВА ДЛЯ ИНСТАЛЛЯЦИИ ОПЕРАЦИОННОЙ СИСТЕМЫ

<br/>

### Стартуем виртуальную машину с возможностью подключения по RDP:

    $ VBoxHeadless --startvm ${vm} &

Или

    $ vboxmanage startvm ${vm} -type headless &

(В centos лучше запускать с nohup $ nohup vboxmanage startvm ${vm} -type headless &, иначе при закрытии сессии, вируальная машина будет убита).

### Посмотреть стартованные виртуальные машины можно командой:

    $ vboxmanage list runningvms

### Подключиться к виртуальной машине:

Если работаете в linux, подключиться к виртуальной машине можно например, с помощью remmina, rdesktop

    $ sudo apt-get install -y rdesktop

    $ rdesktop \
    -r sound:local \
    -k common  \
    -g  1600x1024 \
    192.168.1.5:3389

Иногда следует использовать и другие ключи:

-f полноэкранный режим. Для выхода из него CTRL+ALT+ENTER  
-k en-ru указать явно раскладку клавиатуры.

rdesktop - всевозможные ключи:

http://manpages.ubuntu.com/manpages/lucid/man1/rdesktop.1.html

В Windows для этого вполне подойдет Remote Desktop Connecton (mstsc.exe)

<br/>

Далее я обычно нажимаю tab [Enter] и дописываю linux text [Enter]

<br/>

### Могут понадобиться следующие команды:

<a href="/server/linux/virtual/virtualbox/commands/">Вот</a>

<br/>

### Постинсталляционные донастройки после инсталляции операционной системы

1. Disable SE если он не нужен.

   # sed -i.bkp -e "s/SELINUX=enforcing/SELINUX=disabled/g" /etc/selinux/config

2. Настройка сетевых интерфейсов.

Разумеется, для начала их нужно активировать и прописать нужные конфиги.

Получилось так, что имеем 2 Интерфейса. Одновременно работает только 1.  
Удаляем ненужный и прописываем нужный. По умолчанию, должен использоваться NAT интерфейс.

    $ route -n

    $ route delete default gw 192.168.56.1 eth1
    $ route add default gw 10.0.2.2 eth0

Чтобы после каждой перезагрузки не делать эти же команды каждый раз. (Не заработало у меня)

    # vi /etc/sysconfig/static-routes

    any -net 10.0.2.0 netmask 255.255.255.0 gw 10.0.2.2 eth0
