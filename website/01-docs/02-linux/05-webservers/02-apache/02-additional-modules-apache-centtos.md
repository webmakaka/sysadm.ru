---
layout: page
title: Apache Httpd сервер модули mod_ssl, mod_security, mod_python (Centos 6.6)
permalink: /linux/webservers/apache/mods/
---

# Apache Httpd сервер модули mod_ssl, mod_security, mod_python (Centos 6.6)

Понадобилось установить для Apache следующие модули: mod_ssl, mod_security, mod_python

    # cd /tmp

    mod_security и mod_python можно взять из epel repo

    ## RHEL/CentOS 6 64-Bit ##
    # wget http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
    # rpm -ivh epel-release-6-8.noarch.rpm


    # yum install -y mod_ssl mod_security mod_python

    # ls /usr/lib64/httpd/modules/

    mod_ssl.so
    mod_security2.so
    mod_python.so

    # httpd -M



http://habrahabr.ru/post/228339/
http://habrahabr.ru/post/145215/
http://modpython.org/live/current/doc-html/installation.html
http://olezhek.net/2009-11-11-apache-mod_python.html

http://www.jboss.ru/jboss_ru/servlet/articles?m=SHOW&id=3399
http://citforum.ru/programming/java/java2_linux/


Install the Mod_jk Module Into Apache HTTPD or Enterprise Web Server HTTPD
https://access.redhat.com/documentation/en-US/JBoss_Enterprise_Application_Platform/6/html/Administration_and_Configuration_Guide/Install_the_Mod_jk_Module_Into_Apache_HTTPD_or_Enterprise_Web_Server_HTTPD1.html


http://www.bluhm-de.com/ru/installing-mod_jk-for-apache-2.2-on-centos-6
https://grigoregabriel.wordpress.com/2013/06/03/apache-2-2-in-front-of-tomcat-6-on-redhat-and-centos-systems/


https://archive.today/tVJzC



odba
http://odba.ru/showthread.php?t=544
