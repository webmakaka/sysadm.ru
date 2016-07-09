---
layout: page
title: Cisco Routers in the Beeline ISP
permalink: /devices/cisco/routers/1941/beeline-general/
---

### Cisco Routers in the Beeline ISP


В этом опусе (надеюсь художественно-техническом) я попытаюсь дать общие рекомендации по настройке ios-маршрутизаторов (роутер) для работы в сети Корбины. Общие потому, что настройка cisco отличается не столько от модели к модели, сколько от версии её операционной системы, т.е. в Корбине может работать любой cisco маршрутизатор, имеющий хотя бы один Ethernet интерфейс и чей ios поддерживает нужные фитчи. Наличие оных вы всегда можете уточнить на сайте производителя, пройдя по ссылке на http://www.cisco.com/go/fn . Для работы по l2tp-протоколу необходимо наличие фитчи - L2TP Client Initiated Tunneling, для работы по pptp необходим ios от 12.2 или современнее. Все настройки выполнены с cli (command line interface), как тоже самое сделать с помощью SDM я не знаю.

Пошаговая инструкция по настройке “cisco из коробки” с cli:
Первичную настройку удобнее делать с помощью консольного кабеля, идущего с роутером в комплекте (голубого или черного цвета), подключив его в порт “console”. Используя любой удобный вам терминальный клиент - PuTTy, SecureCRT, TerraTerm, hyper terminal и др. необходимо войти в командный интерфейс (настроив используемый COM-порт на: Baud rate (TX/RX) is 9600/9600, no parity, 1 stopbits, 8 databits у нового роутера username: cisco, password: cisco). Посмотреть текущую конфигурацию можно командой: show running-config, но там мало интересного и я рекомендовал бы от неё избавиться дав команду: write erase и перезагрузиться: reload (на вопрос System configuration has been modified. Save? [yes/no]: ответьте “no”). После загрузки (откажитесь от предложения мастера начального конфигурирования помочь вам), вы получите роутер с пустой конфигурацией, готовый к настройке.

Базовая настройка:
- настройка локального интерфейса
- vty-интерфейса (если будете использовать)
- включение/отключение нужных/не нужных сервисов
- настройка интерфейса



base_config.txt


    version 12.4
    no service pad
    service timestamps debug datetime msec localtime
    service timestamps log datetime msec localtime
    service password-encryption
    !
    hostname cisco
    !
    boot-start-marker
    boot-end-marker
    !
    logging buffered 51200 debugging
    logging console critical
    enable secret 0 cisco
    !
    no aaa new-model
    resource policy
    !
    clock timezone GMT 3
    clock summer-time GMT recurring last Sun Mar 2:00 last Sun Oct 3:00
    !
    dot11 ssid WiFi-home-AP
     max-associations 3
     authentication open
     authentication key-management wpa
     wpa-psk ascii 0 megapassword
    !
    no ip dhcp use vrf connected
    ip dhcp excluded-address 192.168.1.1
    !
    ip dhcp pool home-pool
     import all
     network 192.168.1.0 255.255.255.0
     default-router 192.168.1.1
     dns-server 192.168.1.1
    !
    !
    ip cef
    !
    ip ssh time-out 60
    ip ssh authentication-retries 2
    l2tp-class corbina
    !
    !
    !
    username cisco privilege 15 secret 0 cisco
    !
    pseudowire-class pw-class-1
    encapsulation l2tpv2
    protocol l2tpv2 corbina
    ip local interface FastEthernet4
    !
    !
    !
    bridge irb
    !
    !
    !
    interface FastEthernet0
    !
    interface FastEthernet1
    !
    interface FastEthernet2
    !
    interface FastEthernet3
    !
    interface FastEthernet4
     no shutdown
     ip address dhcp
     no ip redirects
     no ip unreachables
     no ip proxy-arp
     ip nat outside
     ip virtual-reassembly
     ip route-cache flow
     duplex auto
     speed auto
    !
    interface Dot11Radio0
     no ip address
     !
     encryption mode ciphers tkip
     !
     ssid WiFi-home-AP
     !
     speed basic-1.0 basic-2.0 basic-5.5 6.0 9.0 basic-11.0 12.0 18.0 24.0 36.0 48.0 54.0
     station-role root
     world-mode dot11d country RU both
     bridge-group 1
     bridge-group 1 subscriber-loop-control
     bridge-group 1 spanning-disabled
     bridge-group 1 block-unknown-source
     no bridge-group 1 source-learning
     no bridge-group 1 unicast-flooding
    !
    interface Virtual-PPP1
     ip address negotiated
     ip verify unicast reverse-path allow-self-ping
     no ip redirects
     no ip unreachables
     no ip proxy-arp
     ip mtu 1460
     ip nat outside
     ip virtual-reassembly
     no cdp enable
     ppp authentication chap callin
     ppp chap hostname corbina
     ppp chap password 0 corbina
     ppp ipcp route default
     pseudowire 85.21.0.255 1 encapsulation l2tpv2 pw-class pw-class-1
    !
    interface Vlan1
    no ip address
    bridge-group 1
    !
    interface BVI1
    ip address 192.168.1.1 255.255.255.0
    ip nat inside
    ip virtual-reassembly
    !
    ip route 10.0.0.0 255.0.0.0 dhcp
    ip route 78.107.23.0 255.255.255.0 dhcp
    ip route 78.107.69.98 255.255.255.255 dhcp
    ip route 83.102.231.32 255.255.255.240 dhcp
    ip route 83.102.146.96 255.255.255.224 dhcp
    ip route 85.21.79.0 255.255.255.0 dhcp
    ip route 85.21.90.0 255.255.254.0 dhcp
    ip route 85.21.108.16 255.255.255.240 dhcp
    ip route 85.21.88.130 255.255.255.255 dhcp
    ip route 85.21.138.208 255.255.255.240 dhcp
    ip route 85.21.52.254 255.255.255.255 dhcp
    ip route 85.21.72.80 255.255.255.240 dhcp
    ip route 85.21.0.255 255.255.255.255 dhcp     !  этот маршрут обязателен для l2tp
    ip route 89.179.135.67 255.255.255.255 dhcp
    ip route 195.14.50.1 255.255.255.255 dhcp
    ip route 195.14.50.16 255.255.255.255 dhcp
    ip route 195.14.50.21 255.255.255.255 dhcp
    ip route 195.14.50.26 255.255.255.255 dhcp
    ip route 195.14.50.93 255.255.255.255 dhcp
    ip route 213.234.192.8 255.255.255.255 dhcp
    ip route 85.21.192.3 255.255.255.255 dhcp
    ip route 83.102.254.255 255.255.255.255 dhcp
    !
    ip http server
    ip http authentication local
    ip http secure-server
    ip http timeout-policy idle 60 life 86400 requests 10000
    ip dns server
    ip nat inside source route-map local interface FastEthernet4 overload
    ip nat inside source route-map public interface Virtual-PPP1 overload
    !
    ip access-list extended nat
    deny ip any host 85.21.0.255
    permit ip 192.168.1.0 0.0.0.255 any
    !
    logging trap debugging
    no cdp run
    !
    access-list 1 permit 192.168.1.2
    !
    route-map public permit 10
    match ip address nat
    match interface Virtual-PPP1
    !
    route-map local permit 10
    match ip address nat
    match interface FastEthernet4
    !
    !
    control-plane
    !
    bridge 1 protocol ieee
    bridge 1 route ip
    !
    line con 0
    no modem enable
    !
    line aux 0
    no login
    !
    line vty 0 4
     access-class 1 in
     exec-timeout 10 0
     login local
     transport input telnet ssh
    !
    scheduler max-task-time 5000
    scheduler allocate 4000 1000
    scheduler interval 500
    sntp server 195.14.40.141
    sntp server 212.100.132.50
    end




http://homenet.beeline.ru/index.php?showtopic=206930&st=30&p=1063801518&#entry1063801518
