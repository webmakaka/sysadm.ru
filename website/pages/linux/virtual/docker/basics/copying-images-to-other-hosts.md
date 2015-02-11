---
layout: page
title: Копируем Images на другой Host
permalink: /linux/virtual/docker/basics/copying-images-to-other-hosts/
---


Компьютер 1:

Если нужно перенести контейнер делаем commit, чтобы получить image

    docker commit <container_id> <image_name>

Сохраняем image в файл.

    docker save -o /tmp/<image_name>.tar <image_name>

Проверяем, создался ли файл

    ls -lh /tmp/<image_name>.tar


Компьютер 2:

    tar -tf /tmp/<image_name>.tar

    docker load -i /tmp/<image_name>.tar

    docker images
