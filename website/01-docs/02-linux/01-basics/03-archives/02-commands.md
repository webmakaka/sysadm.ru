---
layout: page
title: Команды работы с архивами в Linux
permalink: /linux/basics/archives/commands/
---

# Команды работы с архивами в Linux

<br/>

### 7z

-- Извлечь архив в файл

    # 7z x ./VBoxGuestAdditions_5.0.10.iso -o./VBoxGuestAdditions_5.0.10/


<br/>

### tar.gz

Создать tar.gz:

    tar -cvzpf FileName.tar.gz ./file_dir

Извлечь tar.gz:

    tar -xvzpf FileName.tar.gz ./

Если:
tar: .: Not found in archive

Можно попробовать:

    tar -xzvf dynagen-0.11.0.tar.gz



<br/>

### tar.bz2

Извлечь tar.bz2:

    tar -jxf FileName.tar.bz2


<br/>

### ZIP

Создать архив zip:

    for i in $(find ./logs/ -name "*") ;do zip ./weblogic_logs.zip $i; done

<br/>

### tar

Извлечь tar:

    tar xvf FileName.tar -C ./


<br/>

### .tgz

Извлечь .tgz:

    tar xf FileName.tgz -C ./



<br/>

### .rar

Извлечь .rar:

    $ unrar e archiveName.rar


Еще где-то стырено:    

    # unrar x (file_name).rar           extract with full path
    # unrar e -kb (file_name).rar       (Keep broken)
    # unrar l (file_name).rar           list files inside
    # unrar e (file_name).rar           dump files excluding folders
    # rar a (file_name).rar (file_name) create a archive Rar file
    # rar r (file_name).rar             recover or fix a archive file or files
    # rar a -p (file_name).rar          create a archive Rar file with password
