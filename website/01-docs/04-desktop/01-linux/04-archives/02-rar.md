---
layout: page
title: Rar в Linux
description: Rar в Linux
keywords: linux, Rar в Linux
permalink: /desktop/linux/archives/rar/
---

# Rar в Linux

<br/>

## Ubuntu

    # apt install -y \
    rar unrar-free

<br/>

## Centos

-- По идее, ставится так:

    # wget http://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-10.noarch.rpm
    # yum install -y rar unrar

-- rar из исходников (не проверял !!!)

    wget http://www.rarlab.com/rar/rarlinux-3.8.0.tar.gz
    tar -zxvf rarlinux-3.8.0.tar.gz
    cd rar
    su root
    make
    make install
    exit

<br/>

### Работа с архиватором rar

    // Извлечь .rar в каталог с текущим именем архива
    $ unrar x Archive.part1.rar

<br/>

    // Извлечь .rar в текущий каталог (Получается каша)
    $ unrar e archiveName.rar

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
