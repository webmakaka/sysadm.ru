---
layout: page
title: Незапоминаемые команды в принципе
permalink: /linux/commands/
---



Отсортировать файлы по размеру:

    $ ls -s | sort -n


Отсортировать каталоги по размеру:

    # du | sort -nr | cut -f2- | xargs du -hs


Создать архив zip:

    for i in $(find ./logs/ -name "*") ;do zip ./weblogic_logs.zip $i; done


Создать tar.gz:

    tar -cvzpf FileName.tar.gz ./file_dir

Извлечь tar.gz:

    tar -xvzpf FileName.tar.gz ./

Если:
tar: .: Not found in archive

Можно попробовать:

    tar -xzvf dynagen-0.11.0.tar.gz


Извлечь tar.bz2:

    tar jxf FileName.tar.bz2


Извлечь tar:

    tar xvf FileName.tar -C ./


Извлечь .tgz:

    tar xf FileName.tgz -C ./


Найти рекурсивно файлы размером более 100 Мб

    find . -size +100000k -exec du -h {} \;


Найти в файлах содержимое текста

    $ grep -rl 'что_ищем' /путь

// Пример:

    $ grep -rl 'ApplicationContext' ./

<!--
Какие порты используются приложениями:
ps -ef | grep java | grep "netcracker/config" | sed 's/^[a-zA-Z]\{1,\}[[:space:]]*\([0-9]\{1,5\}\).*\(\-Xmx[0-9]*m\).*t3.\{3\}\([a-zA-Z\.0-9]*:[0-9]\{4\}\)[[:space:]].*\-Dnetcracker\.home=\([^[:space:]]\{1,\}\).*$/\1\t\2\t\3\t\4/' | sort

-->

Удалить содержимое файла

    # cat /dev/null > my_file_log

Узнать количество строк в файле

    $ wc -l /etc/hosts
    19 /etc/hosts

<br/>

    $ wc -l /etc/hosts | cut -f1 -d ' '
    19

<br/>

    $ cat /etc/hosts | wc -l
    19



вывести начиня с 4 строки 4 строки текста

    head -n4 /etc/squid/squid.conf | tail -n4


Найти в файле все строки начинающиеся на http_access

    grep '^http_access' /etc/squid/squid.conf


открыт ли порт 4848 на сервере

    netstat -lpna |grep 4848


Список подключенных хостов

    $ netstat -lantp | grep ESTABLISHED |awk '{print $5}' | awk -F: '{print $1}' | sort -u


<br/>

    $ ls -R | grep ":$" | sed -e 's/:$//' -e 's/[^-][^\/]*\//--/g' -e 's/^/ /' -e 's/-/|/'


<div align="center">
	<img src="http://mtaalamu.ru/uploads/images/00/00/14/2012/05/30/1ecddd.png" border="0">
</div>


Проще

    # tree /etc/

<br/>

    ── screenrc
    ├── securetty
    ├── security
    │   ├── access.conf
    │   ├── chroot.conf
    │   ├── console.apps
    │   ├── console.handlers
    │   ├── console.perms
    │   ├── console.perms.d
    │   ├── group.conf


<br/>

    # cat /etc/nsswitch.conf | egrep "^passwd|^group|^shadow"
    passwd:     files ldap
    shadow:     files ldap
    group:      files ldap

<br/>

    # cat /etc/pam.d/sshd | egrep -v '^(#|$)'
    auth	   required	pam_sepermit.so
    auth       include      password-auth
    account    required     pam_nologin.so
    account    include      password-auth
    password   include      password-auth
    session    required     pam_selinux.so close
    session    required     pam_loginuid.so
    session    required     pam_selinux.so open env_params
    session    optional     pam_keyinit.so force revoke
    session    include      password-auth
    session required /lib64/security/pam_mkhomedir.so skel=/etc/skel/umask=0077
