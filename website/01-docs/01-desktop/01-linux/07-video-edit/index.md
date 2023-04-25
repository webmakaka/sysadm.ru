---
layout: page
title: Развернуть видео на 90 градусов в ubuntu
description: Развернуть видео на 90 градусов в ubuntu
keywords: Развернуть видео на 90 градусов в ubuntu
permalink: /desktop/linux/editors/
---

# Развернуть видео на 90 градусов в ubuntu (в командной строке)

<br/>

```
$ ffmpeg -i ./P8193794.MOV -vf "transpose=1" -acodec copy output-video.mov
```
