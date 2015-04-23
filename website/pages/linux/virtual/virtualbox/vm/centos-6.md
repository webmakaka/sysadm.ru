---
layout: page
title: Создание виртуальной машины VirtualBox с Centos 6.X. в командной строке linux
permalink: /linux/virtual/virtualbox/vm/centos-6/
---

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

Мне понадобился адаптер с NAT.




Наберите команду;

    $ VBoxManage list bridgedifs

Обратите внимание на значение:

Name:                eth0

Я использую eth0 как основной физический интерфейс, который будут использовать виртуальные машины в качестве моста.

Подключаю к виртуальной машине 3 виртуальных сетевых “Intel® 82540EM Gigabit Ethernet Controller”, работающих как bridget (3 адаптера нужные в случае необходимости установить RAC):

$ VBoxManage modifyvm ${vm} \
--nictype1 82540EM \
--nic1 bridged \
--bridgeadapter1 eth0



=========================================


<strong>Определяем порядок устройств, с которых будет произведена попытка стартовать систему:</strong>


$ VBoxManage modifyvm ${vm} \
--boot1 disk \
--boot2 dvd


<strong>Определяем каталог для снапшотов:</strong>



$ VBoxManage modifyvm ${vm} \
--snapshotfolder ${VM_HOME}/${vm}/snapshots



<strong>Предоставим возможность подключения к машине по RDP:</strong>


$ VBoxManage modifyvm ${vm} \
--vrde on \
--vrdemulticon on \
--vrdeauthtype null \
--vrdeaddress 192.168.1.5 \
--vrdeport 3389

Здесь мы указываем:

--vrdeaddress - ip адрес машины, на которой установлен vitrualbox
--vrdeauthtype null - аутентификация не требуется.
--vrdemulticon on - разрешено множественное подключение к виртуальным машинам.
--vrdeport порт к которому можно будет подключиться при старте виртуальной машины.


<strong>Устанавливаем двунаправленный режим работы буфера обмена:</strong>


$ VBoxManage modifyvm ${vm} --clipboard bidirectional

(Вроде drag and drop пока не работает в bidirectional)

$ VBoxManage modifyvm ${vm} --draganddrop hosttoguest


<strong>Показать результат созданной виртаульной машины:</strong>


$ VBoxManage showvminfo ${vm}

ВИРТУАЛЬНАЯ МАШИНА ГОТОВА ДЛЯ ИНСТАЛЛЯЦИИ ОПЕРАЦИОННОЙ СИСТЕМЫ


<strong>Стартуем виртуальную машину с возможность подключения по RDP:</strong>



$ VBoxHeadless --startvm ${vm} &

Или

$ vboxmanage startvm ${vm} -type headless &

(В centos лучше запускать с nohup  $ nohup vboxmanage startvm ${vm} -type headless &, иначе при закрытии сессии, вируальная машина будет убита).


<strong>Посмотреть стартованные виртуальные машины можно командой:</strong>


$ vboxmanage list runningvms



<strong>Подключиться к виртуальной машине:</strong>

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


<img src="vm_oracle_linux.png" alt="Oracle Linux" />



<strong>Извлечь DVD диск можно командой</strong>

$ VBoxManage modifyvm ${vm} --dvd none

Reset виртуальной машины

$ VBoxManage controlvm ${vm} reset

Выключить виртуальную машину

$ VBoxManage controlvm ${vm} poweroff


</pre>
