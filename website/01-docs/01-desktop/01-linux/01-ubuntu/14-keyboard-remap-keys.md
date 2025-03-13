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
2025.02.07

<br/>

Купил в интернете клавиатуру "Redragon игровая клавиатура механическая проводная Elf".

<br/>

![Remap клавиш механической клавиатуры Redragon Elf](/img/desktop/linux/ubuntu/keyboard-remap-keys.png 'Remap клавиш механической клавиатуры Redragon Elf'){: .center-image }

<br/>

Ранее использовал длительное время клавиатуру попроще, и привык к расположению кнопок.
А на новой клавиши End, PgUp, PgDn расположены для меня непривычно.
Благо можно клавиши вытащить и местами поменять.
Остается перенастроить назначение клавиш в операционной системе.
Какое-то время я на поиски подходящего решения все-таки потерял и чтобы в следующий раз не потерять еще, лучше записать.

<br/>

### Xremap (Рекомендую этот вариант)

<br/>

**github:**  
https://github.com/xremap/xremap

<br/>

```
// Скачал с github
$ sudo mv xremap /usr/bin/
```

<br/>

```
$ sudo vi /etc/xremap-config.yml
```

<br/>

```
modmap:
  - name: End
    remap:
      KEY_PAGEDOWN: KEY_END
  - name: PgUp
    remap:
      KEY_END: KEY_PAGEUP
  - name: PgDn
    remap:
      KEY_PAGEUP: KEY_PAGEDOWN
```

<br/>

```
// Клавиши
https://github.com/emberian/evdev/blob/1d020f11b283b0648427a2844b6b980f1a268221/src/scancodes.rs#L26-L572
```

<br/>

**Добавлю в сервис при запуске;**

<br/>

```
$ sudo vi /etc/systemd/system/xremap.service
```

<br/>

```
[Unit]
Description=xremap

[Service]
Restart=always
TimeoutStartSec=2
ExecStart=/bin/sh -c '/usr/bin/xremap /etc/xremap-config.yml'
ExecStop=/usr/bin/pkill -f /usr/bin/xremap

[Install]
WantedBy=multi-user.target
```

<br/>

```
$ sudo systemctl enable xremap.service
$ sudo systemctl start  xremap.service
$ sudo systemctl status xremap.service
```

<br/>

### Xmodmap (Не рекомендую)

<br/>

Делаю!  
2025.01.28

<br/>

При старте работы в браузере, переставало работать.

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
$ sudo vi /opt/xmodmap.config
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
$ xmodmap /opt/xmodmap.config
```

<br/>

```
$ sudo vi /etc/profile.d/xmodmap.sh
```

<br/>

```bash
#!/bin/bash
if [ -f /opt/xmodmap.config ]; then
  /usr/bin/xmodmap /opt/xmodmap.config
else
  echo "Файл /opt/xmodmap.config не найден."
fi
```

<br/>

```
$ sudo chmod 755 /etc/profile.d/xmodmap.sh
$ source /etc/profile.d/xmodmap.sh
```

<!--


# =====================

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
Command: /usr/bin/xmodmap /opt/xmodmap
Comment: remaps keys
```

<br/>

Указать, разумеется нужно своего пользователя и ребутнуться. -->

<!-- <br/>

How to remap keys in Ubuntu xmodmap [+Video]

<br/>

https://www.youtube.com/watch?v=g_WUusDluLw

<br/>

https://www.thequantizer.com/remap-keys-startup-ubuntu/ -->

<br/>

### Input-remapper (Не рекомендую)

Не заработал. Возникала непонятная ошибка, которую не удалось победить.

<br/>

https://github.com/sezanzeb/input-remapper

<br/>

```
$ input-remapper-gtk
```

<br/>

```
// Удаляем
$ sudo apt remove -y input-remapper
```
