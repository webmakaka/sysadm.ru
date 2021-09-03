---
layout: page
title: OpenVPN в Ubuntu
description: OpenVPN в Ubuntu
keywords: desktop, linux, ubuntu, vpn, openvpn, autostart
permalink: /desktop/linux/ubuntu/vpn/openvpn/
---

# OpenVPN

https://openvpn.net/community-downloads/

Делаю!  
02.09.2021

<br/>

### Инсталляция openvpn

    $ sudo apt install openvpn -y

<br/>

### Способ 2 [Недоделано]

    $ cd ~/tmp/
    $ wget https://swupdate.openvpn.org/community/releases/openvpn-2.5.3.tar.gz

    $ tar -zxvf openvpn-2.5.3.tar.gz
    $ cd openvpn-2.5.3/

Хз, что дальше.

Наверное, configure, make, make install

Пока не очень актуально. Буду юзать, что лежит в стандартных пакетах. Лень!

<br/>

### Подключаемся

<br/>

Запускаю от root. От обычного ошибка при подключении.

<br/>

    $ sudo su -

<br/>

    // Будет спрашивать логин и пароль
    # openvpn \
        --auth-nocache \
        --config /etc/openvpn/config.ovpn

<br/>

    // Не будет спрашивать, но пароли будут в консоли
    // Обратить внимание на разделитель login и пароль \n
    # openvpn \
        --auth-nocache \
        --config /etc/openvpn/config.ovpn \
        --auth-user-pass <(echo -e "login\npassword")

<br/>

### Автостарт OpenVpn

    $ sudo vi /etc/default/openvpn

<br/>

```
AUTOSTART="all"
```

<br/>

    $ sudo cp config.ovpn /etc/openvpn/client.conf

<br/>

    // Здесь можно отредактировать некоторые параметы
    $ sudo vi /etc/openvpn/client.conf

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

    // Прописываем логин и пароль
    // Первая строка логин, вторая пароль
    $ sudo vi /etc/openvpn/login.conf

<br/>

```
login
password
```

<br/>

    $ sudo chmod 400 /etc/openvpn/login.conf

<br/>

    $ sudo systemctl enable openvpn@client.service
    $ sudo systemctl daemon-reload
    $ sudo service openvpn@client start
    $ sudo service openvpn@client status

<br/>

**Использовалось:**  
https://www.ivpn.net/knowledgebase/linux/linux-autostart-openvpn-in-systemd-ubuntu/
