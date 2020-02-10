---
layout: page
title: Скопировать Docker Images на другой Host
permalink: /devops/containers/docker/basics/copying-images-to-other-hosts/
---

# Скопировать Docker Images на другой Host

Делаю:  
17.08.2019

<br/>

Host 1:

Если нужно перенести контейнер делаем commit, чтобы получить image

    $ docker commit <container_id> <new_image_name>

Сохраняем image в файл.

    $ docker save -o /tmp/<new_image_name>.tar <new_image_name>

Проверяем, создался ли файл

    $ ls -lh /tmp/<new_image_name>.tar


<br/>

Host 2:  
(Старые версии docker, до веррии 1.4 так не умеют. Врочем все, что меньше 18, наверное уже старье)


<!--
    // Посмотреть всяку ерунду в контейнере
    $ tar -tf /tmp/<new_image_name>.tar
-->

    $ docker load -i /tmp/<new_image_name>.tar

    $ docker images | grep <new_image_name>
