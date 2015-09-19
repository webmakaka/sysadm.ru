---
layout: page
title: Текстовый редактор в Linux с подсветкой синтаксиса и парных тегов HTML (аналог notepad++ в Windows)
permalink: /linux/editors/
---


<h1>Atom</h1>

Вообщем по всему интернету пиарят именно этот редактор как самый охуенный.<br/>
Да у меня он сейчас используется он и gedit как основные средства редактирования кода.<br/>
Разрабатывался для пользователей mac, но умельцы заставили его работать и под ubuntu linux. Как дела обстоят с другими Linux дистрибутивами не знаю.

<br/><br/>

Вообщем, кого не устраивает gedit, советую попробовать. Подстветка парных тегов работает.

<br/><br/>

<pre>

    $ sudo add-apt-repository ppa:webupd8team/atom
    $ sudo apt-get update
    $ sudo apt-get install atom

Подробнее:
http://www.webupd8.org/2014/05/install-atom-text-editor-in-ubuntu-via-ppa.html




==========================

### Замена tab 4 символами пробела

Возможно, в новых версиях, уже не нужно этого делать.

Чтобы заменять tab на space (символы табуляции символами пробела):

    $ apm install tabs-to-spaces

И, наверное имеет смысл в настройках указать, что 1 tab - 4 символа пробела.


    Infisible space: .
    Soft Tab: yes
    Tab type: Soft
    tabs-to-spaces:untabify

Подробнее:
https://atom.io/packages/tabs-to-spaces

==========================

Сделать парные теги более заметными:

    cd /home/user_name/.atom
    cp styles.less styles.less.bkp

Заменяем, можно целиком файл.


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

Atom Beautify - позволит одной командой сделать код более читаемым. Актуально в первую очередь для уже сконвертированного java script кода.

jshint - подсказка по ошибкам в javascrpt


<br/>

### Быстро найти файл по имени:

CTRL + T и начать набирать название файла в проекте.



<br/><br/>
<hr/>
<br/><br/>

<h1>Brackets</h1>

http://brackets.io/

<br/><br/>

Я бы, наверное постоянно использовал его
(так как атом мне кажется несколько тормознутым),
но он не редактирует (надеюсь, что пока) файлы, лежащие
на удаленном сервере (по sftp). Если этого для работы вам
и не требуется, то наверное лучше выбрать его.


<br/><br/>
<hr/>
<br/><br/>

<h1>scite</h1>
<pre>

Инсталляция:

$ sudo apt-get install scite
$ sudo apt-get install lua5.1


$ cd /tmp/

$ wget http://prev.sysadm.ru/linux/editors/ubuntu/scite/scite_conf.zip

$ unzip scite_conf.zip
$ cd scite_conf/

$ cp ./home/user/* ~/
$ sudo cp -R ./usr/* /usr/
