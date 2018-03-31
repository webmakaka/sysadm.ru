---
layout: page
title: Скачать playlist с youtube в Ubuntu Linux
permalink: /linux/basics/ubuntu/download-youtube-playlist/
---


# Скачать playlist с youtube в Ubuntu Linux

<br/>

### Скачать playlist с youtube в GUI

Я использую для этих целей 4k video downloader.

Из минусов - ограничение в 25 файлов в плейлисте для бесплатной версии и реклама уг.



<br/>

### Скачать playlist с youtube в командной строке (youtube-dl)

Т.к. 4k video downloader имеет ограничение на размер плейлиста. Буду юзать программу которая скачивает плейлисты в командной строке.

Первый раз делаю!  
30.03.2018


**Программа: **
https://rg3.github.io/youtube-dl/download.html


Вроде можно установить ее с помощью pip, что-то вроде $ sudo pip install youtube-dl --upgrade


    $ sudo curl -L https://yt-dl.org/downloads/latest/youtube-dl -o /usr/local/bin/youtube-dl
    $ sudo chmod a+rx /usr/local/bin/youtube-dl

    $ cd ~/Downloads/
    $ mkdir -p myPlaylist
    $ cd myPlaylist/

    -- качаю playlit курса по TypeScript от Microsoft (чтобы на javascript программировать как на java)
    https://www.youtube.com/watch?v=YPShu0H3RbM&list=PLqq-6Pq4lTTanfgsbnFzfWUhhAz3tIezU
    
Удаляю из url v=<ID> т.е ?v=YPShu0H3RbM&
    
    -- Скачиваю видео лучшего качества из имеющегося:
    $ youtube-dl -f 'bestvideo[ext=mp4]+bestaudio[ext=m4a]/mp4' https://www.youtube.com/watch?list=PLqq-6Pq4lTTanfgsbnFzfWUhhAz3tIezU --output "%(title)s.%(ext)s"
    

output я меняю, т.к. по умолчанию в конце добавляется id видео. Мне это не нужно. 
Можно, также использовать такой формат как --output "%(uploader)s%(title)s.%(ext)s"


Результат:

    $ ls
    TypeScript Basics 10 - Implicit typing.mp4
    TypeScript Basics 11 - Any and union types.mp4
    TypeScript Basics 12 - TypeScript classes.mp4
    TypeScript Basics 13 - Methods and constructors.mp4
    TypeScript Basics 14 - Inheritance and Polymorphism In TypeScript.mp4
    TypeScript Basics 15 - Interfaces and Duck Typing.mp4
    TypeScript Basics 16 - Member visibility.mp4
    TypeScript Basics 17 - readonly modifier.mp4
    TypeScript Basics 18 - Enums.mp4
    TypeScript Basics 19 - Generics.mp4
    TypeScript Basics 1 - Course plan and prerequisites.mp4
    TypeScript Basics 20 - Modules.mp4
    TypeScript Basics 21 - TypeScript compiler arguments.mp4
    TypeScript Basics 22 - Using tsconfig json file.mp4
    TypeScript Basics 23 - Creating an npm project.mp4
    TypeScript Basics 24 - Installing libraries and type definitions.mp4
    TypeScript Basics 25 - Setting up the project.mp4
    TypeScript Basics 26 - Installing dependencies.mp4
    TypeScript Basics 27 - Creating model classes.mp4
    TypeScript Basics 28 - Creating an api request service.mp4
    TypeScript Basics 29 - Calling service and troubleshooting errors.mp4
    TypeScript Basics 2 - Examining some problems with JavaScript.mp4
    TypeScript Basics 30 - Convert response to model object.mp4
    TypeScript Basics 31 - Using callbacks and handling repo response.mp4
    TypeScript Basics 32 - Chaining calls and accepting commandline argument.mp4
    TypeScript Basics 33 - Conclusion.mp4
    TypeScript Basics 3 - How TypeScript works.mp4
    TypeScript Basics 4 - TypeScript versus JavaScript.mp4
    TypeScript Basics 5 - Setting up TypeScript.mp4
    TypeScript Basics 6 - Introducing type declarations.mp4
    TypeScript Basics 7 - Arrays and tuples.mp4
    TypeScript Basics 8 - Type erasure and error behavior.mp4
    TypeScript Basics 9 - Typing with functions.mp4

    
<br/>
    
**Можно также выбрать более подходящий формат:**

    youtube-dl -F http://www.youtube.com/watch?v=3JZ_D3ELwOQ
    sample output:

    [youtube] Setting language
    [youtube] 3JZ_D3ELwOQ: Downloading webpage
    [youtube] 3JZ_D3ELwOQ: Downloading video info webpage
    [youtube] 3JZ_D3ELwOQ: Extracting video information
    [info] Available formats for 3JZ_D3ELwOQ:
    format code extension resolution  note 
    171         webm      audio only  DASH webm audio , audio@ 48k (worst)
    140         m4a       audio only  DASH audio , audio@128k
    160         mp4       192p        DASH video 
    133         mp4       240p        DASH video 
    134         mp4       360p        DASH video 
    135         mp4       480p        DASH video 
    136         mp4       720p        DASH video 
    137         mp4       1080p       DASH video 
    17          3gp       176x144     
    36          3gp       320x240     
    5           flv       400x240     
    43          webm      640x360     
    18          mp4       640x360     
    22          mp4       1280x720    (best)
    
    
You can choose best and type

    $ youtube-dl -f 22 http://www.youtube.com/watch?v=3JZ_D3ELwOQ
    
To get the best video quality (1080p DASH - format "137") and best audio quality (DASH audio - format "140"), you must use the following command:

    $ youtube-dl -f 137+140 http://www.youtube.com/watch?v=3JZ_D3ELwOQ

    
Подробнее:      
https://unix.stackexchange.com/questions/272868/download-only-format-mp4-on-youtube-dl/272934

<br/>

### Сообщение о необходимости обновить avconv

    WARNING: Your copy of avconv is outdated and unable to properly mux separate video and audio files, youtube-dl will download single file media. Update avconv to version 10-0 or newer to fix this.

    $ avconv |& grep \ version | awk '{print $3}'
    9.20-6:9.20-0ubuntu0.14.04.1,
    
    $ sudo add-apt-repository ppa:heyarje/libav-11 && sudo apt-get update
    $ sudo apt-get install -y libav-tools

    $ avconv |& grep \ version | awk '{print $3}'
    11.3-6:11.3-1~trusty,
