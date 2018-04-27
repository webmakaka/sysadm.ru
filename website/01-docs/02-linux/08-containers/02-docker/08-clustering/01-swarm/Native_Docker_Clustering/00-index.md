---
layout: page
title: Docker Swarm > Native Docker Clustering [2016, ENG]
permalink: /linux/containers/docker/clustering/swarm/Native_Docker_Clustering/
---

# Docker Swarm: Native Docker Clustering [2016, ENG]

<br/>

Вот такую схему собираем:

<br/>

<div align="center">
    <img src="//files.sysadm.ru/img/linux/containers/docker/clustering/swarm/native-docker-clustering/pic1.png" border="0" alt="Native Docker Clustering">
</div>

<br/>


В курсе используется consul. Исходники контейнеров можно попытаться восстоздать. В курсе они не приводятся.

<br/>


Я использую vagrant для старта сразу нескольких виртуальных машин virtualbox с coreos внутри.


**Файлы для старта виртуальных машин с coreos**

https://bitbucket.org/sysadm-ru/native-docker-clustering

<br/>

Шаг по настройке security не завершил. Без этого шага ничего не работает. Содрежимое контейнеров и как они работают, пока не разобрался.

<br/>

<ul>
    <li>
        <a href="/linux/containers/docker/clustering/swarm/Native_Docker_Clustering/configs/">Configs</a>
    </li>
    <li>
        <a href="/linux/containers/docker/clustering/swarm/Native_Docker_Clustering/Building_Your_Swarm_Infrastructure/">Module 4: Building your Swarm Infrastructure</a>
    </li>
    <li>
        <a href="/linux/containers/docker/clustering/swarm/Native_Docker_Clustering/Securing_your_Swarm_Cluster/">Module 5: Securing your Swarm Cluster</a>
    </li>
</ul>
