---
layout: page
title: Инсталляция Libre Office в Ubuntu 18.04
permalink: /linux/desktops/ubuntu/libreoffice/
---

# Инсталляция Libre Office в Ubuntu 18.04

Делаю!  
19.02.2019

Понадобилось переустановить Libre Office

Сделал так:

    sudo apt-get remove --purge libreoffice*
    sudo apt-get clean
    sudo apt-get autoremove

<br/>

Скачал файл с оф.сайта libre office.

<br/>

    $ cd ~/Downloads/
    $ tar -xvzf LibreOffice_6.2.0_Linux_x86-64_deb.tar.gz
    $ cd LibreOffice_6.2.0.3_Linux_x86-64_deb/DEBS
    $ sudo dpkg -i *.deb

https://ask.libreoffice.org/en/question/56236/how-may-i-install-libreoffice-targz-file/
