---
layout: page
title: IPTABLES NAT
description: IPTABLES NAT
keywords: IPTABLES NAT
permalink: /desktop/linux/networks/nat/centos/nat/
---

# IPTABLES NAT

Есть несколько способов добавить правила в таблицы.<br/>
Здесь представлен самый простой способ и он полностью удаляет настройки по умолчанию.

Если вы перестартуете iptables, вернутся значения по умолчанию.

Не советую бездумно выполнять этот скрипт на важных серверах. Особенное, если они не расположены в шаговой доступности.

<!--

<pre>

# cp /etc/sysconfig/iptables /etc/sysconfig/iptables.bkp

</pre>

-->

<!--

# $EXT_IP - внешний, реальный IP-адрес шлюза
# $INT_IP - внутренний IP-адрес шлюза, в локальной сети
# $LAN_IP - внутренний IP-адрес сервера, предоставляющего службы внешнему миру
# $SRV_PORT - порт службы. Для веб-сервера равен 80, для SMTP - 25 и т.д.
# EXT_IF=eth0 - внешний интерфейс шлюза. Именно ему присвоен сетевой адрес $EXT_IP
# INT_IF=eth1 - внутренний интерфейс шлюза, с адресом $INT_IP

-->

<pre class="blue_border">
<strong class="userinput"># <code>vi iptables</code></strong>
</pre>

<pre class="white_border">
<code>#!/bin/bash

# Включаем форвардинг пакетов
echo 1 > /proc/sys/net/ipv4/ip_forward

# Очистить таблицы
iptables -F
iptables -t nat -F
iptables -t mangle -F

# Удалить цепочку пользователя
iptables -X


# Разрешаем трафик на loopback-интерфейсе
iptables -A INPUT -i lo -j ACCEPT

# Включаем форвардинг пакетов (NAT)
iptables -t nat -A POSTROUTING -s 192.168.2.0/24 -o eth0  -j MASQUERADE

# Разрешаем доступ из внутренней сети наружу
iptables -A FORWARD -i eth1 -o eth0 -j ACCEPT

# Разрешаем ответы из внешней сети
iptables -A FORWARD -i eth0 -m state --state ESTABLISHED,RELATED -j ACCEPT

# Запрещаем доступ снаружи во внутреннюю сеть
iptables -A FORWARD -i eth0 -o eth1 -j REJECT</code>
</pre>

<pre class="blue_border">
    <strong class="userinput"># <code>chmod +x iptables</code></strong>
</pre>

<pre class="blue_border">
    <strong class="userinput"># <code>./iptables</code></strong>
</pre>

<br/>

### Опубликовать сервер во внешней сети

-- При обращении к серверу 192.168.1.34 по протоколу tcp на порт 8000 интерфейса eth0, перенаправлять запросы на сервер 192.168.2.5 порт 8000

<pre class="blue_border">
    <strong class="userinput"># <code>iptables -t nat -A PREROUTING --dst 192.168.1.34 -i eth0 -p tcp --dport 8000 -j DNAT --to-destination 192.168.2.5:8000</code></strong>
</pre>

<pre class="blue_border">
    <strong class="userinput"># <code>iptables -I FORWARD 1 -i eth0 -o eth1 -d 192.168.2.5 -p tcp -m tcp --dport 8000 -j ACCEPT</code></strong>
</pre>

<!--

<pre>

<br/><br/>

// Полезные команды

# service iptables reload
# service iptables save

# iptables-save > rules.txt
# iptables-restore < rules.txt


<br/><br/>
Почитать:<br/>
http://www.it-simple.ru/?p=2250
<br/><br/>
iptables: Проброс RDP наружу или форвард портов между локальными сетями<br/>
http://mnorin.com/iptables-probros-rdp-naruzhu-ili-forvard-portov-m.html<br/>
http://redhat-club.org/2011/%D0%BD%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0-nat-%D0%B2-rhel-centos-fedora<br/>
http://habrahabr.ru/post/205460/


 Дебаг правил iptables в CentOS 6.2


Дебажить правила для iptables удобно с помощью логирования, при котором логи файрволла пишутся в отдельный файл. Для этого надо в iptables добавить директивы логирования пакетов и  настроить систему логирования CentOS на запись этих сообщений в отдельный файл.

1. Добавляем директивы логирования пакетов в iptables
Допустим базовый конфигурационный файл /etc/sysconfig/iptables выглядит так

# Firewall configuration written by system-config-firewall
# Manual customization of this file is not recommended.
*filter
:INPUT ACCEPT [0:0]
:FORWARD ACCEPT [0:0]
:OUTPUT ACCEPT [0:0]
-A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
-A INPUT -p icmp -j ACCEPT
-A INPUT -i lo -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT
-A INPUT -j REJECT --reject-with icmp-host-prohibited
-A FORWARD -j REJECT --reject-with icmp-host-prohibited.

Последними двумя строками отклоняются все INPUT и FORWARD пакеты, которые не подошли правилам выше.
ПЕРЕД этими строчками добавляем следующие директивы

#************* for debug
-A INPUT -j LOG --log-level DEBUG --log-prefix "[FW INPUT]:"
-A FORWARD -j LOG --log-level DEBUG --log-prefix "[FW FORWARD]:"

То есть мы указали в iptables, что все пакеты, не прошедшие по правилам выше директив логирования должны логироваться.

2. Перезапускаем iptables
# service iptables restart

3. Настраиваем rsyslog для записи логов iptables в отдельный файл
Для того, чтобы записать именно сообщения iptables, отфильтруем их на основании уровня логирования DEBUG.
Редактируем vi /etc/rsyslog.conf
# vi /etc/rsyslog.conf
Добавляем в секцию #### RULES ####
kern.debug /var/log/iptables.log
Что означает писать все сообщения ядра уровня логирования DEBUG в файл /var/log/iptables.log

4. Перезапускаем rsyslog
# service rsyslog restart

Теперь все, что не подошло имеющимся правилам и будет отброшено, будет записываться в файл /var/log/iptables.log. По этим данным легко понять какие пакеты мы упустили при написании правил.

http://www.olegsmith.com/2011/12/iptables-centos-62.html

</pre>

-->
