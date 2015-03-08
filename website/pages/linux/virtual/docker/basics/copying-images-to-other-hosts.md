---
layout: page
title: Копируем Images на другой Host
permalink: /linux/virtual/docker/basics/copying-images-to-other-hosts/
---


Компьютер 1:

Если нужно перенести контейнер делаем commit, чтобы получить image

    docker commit <container_id> <new_image_name>

Сохраняем image в файл.

    docker save -o /tmp/<new_image_name>.tar <new_image_name>

Проверяем, создался ли файл

    ls -lh /tmp/<new_image_name>.tar


Компьютер 2:

    tar -tf /tmp/<new_image_name>.tar

    docker load -i /tmp/<new_image_name>.tar

    docker images
