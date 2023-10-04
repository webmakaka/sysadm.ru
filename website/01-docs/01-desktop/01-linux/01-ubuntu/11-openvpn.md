---
layout: page
title: OpenVPN в Ubuntu
description: OpenVPN в Ubuntu
keywords: desktop, linux, ubuntu, vpn, openvpn, autostart
permalink: /desktop/linux/ubuntu/vpn/openvpn/
---

# OpenVPN

https://openvpn.net/community-downloads/


<br/>

Делаю!  
2023.09.01

<br/>

### Инсталляция openvpn

```
$ sudo apt install -y openvpn
```

<br/>

### Подключаемся

<br/>

Запускаю от root. От обычного ошибка при подключении.

<br/>

```
$ sudo su -
```

<br/>

```
// Будет спрашивать логин и пароль
# openvpn \
    --auth-nocache \
    --config /etc/openvpn/config.ovpn
```

<br/>

```
// Не будет спрашивать, но пароли будут в консоли
// Обратить внимание на разделитель login и пароль \n
# openvpn \
    --auth-nocache \
    --config /etc/openvpn/config.ovpn \
    --auth-user-pass <(echo -e "login\npassword")
```

<br/>

### Автостарт OpenVpn

<br/>

```
$ sudo vi /etc/default/openvpn
```

<br/>

```
AUTOSTART="all"
```

<br/>

```
$ sudo cp config.ovpn /etc/openvpn/client.conf
```

<br/>

```
// Здесь можно отредактировать некоторые параметы
$ sudo vi /etc/openvpn/client.conf
```

<br/>

Например, для того, чтобы задать login и пароль (правда в откртом виде).

```
auth-user-pass
```

<br/>

меняю на

<br/>

```
auth-user-pass login.conf
```

<br/>

```
// Прописываем логин и пароль
// Первая строка логин, вторая пароль
$ sudo vi /etc/openvpn/login.conf
```

<br/>

```
login
password
```

<br/>

```
$ sudo chmod 400 /etc/openvpn/login.conf
```

<br/>

```
$ sudo systemctl enable openvpn@client.service
$ sudo systemctl daemon-reload
$ sudo service openvpn@client start
$ sudo service openvpn@client status
```

<br/>

**Использовалось:**  
https://www.ivpn.net/knowledgebase/linux/linux-autostart-openvpn-in-systemd-ubuntu/

<br/>

### Проблема с использованием DNS OpenVPN сервера

Проблема была в том, что не использовались DNS сервера внутренней сети.

<br/>

```
$ sudo apt install -y openvpn-systemd-resolved
```

<br/>

```
$ sudo vi /etc/openvpn/client.conf
```

<br/>

**Добавил:**

<br/>

```
***
script-security 2
up /etc/openvpn/update-systemd-resolved
down /etc/openvpn/update-systemd-resolved
down-pre

dhcp-option DOMAIN-ROUTE .
***
```

<br/>

**Использовалось:**  
https://askubuntu.com/questions/1032476/ubuntu-18-04-no-dns-resolution-when-connected-to-openvpn

<br/>

### Обновление

```
$ sudo cp /etc/openvpn/client.conf /etc/openvpn/client.conf.prev
$ sudo vi /etc/openvpn/client.conf

меняем сертификат

$ sudo service openvpn@client restart
$ route -n
```

<br/>

### Backup конфигов

Нужно скопировать куда-нибудь:

<br/>

```
login.conf 
client.conf
```


<br/>

### Восстановление после переустановки операционной системы


<br/>

Делаю!  
2023.10.03

<br/>

```
$ sudo apt install -y openvpn-systemd-resolved
```

<br/>

```
$ sudo cp ./login.conf /etc/openvpn/
$ sudo cp ./client.conf /etc/openvpn/
```


<br/>

```
$ sudo cp ./login.conf /etc/openvpn/
$ sudo cp ./client.conf /etc/openvpn/
```

<br/>

```
$ sudo systemctl enable openvpn@client.service
$ sudo systemctl daemon-reload
$ sudo service openvpn@client start
$ sudo service openvpn@client status
```
