---
layout: page
title: Скачать playlist с youtube в Ubuntu Linux
description: Скачать playlist с youtube в Ubuntu Linux
keywords: linux, ubuntu, youtube, скачать, playlist, youtube-dl
permalink: /linux/ubuntu/download-youtube-playlist/
---

# Скачать playlist с youtube в Ubuntu Linux

<br/>

### Скачать playlist с youtube в GUI

Я использую для этих целей 4k video downloader.

Из минусов - ограничение в 25 файлов в плейлисте для бесплатной версии и реклама уг.

<br/>

### Скачать playlist с youtube в командной строке (youtube-dl)

Т.к. 4k video downloader имеет ограничение на размер плейлиста. Буду юзать программу которая скачивает плейлисты в командной строке.

Делаю:  
13.03.2020

**Программа: **
https://rg3.github.io/youtube-dl/download.html

<br/>

**Установить ffmpeg - иначе могут быть видео и аудио отдельно!**

    $ sudo apt install -y ffmpeg

<br/>

    $ sudo curl -L https://yt-dl.org/downloads/latest/youtube-dl -o /usr/local/bin/youtube-dl
    $ sudo chmod a+rx /usr/local/bin/youtube-dl


<!--

<br/>

Можно установить ее с помощью pip3:
    $ sudo pip3 install youtube-dl --upgrade

-->

<br/>

### Поехали скачивать

    $ mkdir ~/Downloads/myPlaylist && cd ~/Downloads/myPlaylist

Нужно скачать вот этот плей лист. Там про kubernetes.

https://www.youtube.com/watch?v=YzaYqxW0wGs&list=PL34sAs7_26wNBRWM6BDhnonoA5FMERax0

Удаляю из url v=<ID> т.е v=YPShu0H3RbM&

-- Скачиваю видео лучшего качества из имеющегося:

    $ youtube-dl -i -f 'bestvideo[ext=mp4]+bestaudio[ext=m4a]/bestvideo+bestaudio' --merge-output-format mp4 https://www.youtube.com/watch?list=PL34sAs7_26wNBRWM6BDhnonoA5FMERax0 --output "%(title)s.%(ext)s"

<br/>

output я меняю, т.к. по умолчанию в конце добавляется id видео. Мне это не нужно.
Можно, также использовать такой формат как --output "%(uploader)s%(title)s.%(ext)s"

<br/>

**Еще интересные параметры:**

    --playlist-start 1 - с какого индекса в плейлисте начать
    -i - игнорить ошибки, вроде скрытого файла.

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

<br/>

You can choose best and type

    $ youtube-dl -f 22 http://www.youtube.com/watch?v=3JZ_D3ELwOQ

<br/>

To get the best video quality (1080p DASH - format "137") and best audio quality (DASH audio - format "140"), you must use the following command:

    $ youtube-dl -f 137+140 http://www.youtube.com/watch?v=3JZ_D3ELwOQ

<br/>

Подробнее:  
https://unix.stackexchange.com/questions/272868/download-only-format-mp4-on-youtube-dl/272934

<!-- <br/>

### Сообщение о необходимости обновить avconv

    WARNING: Your copy of avconv is outdated and unable to properly mux separate video and audio files, youtube-dl will download single file media. Update avconv to version 10-0 or newer to fix this.

    $ avconv |& grep \ version | awk '{print $3}'
    9.20-6:9.20-0ubuntu0.14.04.1,

    $ sudo add-apt-repository ppa:heyarje/libav-11 && sudo apt-get update
    $ sudo apt-get install -y libav-tools

    $ avconv |& grep \ version | awk '{print $3}'
    11.3-6:11.3-1~trusty, -->