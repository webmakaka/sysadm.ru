---
layout: page
title: Инсталляция Libre Office в Ubuntu
permalink: /linux/desktops/ubuntu/libreoffice/
---

# Инсталляция Libre Office в Ubuntu

Делаю!  
14.08.2018

Понадобилось переустановить Libre Office

Сделал так:

    sudo apt-get remove --purge libreoffice*
    sudo apt-get clean
    sudo apt-get autoremove

<br/>

Скачал файл с оф.сайта libre office.

<br/>

    tar -xvzf LibreOffice_5.0.0_Linux_x86_deb.tar.gz
    cd LibreOffice_5.0.0.5_Linux_x86_deb/DEBS/
    dpkg -i *.deb

https://ask.libreoffice.org/en/question/56236/how-may-i-install-libreoffice-targz-file/
