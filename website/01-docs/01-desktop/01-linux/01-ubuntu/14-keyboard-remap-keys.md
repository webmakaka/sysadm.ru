---
layout: page
title: Remap клавиш механической клавиатуры Redragon Elf
description: Remap клавиш механической клавиатуры Redragon Elf
keywords: desktop, linux, ubuntu, keyboard, remap
permalink: /desktop/linux/ubuntu/keyboard-remap-keys/
---

# Remap клавиш механической клавиатуры Redragon Elf

<br/>

Делаю!  
2025.01.27

<br/>

Купил клавиатуру "Redragon игровая клавиатура механическая проводная Elf"

<br/>

![Remap клавиш механической клавиатуры Redragon Elf](/img/desktop/linux/ubuntu/keyboard-remap-keys.png 'Remap клавиш механической клавиатуры Redragon Elf'){: .center-image }

<br/>

Ранее использовал длительное время клавиатуру попроще, и привык к расположению кнопок. А на новой клавиши End, PgUp, PgDn расположены для меня непривычно. Благо можно клавиши вытащить и местами поменять. Остается перенастроить назначение клавишь в операционной системе.

<br/>

```
$ xev
```

<br/>

Получил следующие значения

<br/>

```
keycode 110 (keysym 0xff50, Home)
keycode 115 (keysym 0xff57, End)

keycode 112 (keysym 0xff55, Prior)
keycode 117 (keysym 0xff56, Next)
```

<br/>

```
$ vi ~/.Xmodmap
```

<br/>

```
keycode 115 = Prior

keycode 112 = Next
keycode 117 = End
```

<br/>

```
// This will only work for the current session, after rebooting the key mapping will be restored to the default
$ xmodmap ~/.Xmodmap
```

<br/>

```
$ gnome-session-properties
```

<br/>

```
Add
```

<br/>

```
Name: xmodmap
Command: /usr/bin/xmodmap /home/marley/.Xmodmap
Comment: remaps keys
```

<br/>

Указать, разумеется нужно своего пользователя и ребутнуться.

<br/>

Посмотрим, что получится.

<br/>

### How to remap keys in Ubuntu xmodmap [+Video]

<br/>

https://www.youtube.com/watch?v=g_WUusDluLw

<br/>

https://www.thequantizer.com/remap-keys-startup-ubuntu/
