---
layout: page
title: Уменьшить размер занимаемого виртуальной машиной дискового пространства
description: Уменьшить размер занимаемого виртуальной машиной дискового пространства
keywords: Уменьшить размер занимаемого виртуальной машиной дискового пространства
permalink: /devops/linux/virtual/virtualbox/decrease-disk-space/
---

# Уменьшить размер занимаемого виртуальной машиной дискового пространства

Имеем экспортированную виртуальную машину VirtualBox размером 46,2 GB.

1. Подключаюсь к виртуальной машине и забиваю блоки дисков, которые не используются нулями.

Смотрю куда смонтированы диски.

    # mount

Нужно зайти в точку монтирования и забить свободное место нулями.

<br/>

    # dd if=/dev/zero of=file_for_removing bs=1M

После файл нужно удалить.

    # rm -rf file_for_removing

Когда выполнил такие операции, виртуальная машина стала занимать не 8 ГБ а 3 ГБ.

2. Выполняю действия по уменьшению диска

    vboxmanage modifyhd vm_oel62_oracle112_dsk1.vdi compact

В результате мне пришлось сконвертировать vmdk в vdi.

Получилось 38.1 GB. Правда виртуалка была забита файлами.