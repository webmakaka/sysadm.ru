---
layout: page
title: Инсталляция CoreOS в virtualBox
permalink: /linux/virtual/coreos/installation/iso-disk/
---


### Первая попытка поставить на хост


На хосте стартовал ubuntu с USB флешки в режиме для ознакомления.

На рабочей станции, с которой планирую подключаться к серверу сгенерирова публичный ключ. Его потом нужно прописать в конфиге.



<br/>

### На Ubuntu

    # cd /tmp

    # wget https://raw.githubusercontent.com/coreos/init/master/bin/coreos-install
    # chmod +x coreos-install


<br/>

### Подготавливю минимальный конфиг

<script src="http://gist-it.appspot.com/https://github.com/sysadm-ru/coreos-cloud-config/blob/master/cloud-config.yaml">
</script>

С названием сетевых интерфейсов пока не разобрался как они задаются. Поэтому несколько раз переделывал все шаги, пока не угадал нужный.



Проверка валидности конфига:  
https://coreos.com/validate/


// Скачиваю конфиг приведенный выше. Разумеется, в него нужно подставить свой rsa ключ

    # wget https://raw.githubusercontent.com/sysadm-ru/coreos-cloud-config/master/cloud-config.yaml

// Запускаю инсталляцию

    # ./coreos-install -d /dev/sda -C stable -c ./cloud-config.yaml


<br/>

### Подключаюсь по SSH к хосту с CoreOS

    $ ssh core@192.168.1.200
    CoreOS stable (835.11.0)
    core@localhost ~ $


<br/>
<br/>


Может быть полезным:  

https://deis.com/blog/2015/coreos-on-virtualbox  
https://coreos.com/os/docs/latest/installing-to-disk.html  
https://coreos.com/os/docs/latest/booting-with-iso.html  
http://www.youtube.com/watch?v=yiWa0KFJDfI  


http://www.liberidu.com/blog/2015/04/11/basic-newbie-install-coreos-on-virtualbox-getting-started-with-docker/
