---
layout: page
title: Инсталляция Libre Office в Ubuntu 18.04
description: Инсталляция Libre Office в Ubuntu 18.04
keywords: desktop, linux, ubuntu, libreoffice
permalink: /desktop/linux/ubuntu/libreoffice/
---

# Инсталляция Libre Office в Ubuntu 18.04

Делаю!  
09.03.2022

Понадобилось переустановить Libre Office

Сделал так:

    sudo apt-get remove --purge libreoffice*
    sudo apt-get clean
    sudo apt-get autoremove

<br/>

Скачал файл с оф.сайта libre office.

<br/>

    $ cd ~/tmp
    $ wget https://download.documentfoundation.org/libreoffice/stable/7.3.1/deb/x86_64/LibreOffice_7.3.1_Linux_x86-64_deb.tar.gz
    $ tar -xvzf LibreOffice_7.3.1_Linux_x86-64_deb.tar.gz
    $ cd LibreOffice_7.3.1.3_Linux_x86-64_deb/DEBS/
    $ sudo dpkg -i *.deb

https://ask.libreoffice.org/en/question/56236/how-may-i-install-libreoffice-targz-file/
