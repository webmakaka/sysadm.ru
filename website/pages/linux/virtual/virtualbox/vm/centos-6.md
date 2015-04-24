---
layout: page
title: Создание виртуальной машины VirtualBox с Centos 6.X. в командной строке linux
permalink: /linux/virtual/virtualbox/vm/centos-6/
---

Имеем ноутбук, подключенный по WI-FI.
На него нужно установить виртуальную машину с Centos.
Доступа к управлению WI-FI точкой доступа нет.

___

Задаем переменную с именем создаваемой виртуальной машины, чтобы в дальнейшем лишний раз не подставлять данное значение в команды.

    # su - vmadm

    $ vm=vm_serv_1_centos_66

    $ echo $vm
    vm_serv_1_centos_66

Создаем каталоги для виртуальной машины  и для snapshots

    $ mkdir -p ${VM_HOME}/${vm}/snapshots


### Создание и регистрация виртуальной машины:


Узнать список поддерживаемых операционных систем

    $ VBoxManage list ostypes

    $ VBoxManage createvm \
    --name ${vm} \
    --ostype RedHat_64 \
    --basefolder ${VM_HOME}/${vm} \
    --register

Должно появиться сообщение:

    Virtual machine 'vm_serv_1_centos_66' is created and registered.



### Устанавливаем планку оперативной памяти:


    $ VBoxManage modifyvm ${vm} --memory 1024


### Подключаю видеокарту на 32 MB:


    $ VBoxManage modifyvm ${vm} --vram 32


### Снимаю sound карту, вытаскиваем дисковвод:

    $ VBoxManage modifyvm ${vm} --floppy disabled --audio none


### Подключаю контроллер жестких дисков (SAS):


    $ VBoxManage storagectl ${vm} \
    --add sas \
    --name "SAS Controller"


### Создание и подключение жестких дисков:


Создаю виртуальные жесткие диски. Размер (size), рекомендуется задавать согласно имеющихся ресурсов. Иначе возможны проблемы и крах виртуальной машины):

    $ cd ${VM_HOME}/${vm}/${vm}

    $ VBoxManage createhd \
    --filename ${vm}_dsk1.vdi \
    --size 40960 \
    --format VDI \
    --variant Standard




### Подключаю диски к SAS контроллеру:


    $ VBoxManage storageattach ${vm} \
    --storagectl "SAS Controller" \
    --port 0 \
    --type hdd \
    --medium ${vm}_dsk1.vdi



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


### Подключение сетевых интерфейсов:


Мне понадобился 1 hostonly адаптер для подключения к виртуальной машине по SSH с хоста.

    $ VBoxManage modifyvm ${vm} \
    --nictype1 82540EM \
    --nic1 hostonly \
    --hostonlyadapter1 wlan0


wlan0 - адаптер хостовой машины.  

И 1 адаптер с NAT, чтобы компьютер мог выходить в интернет.

    $ VBoxManage modifyvm ${vm} \
    --nictype2 82540EM \
    --nic2 nat


Если что-то пошло не так, можно удалить созданный интерфейс командой:

    $ VBoxManage modifyvm ${vm} --nic2 none


=========================================


### Определяем порядок устройств, с которых будет произведена попытка стартовать систему:


    $ VBoxManage modifyvm ${vm} \
    --boot1 disk \
    --boot2 dvd


### Определяем каталог для снапшотов:


    $ VBoxManage modifyvm ${vm} \
    --snapshotfolder ${VM_HOME}/${vm}/snapshots



### Предоставим возможность подключения к машине по RDP:


    $ VBoxManage modifyvm ${vm} \
    --vrde on \
    --vrdemulticon on \
    --vrdeauthtype null \
    --vrdeaddress 192.168.35.49 \
    --vrdeport 3389

Здесь мы указываем:  

--vrdeaddress - ip адрес машины, на которой установлен vitrualbox  
--vrdeauthtype null - аутентификация не требуется.  
--vrdemulticon on - разрешено множественное подключение к виртуальным машинам.  
--vrdeport порт к которому можно будет подключиться при старте виртуальной машины.  



### Показать результат созданной виртаульной машины:


    $ VBoxManage showvminfo ${vm}



## ВИРТУАЛЬНАЯ МАШИНА ГОТОВА ДЛЯ ИНСТАЛЛЯЦИИ ОПЕРАЦИОННОЙ СИСТЕМЫ


### Стартуем виртуальную машину с возможностью подключения по RDP:



    $ VBoxHeadless --startvm ${vm} &

Или

    $ vboxmanage startvm ${vm} -type headless &


(В centos лучше запускать с nohup  $ nohup vboxmanage startvm ${vm} -type headless &, иначе при закрытии сессии, вируальная машина будет убита).



### Посмотреть стартованные виртуальные машины можно командой:


    $ vboxmanage list runningvms



### Подключиться к виртуальной машине:

Если работаете в linux, подключиться к виртуальной машине можно например, с помощью remmina, rdesktop

    $ rdesktop \
    -r sound:local \
    -k common  \
    -g  1600x1024 \
    192.168.1.5:3389


Иногда следует использовать и другие ключи:  

-f полноэкранный режим. Для выхода из него CTRL+ALT+ENTER  
-k en-ru  указать явно раскладку клавиатуры.  


rdesktop - всевозможные ключи:  

http://manpages.ubuntu.com/manpages/lucid/man1/rdesktop.1.html  

В Windows для этого вполне подойдет Remote Desktop Connecton (mstsc.exe)


Извлечь DVD диск можно командой

    $ VBoxManage modifyvm ${vm} --dvd none

Reset виртуальной машины

    $ VBoxManage controlvm ${vm} reset

Выключить виртуальную машину

    $ VBoxManage controlvm ${vm} poweroff
