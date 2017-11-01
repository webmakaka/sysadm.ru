---
layout: page
title: Cisco Router 1941 в домашней сети Билайн
permalink: /devices/cisco/routers/1941/beeline/
---


# Cisco Router 1941 в домашней сети Билайн

Поменялся способ подключения для клиентов. Я не буду удалять написанное ранее, может когда еще что-нибудь понадобится.

Сейчас можно получить публичный ip адрес на свой интерфейс от провайдера без всяких настроек l2tp. Достаточно авторизоваться на сайте и подождать какое-то количество времени, чтобы получить нужный ip адрес. По крайней мере так теперь работает в Москве.

Появляется картинка с сообщением.

    https://login.beeline.ru/


Но насколько я понимаю, она просто висит, чтобы пользователь ничего не делал. После авторизации на форме, по DHCP на интерфейс роутера придет нужный IP адрес, а прийти он должен как раз в пределать этого времени. Точнее, за определенное время выданый ранее IP адрес устареет, и ему предложат взять другой, правильный. После эта операция будет повторяться, только присваиваться будет один и тот же IP адрес. В моем случае мой статический адрес.


<br/>

<div align="center">
    <img src="//files.sysadm.ru/img/devices/cisco/routers/1941/beeline/login_beeline.png" border="0" alt="beeline login">
</div>    

<br/>



    #show ip interface brief
    Interface                  IP-Address      OK? Method Status                Protocol
    Embedded-Service-Engine0/0 unassigned      YES NVRAM  administratively down down    
    GigabitEthernet0/0         95.31.31.8      YES DHCP   up                    up      
    GigabitEthernet0/1         192.168.1.1     YES NVRAM  up                    up      
    NVI0                       unassigned      YES unset  administratively down down    
    Virtual-PPP1               unassigned      YES IPCP   administratively down down    


Virtual-PPP1 - отключен  
GigabitEthernet0/0 - присвоен мой статический IP. Ранее статический IP адрес назначался Virtual-PPP1, а GigabitEthernet0/0 - какой-то IP от DHCP сервера билайна.

Как было настроено раньше, можно посмотреть в архивных записях.

Пришлось обратиться к настройкам, после того как интернет стал постоянно отваливаться. А то так и не обратил бы внимания.
Проблема оказалась на стороне провайдера. Техники, что-то сделали с кабелем на крыше, после этого работает достаточно стабильно.


Чтобы настроить Cisco роутер, достаточно повторить теже шаги, что из материалов в архиве, но настраивать L2TP, апгрейдить лицензию, я думаю уже не нужно.

Я же пока настраивать с нуля роутер не собираюсь, поэтому апгрейдить работающие инструкции на лету, не хочу.


<br/>

### Настройка роутера для работы в домашней сети Билайн

<a href="/devices/cisco/routers/1941/beeline-not-works/">Cisco Router 1941 если отвалился интернет в локальной сети Билайн</a>

<a href="/devices/cisco/routers/1941/beeline-port-forwarding/">Cisco Router 1941 Проброс порта в локальную сеть Билайн</a> (WebServer)


<br/>

### Архив:


<a href="/devices/cisco/routers/1941/info/">Техническая информация на сайте провайдера по подключеню</a>  

<a href="/devices/cisco/routers/1941/beeline-l2tp-first-problem/">Cisco Router 1941 проблемы с подключением к сети Билайн по l2tp</a> (Необходима лицензия data license имеем ipbase)

<a href="/devices/cisco/routers/1941/beeline-l2tp/">Cisco Router 1941 настройка работы интернета в Билайн по l2tp</a>



<br/>

### Другие конфиги (может когда и кому понадобятся). Можно добавить свои.


<a href="/devices/cisco/routers/1941/beeline-general/">Настройка маршрутизаторов Cisco для работы в сети Корбина)</a>

<!--
<a href="https://gist.github.com/sysadm-ru/034b841e24a0412c70ba">Cisco 871W (version 12.4)</a>


<a href="https://gist.github.com/sysadm-ru/218432aa3bc80161637d">Cisco Router 1811 (version 12.4)</a>


<a href="https://gist.github.com/sysadm-ru/ced2e08bfac0ef55aa96"> Cisco 1841 (version 12.4)</a>

<a href="https://gist.github.com/sysadm-ru/0c9889febf255569dc21">Cisco Router 1921/к9 (version 15.3)</a>

<a href="https://gist.github.com/sysadm-ru/cbdef23bdf6b0b3249b93ca524b67a86#file-cisco-nme-xd-48es-2s-p">Cisco NME-XD-48ES-2S-P</a> -->
