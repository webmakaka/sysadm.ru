---
layout: page
title: Настройка VPN подключения l2tp на сервере Centos 6.5
description: Настройка VPN подключения l2tp на сервере Centos 6.5 (Домашний интернет Билайн)
keywords: Настройка VPN подключения l2tp на сервере Centos 6.5 (Домашний интернет Билайн)
permalink: /linux/networks/vpn/xl2tp/centos/6/
---

# Настройка VPN подключения l2tp на сервере Centos 6.5 (Домашний интернет Билайн)

<br/>
<strong>Offtopic:</strong>
<br/>

<p style="padding:10px; border:thin solid black;">
Нашел в сети статью о настройке <strong><a href="http://habrahabr.ru/post/128599/">VPN-сервер в стиле how-to (pptpd+mysql+radius) на CentOS6</a></strong>.

<br/>
Если я все правильно понимаю, схема приблизительно похожа на ту, что создают у себя провайдеры. <br/>
Возможно, что такая схема подключения будет интересна админам, у которых пользователи должны работать удаленно.

</p>

<br/>

<div align="center" style="border-width: 4px; padding: 10px; border-style: inset; border-color: red; ">

Многое поменялось. Теперь для подключения к провайдеру не нуно настраивать l2tp. Смотри подробнее <a href="/devices/cisco/routers/1941/beeline/">здесь</a>

<br/>

Удалять не буду, может когда еще понадобится настроить для какой-нибудь другой задачи!

</div>

<br/>

### Начало

Имеем компьютер с 2-я сетевыми картами.

<br/><br/>

eth0 - внешняя сеть<br/>
eth1 - внутренняя сеть

<br/>

<!--
Делаем из него маршрутизатор, который должен работать в локальной сети Билайн. <br/>
-->

Для доступа в локальную сеть, необходимо, чтобы интерфейс eth0 получил по DHCP провайдера ip адрес и локальные маршруты.<br/><br/>
Для доступа в интернет, необходимо авторизоваться на сервере провайдера. Для этого создается виртуальный интерфейс ppp, устанавливается ПО для авторизации.

<strong>UPD 1:</strong> появилось ощущение, что слишком много раз приходится сообщать login пользователя в конфигах! (Не странно, т.к. конфиги собирались из уже готовых примеров в интернете.) По хорошему, нужно это исправить.

<strong>UPD 2:</strong> Брас (или как он там правильно называется) tp.corbina.net,
который верой и правдой служил долгие коды (а тот который рекомендовали сотрудники биллайна не работал как нужно) в последнее время (2014 год.) стал работать
намного хуже. Сидишь копаешься в интернете, бац и разрыв. Бац опять разрыв. Да блять, да каждый вечер разрыв,
после которого маршрутизатор (речь не про тот, что настраивается в статье) сам не поднимается и его нужно ребутить.
В общем, чтобы не было ни единого разрыва, нужно правильно выбрать сервер для авторизации. Поменял брас, стало намного лучше.

<br/>

### Настройка интерфейса, подключенного к биллайн

<br/>

<pre class="blue_border">
<strong class="userinput"># <code>vi /etc/sysconfig/network-scripts/ifcfg-eth0</code></strong>
</pre>

<pre class="white_border">
<code>DEVICE=eth0
BOOTPROTO=dhcp
ONBOOT=yes</code>
</pre>

<pre class="blue_border">
<strong class="userinput"># <code>service network restart</code></strong>
</pre>

<pre class="blue_border">
<strong class="userinput"># <code>host tp.corbina.net</code></strong>
</pre>

<pre class="gray_border">
tp.corbina.net has address 85.21.0.255
</pre>

<pre class="blue_border">
<strong class="userinput"># <code>host tp.internet.beeline.ru</code></strong>
</pre>

<pre class="gray_border">
tp.internet.beeline.ru has address 85.21.0.50
</pre>

<br/>

### Инсталляция необходимого ПО

<br/>

Добавляю EPEL репозиторий, т.к. в стандартном не нашел.

<br/>

    ## EPEL Repository<br/>
    ## RHEL/CentOS 6 64-Bit ##

<br/>

<pre class="blue_border">
<strong class="userinput"># <code>rpm -ivh http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm</code></strong>
</pre>

<!--

<pre class="blue_border">
<strong class="userinput"># <code>cd /tmp</code></strong>
</pre>

<pre class="blue_border">
<strong class="userinput"># <code>wget http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm</code></strong>
</pre>

<pre class="blue_border">
<strong class="userinput"># <code>rpm -ivh epel-release-6-8.noarch.rpm</code></strong>
</pre>

<br/><br/>



<pre class="blue_border">
<strong class="userinput"># <code>yum install -y pptp xl2tp</code></strong>
</pre>

-->

<pre class="blue_border">
<strong class="userinput"># <code>yum install -y xl2tpd</code></strong>
</pre>

<pre class="blue_border">
<strong class="userinput"># <code>chkconfig --level 345 xl2tpd on</code></strong>
</pre>

### Настройка

<br/>

<pre class="blue_border">
<strong class="userinput"># <code>cp /etc/xl2tpd/xl2tpd.conf /etc/xl2tpd/xl2tpd.conf.bkp</code></strong>
</pre>

<pre class="blue_border">
<strong class="userinput"># <code>vi /etc/xl2tpd/xl2tpd.conf</code></strong>
</pre>

<pre class="white_border">
<code>[global]
access control = yes
auth file = /etc/ppp/chap-secrets

[lac beeline]
;lns = tp.internet.beeline.ru
lns = tp.corbina.net
redial = yes
redial timeout = 10
require chap = yes
require authentication = no
<strong>name = имя_пользователя</strong>
ppp debug = yes
pppoptfile = /etc/ppp/options.xl2tpd
require pap = no
autodial = yes
tunnel rws = 8
tx bps = 100000000
flow bit = no</code>
</pre>

<pre class="blue_border">
<strong class="userinput"># <code>cp /etc/ppp/options.xl2tpd /etc/ppp/options.xl2tpd.bkp</code></strong>
</pre>

<pre class="blue_border">
<strong class="userinput"># <code>vi /etc/ppp/options.xl2tpd</code></strong>
</pre>

<pre class="white_border">
<code>asyncmap 0000
<strong>name имя_пользователя</strong>
remotename L2TP
ipparam beeline
connect /bin/true
mru 1460
mtu 1460
nodeflate
nobsdcomp
persist
maxfail 0
nopcomp
noaccomp
noauth
novj
novjccomp
noipx
nomp
refuse-eap
refuse-pap
unit 0</code>
</pre>

<pre class="blue_border">
<strong class="userinput"># <code>cp /etc/ppp/chap-secrets /etc/ppp/chap-secrets.bkp</code></strong>
</pre>

<pre class="blue_border">
<strong class="userinput"># <code>vi /etc/ppp/chap-secrets</code></strong>
</pre>

<pre class="white_border">
<code>имя_пользователя * пароль *</code>
</pre>

<pre class="blue_border">
<strong class="userinput"># <code>service xl2tpd start</code></strong>
</pre>

**Ждем получения ip адреса (по разному от сразу до 3-х минут).**

<pre class="blue_border">
<strong class="userinput"># <code>less /var/log/messages</code></strong>
</pre>

<pre class="gray_border">
<code>Jan  5 21:00:07 gateway pppd[7958]: local  IP address 95.31.31.8
Jan  5 21:00:07 gateway pppd[7958]: remote IP address 85.21.0.243</code>
</pre>

**Появляется интерфейс ppp0**

<pre class="blue_border">
<strong class="userinput"># <code>ifconfig ppp</code></strong>
</pre>

<pre class="gray_border">
<code>ppp0      Link encap:Point-to-Point Protocol
          inet addr:95.31.31.8  P-t-P:78.107.1.154  Mask:255.255.255.255
          UP POINTOPOINT RUNNING NOARP MULTICAST  MTU:1460  Metric:1
          RX packets:3 errors:0 dropped:0 overruns:0 frame:0
          TX packets:87540191 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:3
          RX bytes:30 (30.0 b)  TX bytes:39058515021 (36.3 GiB)</code>
</pre>

**Таблица маршрутизации выглядит следующим образом**

<pre class="blue_border">
<strong class="userinput"># <code>route -n</code></strong>
</pre>

<pre class="gray_border">
<code>Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
bras409-l0.msk. *               255.255.255.255 UH    0      0        0 ppp0
192.168.2.0     *               255.255.255.0   U     0      0        0 eth1
10.111.0.0      *               255.255.248.0   U     0      0        0 eth0
link-local      *               255.255.0.0     U     1002   0        0 eth0
link-local      *               255.255.0.0     U     1003   0        0 eth1
default         10.111.0.1      0.0.0.0         UG    0      0        0 eth0</code>
</pre>

<!--


# vi /etc/sysconfig/network-scripts/route-eth0

10.0.0.0/8 via 10.111.0.1
62.205.179.146 via 10.111.0.1
85.21.79.0/24 via 10.111.0.1
85.21.90.0/24 via 10.111.0.1
85.21.52.198 via 10.111.0.1
85.21.52.254 via 10.111.0.1
85.21.138.3 via 10.111.0.1
83.102.146.96/27 via 10.111.0.1
83.102.237.231 via 10.111.0.1
195.14.50.1 via 10.111.0.1
195.14.50.3 via 10.111.0.1
195.14.50.16 via 10.111.0.1
195.14.50.26 via 10.111.0.1
85.21.192.3 via 10.111.0.1
213.234.192.8 via 10.111.0.1
85.21.192.5 via 10.111.0.1

-->

**Получаю данные о присвоенном Default Gateway**

<pre class="blue_border">
<strong class="userinput"># <code>eth0_gw=`/sbin/route -n | awk '/^0.0.0.0/ {print $2}'`</code></strong>
</pre>

<pre class="blue_border">
<strong class="userinput"># <code>echo $eth0_gw</code></strong>
</pre>

<pre class="gray_border">
<code>10.111.0.1</code>
</pre>

**Получаю данные о присвоенном нам сервере авторизации L2TP**

<pre class="blue_border">
<strong class="userinput"># <code>vpn_server=`/sbin/route -n | awk '/ppp0/ {print $1}'`</code></strong>
</pre>

<pre class="blue_border">
<strong class="userinput"># <code>echo $vpn_server</code></strong>
</pre>

<pre class="gray_border">
<code>85.21.0.243</code>
</pre>

<br/><br/>

<pre>

Возможно, что предварительно нужно прописать маршруты до билайновских dns и до l2pt сервера.
Последний раз когда настраивал, прокатило и без них.

route add -host 85.21.192.3 gw $eth0_gw
route add -host 213.234.192.8 gw $eth0_gw
route add -host 85.21.192.5 gw $eth0_gw
</pre>

<br/>

<pre class="blue_border">
<strong class="userinput"># <code>route del default</code></strong>
</pre>

<pre class="blue_border">
<strong class="userinput"># <code>route add default dev ppp0</code></strong>
</pre>

<pre class="blue_border">
<strong class="userinput"># <code>route add -host $vpn_server gw $eth0_gw</code></strong>
</pre>

Вот что получилось. (Поднят ppp интерфейс и по умолчанию пакеты отправляются ему).<br/>
При этом маршрут до L2TP сервера должен быть прописан. (Иначе, сервер не знает где ему авторизовываться,
интерфейс ppp падает, все перестает работать).

<pre class="blue_border">
<strong class="userinput"># <code>route -n</code></strong>
</pre>

<pre class="gray_border">
<code>Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
<strong>85.21.0.243     10.111.0.1      255.255.255.255 UGH   0      0        0 eth0</strong>
85.21.0.243     0.0.0.0         255.255.255.255 UH    0      0        0 ppp0
192.168.2.0     0.0.0.0         255.255.255.0   U     0      0        0 eth1
10.111.0.0      0.0.0.0         255.255.248.0   U     0      0        0 eth0
169.254.0.0     0.0.0.0         255.255.0.0     U     1002   0        0 eth0
169.254.0.0     0.0.0.0         255.255.0.0     U     1003   0        0 eth1
<strong>0.0.0.0         0.0.0.0         0.0.0.0         U     0      0        0 ppp0</strong></code>
</pre>

**Пинг пошел**

<pre class="blue_border">
<strong class="userinput"># <code>ping ya.ru</code></strong>
</pre>

<pre class="gray_border">
<code>PING ya.ru (93.158.134.3) 56(84) bytes of data.
64 bytes from www.yandex.ru (93.158.134.3): icmp_seq=1 ttl=55 time=3.91 ms
64 bytes from www.yandex.ru (93.158.134.3): icmp_seq=2 ttl=55 time=4.00 ms</code>
</pre>

<br/><br/>
Остается только настроить <a href="/linux/networks/nat/centos/nat/">NAT</a>.
<br/><br/>

---

Моя задача потренироваться была выполнена.<br/>
Автоматизировать процесс изменения маршрутов в зависимости от того, поднят ли ppp интерфейс не пытался.<br/>
В списке "почитать" первая ссылка, там все это описывается.<br/>

<pre>

Мое предположение, в билайне куча серверов авторизации.
Некоторые из них выдают маршруты, некоторые нет.

Если нужно добавить статические маршруты, можно это сделать в файле:

# vi /etc/sysconfig/network-scripts/route-eth0

10.0.0.0/8 via 10.111.0.1
62.205.179.146 via 10.111.0.1
85.21.79.0/24 via 10.111.0.1
85.21.90.0/24 via 10.111.0.1
85.21.52.198 via 10.111.0.1
85.21.52.254 via 10.111.0.1
85.21.138.3 via 10.111.0.1
83.102.146.96/27 via 10.111.0.1
83.102.237.231 via 10.111.0.1
195.14.50.1 via 10.111.0.1
195.14.50.3 via 10.111.0.1
195.14.50.16 via 10.111.0.1
195.14.50.26 via 10.111.0.1
85.21.192.3 via 10.111.0.1
213.234.192.8 via 10.111.0.1
85.21.192.5 via 10.111.0.1
</pre>

<strong>Если кто напишет, как доделать, исправить, сделать лучше, чтобы было все по феншую, пишите я внесу исправления.</strong>

<!--


??????
# route add default dev ppp0



# route add -host 85.21.0.243 gw 10.111.0.1



===============================================
===============================================
===============================================
   ЗАРАБОТАВШАЯ ТАБЛИЦА МАРШРУТИЗАЦИИ
===============================================
===============================================
===============================================

# route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
83.102.237.231  10.111.0.1      255.255.255.255 UGH   0      0        0 eth0
85.21.138.3     10.111.0.1      255.255.255.255 UGH   0      0        0 eth0
195.14.50.3     10.111.0.1      255.255.255.255 UGH   0      0        0 eth0
85.21.192.3     10.111.0.1      255.255.255.255 UGH   0      0        0 eth0
195.14.50.16    10.111.0.1      255.255.255.255 UGH   0      0        0 eth0
195.14.50.1     10.111.0.1      255.255.255.255 UGH   0      0        0 eth0
85.21.0.243     10.111.0.1      255.255.255.255 UGH   0      0        0 eth0
85.21.0.243     0.0.0.0         255.255.255.255 UH    0      0        0 ppp0
85.21.192.5     10.111.0.1      255.255.255.255 UGH   0      0        0 eth0
85.21.52.254    10.111.0.1      255.255.255.255 UGH   0      0        0 eth0
195.14.50.26    10.111.0.1      255.255.255.255 UGH   0      0        0 eth0
213.234.192.8   10.111.0.1      255.255.255.255 UGH   0      0        0 eth0
62.205.179.146  10.111.0.1      255.255.255.255 UGH   0      0        0 eth0
85.21.52.198    10.111.0.1      255.255.255.255 UGH   0      0        0 eth0
83.102.146.96   10.111.0.1      255.255.255.224 UG    0      0        0 eth0
192.168.2.0     0.0.0.0         255.255.255.0   U     0      0        0 eth1
85.21.90.0      10.111.0.1      255.255.255.0   UG    0      0        0 eth0
85.21.79.0      10.111.0.1      255.255.255.0   UG    0      0        0 eth0
10.111.0.0      0.0.0.0         255.255.248.0   U     0      0        0 eth0
169.254.0.0     0.0.0.0         255.255.0.0     U     1002   0        0 eth0
169.254.0.0     0.0.0.0         255.255.0.0     U     1003   0        0 eth1
10.0.0.0        10.111.0.1      255.0.0.0       UG    0      0        0 eth0
0.0.0.0         0.0.0.0         0.0.0.0         U     0      0        0 ppp0



===============================================
===============================================
===============================================



обычно
   маршруты к dns серверам и l2tp серверу прописывают вручну.


   Вроде все хорошо. Но если касаться теории, то нужно сказать, что обычно
   маршруты к dns серверам и l2tp серверу прописывают вручну. Зачем это
   надо?! Дело в том, что пока мы сидим в локалке у нас имеется дефолтный
   шлюз, через который у нас все соединяется. Как только мы поднимем
   тунель наш дефолтный шлюз должен смениться на шлюз выданного ip(как
   правило интерфейса ppp0). Причем эта замена - принудительная. Так вот в
   момент переключения шлюза пакеты не знаю где искать сервера(ни dns, ни
   l2tp). Именно по этой причине адреса необходимо прописать вручную


             # route add -host  213.234.192.8 gw 10.21.50.1
             # route add -host  85.21.192.3 gw 10.21.50.1
             # route add -host  85.21.0.241 gw 10.21.50.1


   Эти маршруты необходимо прописывать всегда перед созданием vpn тунеля.

   Но кроме добавления маршрутов для серверов, нам необходимо менять
   маршрут по умолчанию. При создании тонеля мы должны устанавливать в
   качестве шлюза ip адрес, который будет выдан серверном и который будет
   использоваться интерфейсом ppp0. А после разрыва соединения должны
   будем вернуть шлюз который б��л. В моем случае это 10.21.50.1. Вот
   пример удаления локального шлюза и установка шлюза через ip адрес
   интерфейса ppp0.

             # route del default
             # route add default dev ppp0


   Вроде все понятно. Для наглядности приведу блок схему алгоритма.

   dns1 - 213.234.192.8
   dns2 - 85.21.192.3
   l2tp - 85.21.0.241
   шлюз - 10.21.50.1





Jan  5 17:08:38 gateway pppd[2278]: local  IP address 95.31.31.8
Jan  5 17:08:38 gateway pppd[2278]: remote IP address 85.21.0.255
Jan  5 17:09:04 gateway pppd[1522]: Failed to open /dev/pts/2: No such file or d
irectory
Jan  5 17:09:34 gateway pppd[1522]: Failed to open /dev/pts/2: No such file or d
irectory
Jan  5 17:10:04 gateway pppd[1522]: Failed to open /dev/pts/2: No such file or directory
Jan  5 17:10:34 gateway pppd[1522]: Failed to open /dev/pts/2: No such file or directory
Jan  5 17:11:04 gateway pppd[1522]: Failed to open /dev/pts/2: No such file or directory

-->

Если на клиенте не все сайты открываются, а на сервере все ок. (Например yandex и google открываются, а множество других нет, хотя и нормально пингуются). Проблемы скорее всего в настройкам MTU.

Следует создать правило в iptables

<pre class="blue_border">
<strong class="userinput"># <code>iptables -I FORWARD -p tcp --tcp-flags SYN,RST SYN -j TCPMSS --clamp-mss-to-pmtu</code></strong>
</pre>

<br/><br/>

Подробности здесь:<br/>
http://www.opennet.ru/base/net/pppoe_mtu.txt.html

Возможно, что здесь лучше описано решение точно такой же задачи:<br/>
http://linuxdata.ru/questions/q48.html

Резервная копия:<br/>
https://archive.today/jSvQG

<strong>Почитать:</strong>
<br/><br/>

http://ru.posix.wikia.com/wiki/PPTP#.D0.9F.D1.80.D0.BE.D1.81.D1.82.D0.BE_.D0.BE_.D1.81.D0.BB.D0.BE.D0.B6.D0.BD.D0.BE.D0.BC._VPN_.D0.B4.D0.BB.D1.8F_.D0.BD.D0.B0.D1.87.D0.B8.D0.BD.D0.B0.D1.8E.D1.89.D0.B8.D1.85.
<br/><br/>
Резервная копия на всякий случай:<br/>
http://archive.is/0G2dc<br/><br/>
http://homenet.beeline.ru/index.php?showtopic=58937<br/>
http://homenet.beeline.ru/index.php?showtopic=289658<br/>
http://homenet.beeline.ru/index.php?showtopic=58937<br/>
http://homenet.beeline.ru/index.php?showforum=629<br/>
http://archlinux.org.ru/forum/topic/7000/<br/>
http://www.linux.org.ru/forum/general/9315021<br/>
http://linuxoid.in/%D0%9D%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0_%D0%BF%D0%BE%D0%B4%D0%BA%D0%BB%D1%8E%D1%87%D0%B5%D0%BD%D0%B8%D1%8F_%D0%BA_ISP_%22Corbina%22_%28%D0%9C%D0%BE%D1%81%D0%BA%D0%B2%D0%B0%29#.D0.9D.D0.B0.D1.81.D1.82.D1.80.D0.BE.D0.B9.D0.BA.D0.B0_.D1.80.D0.BE.D1.83.D1.82.D0.B8.D0.BD.D0.B3.D0.B0

Нужно посмотреть для Centos 7 (пока не смотрел)<br/>
http://homenet.beeline.ru/index.php?showtopic=320882
