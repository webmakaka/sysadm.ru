---
layout: page
title: Команды работы с архивами в Linux
permalink: /linux/desktops/archives/commands/
---

# Команды работы с архивами в Linux

<br/>

### 7z

    // Извлечь архив в каталог
    $  7z x ./VBoxGuestAdditions_5.0.10.iso -o./VBoxGuestAdditions_5.0.10/

    // Создать архив большого размера, поделив его на части
    $  7z a -v1024m Large-file-separated-in-multi-parts.zip Large-Many-Gigabytes-File.SQL

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

<br/>

### .rar

    // Извлечь .rar в текущий каталог:
    $ unrar e archiveName.rar

<br/>

    // Извлечь .rar в каталог с текущим именем архива:
    $ unrar x Archive.part1.rar

<br/>

Еще команды:

    # unrar x (file_name).rar           extract with full path
    # unrar e -kb (file_name).rar       (Keep broken)
    # unrar l (file_name).rar           list files inside
    # unrar e (file_name).rar           dump files excluding folders
    # rar a (file_name).rar (file_name) create a archive Rar file
    # rar r (file_name).rar             recover or fix a archive file or files
    # rar a -p (file_name).rar          create a archive Rar file with password

<br/>

    $ unrar x k4f2g.Typescript.AsyncAwait.in.Node.JS.with.testing.part1.rar > 1.txt

    Typescript AsyncAwait in Node JS with testing/2_-_Express_js_and_Models/6_-_setting_up_express_app_and_basic_routing.mp4 - checksum error
    Typescript AsyncAwait in Node JS with testing/2_-_Express_js_and_Models/7_-_setting_up_Mongoose_and_bodyParser.mp4 - checksum error
    Typescript AsyncAwait in Node JS with testing/2_-_Express_js_and_Models/8_-_User_Mongoose_Model_and_Signup_Route.mp4 - checksum error
    Typescript AsyncAwait in Node JS with testing/3_-_Authentication_and_AuthMiddleware/10_-_Authentication_Middleware.mp4 - checksum error
    Typescript AsyncAwait in Node JS with testing/4_-_Testing/12_-_Integration_test_with_Mocha_and_supertest.mp4 - checksum error
    Typescript AsyncAwait in Node JS with testing/5_-_Promise_and_Async_await_basics/17_-_Basics_of_Promises.mp4 : packed data checksum error in volume k4f2g.Typescript.AsyncAwait.in.Node.JS.with.testing.part4.rar
    Typescript AsyncAwait in Node JS with testing/5_-_Promise_and_Async_await_basics/17_-_Basics_of_Promises.mp4 - checksum error
    Typescript AsyncAwait in Node JS with testing/5_-_Promise_and_Async_await_basics/18_-_Mongoose_and_Promises.mp4 - checksum error
    Typescript AsyncAwait in Node JS with testing/6_-_Async_Await/20_-_more_async_await_and_refactoring_signin.mp4 - checksum error
