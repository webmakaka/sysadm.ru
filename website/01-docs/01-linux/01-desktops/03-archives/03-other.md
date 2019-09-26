---
layout: page
title: Команды работы с архивами в Linux
permalink: /linux/desktops/archives/other/
---

# Команды работы с архивами в Linux

<br/>

### tar.gz

    // Создать tar.gz:
    $ tar -cvzpf FileName.tar.gz ./file_dir

    // Извлечь tar.gz:
    $ tar -xvzpf FileName.tar.gz ./

Если:
tar: .: Not found in archive

Можно попробовать:

    $ tar -xzvf dynagen-0.11.0.tar.gz

<br/>

### tar.bz2

    // Извлечь tar.bz2
    $ tar -jxf FileName.tar.bz2

<br/>

### ZIP

    // Создать zip архив кучи файлов по маске:
    $ for i in $(find ./logs/ -name "*") ;do zip ./weblogic_logs.zip $i; done


<br/>

### tar

    // Извлечь tar
    $ tar xvf FileName.tar -C ./

<br/>

### .tgz

    // Извлечь .tgz:   
    $ tar xf FileName.tgz -C ./

