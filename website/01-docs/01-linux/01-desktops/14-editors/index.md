---
layout: page
title: Текстовый редактор в Linux с подсветкой синтаксиса и парных тегов HTML (аналог notepad++ в Windows)
permalink: /linux/desktops/code/editors/
---

# Текстовый редактор в Linux с подсветкой синтаксиса и парных тегов HTML (аналог notepad++ в Windows)

### Atom от GitHub

Бесплатный редактор. По всему интернету пиарят именно этот редактор как самый оху@#$%ый. Денег не просит.

У меня сейчас используется он и gedit. Если совсем что-то простое - скопировать в буфер, то gedit. Если понабирать текст - atom, впрочем я его использую и для программирования на JavaScript.

Разрабатывался для пользователей mac, но сейчас работает и в Windows и в Linux. Мне видится Atom - может выступать и как бесплатная альтернатива Sublime Text.

На Ubuntu atom давно, как дела обстоят с другими Linux дистрибутивами не знаю. Впрочем на RedHat и его клонах работает на ура.

Куча плагинов на все случаи жизни. Настройка табуляции, скрытые непечатные символы, автосохранение, проверка орфографии, проверка синтаксиса, подсветка, автоформат текста, встроенный предпросмотр для MarkDown и т.д.

В корень проекта можно положить файл .editorconfig (см. исходники sysadm) и настройки редактора будут браться из него.

В общем, кого не устраивает gedit, советую попробовать. Подстветка парных тегов работает.

<br/>

### Инсталляция Atom в Ubuntu

    $ sudo add-apt-repository -y ppa:webupd8team/atom
    $ sudo apt-get update -y
    $ sudo apt-get install -y atom

Подробнее:
http://www.webupd8.org/2014/05/install-atom-text-editor-in-ubuntu-via-ppa.html


<br/>

### Настройка Atom

<br/>

**Сделать парные теги более заметными:**

Я не знаю как пришел к этому конфигу, но можно сделать и получше:

    $ cd ~/.atom/
    $ cp styles.less styles.less.orig

Заменяем, можно целиком файл.

    $ vi styles.less

<br/>

    editor, atom-text-editor::shadow {

        .bracket-matcher {
            border-bottom: 1px solid lime;
            position: absolute;
            border: 1px solid rgba(0, 255, 0, 0.7);
            background-color: rgba(0, 255, 0, 0.3);
            border-radius: 32px
        }
    }


<br/>

**Дополнительные пакеты для удобства работы:**


Edit --> Preferences --> Install

autosave - автосохранение (пакет инсталлируется по умолчанию, нужно его включить)

Atom Beautify - позволит одной командой сделать код более читаемым. Актуально в первую очередь для уже сконвертированного java script кода.

jshint - подсказка по ошибкам в javascrpt  

atom-typescript

<br/>

**Возможно также полезные настройки**

Edit --> Preferences --> Editor

    Show invisibles: true
    Tab Length: 4

    Invisible space: .
    Soft Tab: yes
    Tab type: Soft


<br/>

**Быстро найти файл по имени:**

CTRL + T и начать набирать название файла в проекте.


<br/>

<a href="//jsdev.org/env/atom/install-atom-on-ubuntu-14-04/"><strong>Еще больше материалов по Atom от меня</strong></a>


<br/>

# Visual Studo Code от Microsoft

https://code.visualstudio.com/

Легкий, беспланый редактор с кучей плагинов. Наверное, главная альтернатива Atom. 


<br/>

# Brackets от Adobe

http://brackets.io/

Можно посмотреть. Но в моем случае и так много всяких редакторов. Каких-то особых причин его использовать нет. Но он хорошо справляется со своими задачами.


<br/>

# Scite (наверное, следует использовать что-то из вышеперечисленного)

Я его нашел первым, как редактор с подсветкой парных символов. Не могу сказать, что много им пользовался.

Инсталляция:

    $ sudo apt-get install scite
    $ sudo apt-get install lua5.1


    $ cd /tmp/

    $ wget https://files.sysadm.ru/files/linux/editors/scite/scite_conf.zip

    $ unzip scite_conf.zip
    $ cd scite_conf/

    $ cp ./home/user/* ~/
    $ sudo cp -R ./usr/* /usr/


<br/>

# Другие редакторы

Были какие-то kedit - что-ли, еще какие-то. Вообщем ничего мне не понравилось.

Впочем, для программирования на java есть Eclipse и NetBeans.
Я знал программиста, который на NetBeans программировал для PHP проекта и говорил, что этот редактор самый лучший. Я попробовал. Для PHP из бесплатных, очень даже хорош.

Также имеются платные решения от JebBrains. Наверное, лучшие редакторы современности Idea для java, PhpStorm.
