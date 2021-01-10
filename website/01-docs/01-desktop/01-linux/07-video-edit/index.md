---
layout: page
title: Развернуть видео на 90 градусов в ubuntu
description: Развернуть видео на 90 градусов в ubuntu
keywords: Развернуть видео на 90 градусов в ubuntu
permalink: /desktop/linux/editors/
---

# Развернуть видео на 90 градусов в ubuntu (в командной строке)

    # apt-get install libav-tools
    $ avconv -i FileName.mp4 -vf "transpose=1" NewFileName.mp4
