---
layout: page
title: KVM на Centos 6
permalink: /linux/servers/virtual/kvm/
---

# KVM на Centos 6


<pre>

<strong>Инсталляция и настройка</strong>


-- Проверка, поддерживает ли процессор виртуализацию
# egrep '(vmx|svm)' /proc/cpuinfo



<strong>Настройка сети и моста на хост-сервере</strong>

# yum install -y bridge-utils


# cat > /etc/sysconfig/network-scripts/ifcfg-eth0 << EOF
DEVICE=eth0
TYPE=Ethernet
ONBOOT=yes
BRIDGE=br0
NM_CONTROLLED=no

EOF


# cat > /etc/sysconfig/network-scripts/ifcfg-br0 << EOF
DEVICE=br0
TYPE=Bridge
ONBOOT=yes
BOOTPROTO=static
IPADDR=192.168.1.11
NETWORK=192.168.1.0
NETMASK=255.255.255.0
GATEWAY=192.168.1.1
DELAY=0
NM_CONTROLLED=no

EOF


# service network restart



# brctl show br0
bridge name	bridge id		STP enabled	interfaces
br0		8000.00270e02d38b	no		eth0



-- Делаем настройки в iptables, чтобы трафик виртуалок «ходил» через соединение типа bridge

# iptables -I FORWARD -m physdev --physdev-is-bridged -j ACCEPT
# service iptables save
# service iptables restart


-- Опционально: можно улучшить быстродействие соединения bridge, поправив настройки в /etc/sysctl.conf


# cat >> /etc/sysctl.conf << EOF
net.bridge.bridge-nf-call-ip6tables = 0
net.bridge.bridge-nf-call-iptables = 0
net.bridge.bridge-nf-call-arptables = 0
EOF


# sysctl -p /etc/sysctl.conf


========================================

<strong>Инсталляция ПО для виртуализации</strong>

# yum install -y \
kvm \
libvirt \
python-virtinst

# chkconfig --level 345 libvirtd on
# service libvirtd restart



<strong>Создание хранилища для виртуальных машин (Storage Pool)</strong>


-- список физических дисков
# fdisk -l

-- создаем разделы
# fdisk /dev/sdb
# fdisk /dev/sdc

-- форматируем разделы
# mkfs.ext4 /dev/sdc1
# mkfs.ext4 /dev/sdd1


-- Создаем точку монтирования нашего жесткого диска для файлов виртуальных машин:
# mkdir /guest_images
# chmod 700 /guest_images
# ls -la /guest_images
total 8  
drwx------   2 root root 4096 Фев 11 06:40 .
dr-xr-xr-x. 24 root root 4096 Фев 11 06:40 ..


-- Смонтируем раздел /dev/sdb1 в /guest_images
# mount -t ext4 /dev/sdb1 /guest_images


-- Отредактируем файл /etc/fstab для того, чтобы при перезагрузке хост-сервера раздел с ВМ монтировался автоматически

# vi /etc/fstab
/dev/sdb1               /guest_images           ext4    defaults        1 1


--Сохраняем файл и продолжаем создание хранилища:

# virsh pool-define-as guest_images_dir dir - - - - "/guest_images"
Pool guest_images_dir defined

# virsh pool-list --all
Name                 State      Autostart
-----------------------------------------
guest_images_dir     inactive   no        


# virsh pool-build guest_images_dir


-- Запускаем хранилище
# virsh pool-start guest_images_dir


# virsh pool-list --all
Name                 State      Autostart
-----------------------------------------
guest_images_dir     active     no        



-- Добавляем в автозагрузку:
# virsh pool-autostart guest_images_dir



# virsh pool-list --all
Name                 State      Autostart
-----------------------------------------
guest_images_dir     active     yes       



# virsh pool-info guest_images_dir
Name:           guest_images_dir
UUID:           e9d6f5a9-d9c2-127e-677f-b250284128c2
State:          running
Persistent:     yes
Autostart:      yes
Capacity:       450,58 GiB
Allocation:     15,38 GiB
Available:      435,19 GiB

<!--
# service libvirtd reload
-->
==========================================


<strong>Установка новой виртуальной машины</strong>


При установке ОС Windows не увидит виртуального жесткого диска, поэтому надо подгрузить дополнительный виртуальный cdrom с драйверами /iso/virtio-win.iso — расположение файла ISO с драйверами виртуального диска. Взять можно отсюда.

http://alt.fedoraproject.org/pub/alt/virtio-win/latest/images/bin/


# mkdir -p /iso
# cd /iso
# wget http://alt.fedoraproject.org/pub/alt/virtio-win/latest/images/bin/virtio-win-0.1-74.iso


# virt-install --connect qemu:///system --arch=x86_64 \
-n vm_win2k8_x64 -r 1024 --vcpus=1 \
--disk pool=guest_images_dir,size=50,bus=virtio,cache=none \
-c /iso/Windows2008R2EN.ISO \
--graphics vnc,listen=0.0.0.0,keymap=ru,password=some.password.here \
--noautoconsole --os-type windows \
--os-variant win2k8 \
--network=bridge:br0,model=e1000 \
--disk path=/iso/virtio-win-0.1-74.iso,device=cdrom,perms=ro


Создал еще 1 SSH сессию к виртуальной машине


# netstat -nltp | grep q
tcp        0      0 0.0.0.0:5900                0.0.0.0:*                   LISTEN      12520/qemu-kvm      
tcp        0      0 192.168.122.1:53            0.0.0.0:*                   LISTEN      1421/dnsmasq        


# virsh vncdisplay vm_win2k8_x64
:0


На Linux Клиенте

$ remmina



<!--



-- Установка CentOS на гостевую ВМ:
# virt-install -n VMName_2 --ram 1024 --arch=x86_64 \
--vcpus=1 --cpu host --check-cpu \
--extra-args="vnc sshd=1 sshpw=secret ip=static reboot=b selinux=0" \
--os-type linux --os-variant=rhel6 --boot cdrom,hd,menu=on \
--disk pool=guest_images_dir,size=50,bus=virtio \
--network=bridge:br0,model=virtio \
--graphics vnc,listen=0.0.0.0,keymap=ru,password=some.password.here \
--noautoconsole --watchdog default,action=reset --virt-type=kvm \
--autostart --location http://mirror.yandex.ru/centos/6.5/os/x86_64/


<strong>Примечание 1:</strong>

VMName_2 — имя новой виртуальной машины
–ram 1024 — кол-во виртуальной памяти
–arch=x86_64 — архитектура ОС виртуалки
–vcpus=1 — кол-во виртуальных процессоров
–os-type linux — тип ОС
–disk pool=guest_images_dir,size=50 — размещение хранилища, размер вирт. диска
–network=bridge:br0

<strong>Примечание 2:</strong>

Если на ВМ нужна «белая сеть», тогда ставим
--network=bridge:br0
Если на ВМ требуется «серая сеть», тогда ставим
--network=bridge:virbr0
В этом случае для ВМ будет присвоен серый IP по DHCP от хост-сервера.
--graphics vnc,listen=0.0.0.0,keymap=ru,password=some.password.here
Тут указываем пароль для подключения к ВМ по vnc


-->


</pre>

![Инсталляция KVM в CENTOS](/img/linux/servers/virtual/kvm/remmina.png "Инсталляция KVM в CENTOS"){: .center-image }

![Инсталляция KVM в CENTOS](/img/linux/servers/virtual/kvm/kvm_01.png "Инсталляция KVM в CENTOS"){: .center-image }

![Инсталляция KVM в CENTOS](/img/linux/servers/virtual/kvm/kvm_02.png "Инсталляция KVM в CENTOS"){: .center-image }

![Инсталляция KVM в CENTOS](/img/linux/servers/virtual/kvm/kvm_03.png "Инсталляция KVM в CENTOS"){: .center-image }


<br/><br/>

<strong>Почитать:</strong><br/><br/>
<strong>Установка и настройка KVM под управлением CentOS 6</strong><br/>
http://habrahabr.ru/post/168791/

<br/><br/>
<strong>Настройка KVM в CentOS 6</strong><br/>
http://www.alsigned.ru/?p=2027
