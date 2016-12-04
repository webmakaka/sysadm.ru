---
layout: page
title: Текстовый редактор в Linux с подсветкой синтаксиса и парных тегов HTML (аналог notepad++ в Windows)
permalink: /linux/editors/
---


# Atom от FaceBook

Бесплатный редактор. По всему интернету пиарят именно этот редактор как самый оху@#$%ый. Денег не просит.

У меня сейчас используется он и gedit. Если совсем что-то простое - скопировать в буфер, то gedit. Если понабирать текст - atom, впрочем я его использую и для программирования на javascript.

Разрабатывался для пользователей mac, но сейчас работает и в Windows и Linux. Мне видится Atom - бесплатная альтернатива Sublime Text.

На Ubuntu atom давно, как дела обстоят с другими Linux дистрибутивами не знаю.

Куча плагинов на все случаи жизни. Настройка табуляции, скрытые непечатные символы, автосохранение, проверка орфографии, проверка синтаксиса, подсветка, автоформат текста, встроенный предпросмотр для MarkDown и т.д.


В общем, кого не устраивает gedit, советую попробовать. Подстветка парных тегов работает.

### Инсталляция Atom в Ubuntu

    $ sudo add-apt-repository -y ppa:webupd8team/atom
    $ sudo apt-get update -y
    $ sudo apt-get install -y atom

Подробнее:
http://www.webupd8.org/2014/05/install-atom-text-editor-in-ubuntu-via-ppa.html



<br/>

### Настройка Atom

Edit --> Preferences

    Show invisibles: true
    Tab Length: 4

    Infisible space: .
    Soft Tab: yes
    Tab type: Soft


<!--

    $ apm install tabs-to-spaces
    tabs-to-spaces:untabify

    Подробнее:
    https://atom.io/packages/tabs-to-spaces

-->



<br/>

### Сделать парные теги более заметными:

    $ cd ~/.atom/
    $ cp styles.less styles.less.bkp

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

### Дополнительные пакеты для удобства работы:


Edit --> Preferences --> Install

autosave - автосохранение (пакет инсталлируется по умолчанию, нужно его включить)

Atom Beautify - позволит одной командой сделать код более читаемым. Актуально в первую очередь для уже сконвертированного java script кода.

jshint - подсказка по ошибкам в javascrpt  

atom-typescript


<br/><br/>

**Top 5 Packages: Atom Code Editor**

<br/><br/>

<div align="center">
    <iframe width="560" height="315" src="https://www.youtube.com/embed/DmKNDxlNLoE" frameborder="0" allowfullscreen></iframe>
</div>

<br/><br/>


    # 5 - 8 votes
    The Designer
    i like atom too and my favorite package is emmet https://github.com/emmetio/emmet-atom

    # 4 - 13 votes
    Rodrigo Lima Batista
    https://atom.io/packages/terminal-plus - Terminal-Plus. Love it. Not perfect or too popular but it is awesome.

    # 3 - 16 votes
    Lucas
    I really like Atom. I installed Minimap so it looks like Sublime Text ^^

    # 2 - 18 votes
    Windowface
    Travis, have you ever used Brackets code editor? I've been using it for a while now and love it

    # 0 - honorable mention
    Romain Lepert
    This atom package just makes my life better
    https://atom.io/packages/doge

    # 1 - 21 votes
    Dennis Vlaanderen
    https://atom.io/packages/atom-bootstrap4


<br/>

### Быстро найти файл по имени:

CTRL + T и начать набирать название файла в проекте.



<br/><br/>

# Brackets от Adobe

http://brackets.io/


Я бы, наверное постоянно использовал его (так как атом иногда мне кажется несколько тормознутым), но он не редактирует (надеюсь, что пока) файлы, лежащие на удаленном сервере (по sftp). Если этого для работы вам и не требуется, то наверное лучше выбрать его.

Хотя нет, я уже настолько придрочился к атому, что он у меня используется и когда я в Windows работаю, вместо когда-то любимого Notepad++


<br/><br/>

# scite

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


<br/><br/>

# Другие редакторы

Были какие-то kedit - что-ли, еще какие-то. Вообщем ничего мне не понравилось.

Впочем для программирования на java есть Eclipse и NetBeans.
Я знал программиста, который на NetBeans программировал для PHP проекта и говорил, что этот редактор самый лучший. Я попробовал. Для PHP из бесплатных очень даже хорош.
