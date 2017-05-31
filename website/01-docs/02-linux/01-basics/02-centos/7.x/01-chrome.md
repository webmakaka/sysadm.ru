---
layout: page
title: Инсталляция Google Chrome в Centos 7.x
permalink: /linux/basics/centos/7/chrome/
---

<br/>

# Инсталляция Chrome в Centos 7.x

<br/>

    # vi /etc/yum.repos.d/google-chrome.repo

<br/>

    [google-chrome]
    name=google-chrome
    baseurl=http://dl.google.com/linux/chrome/rpm/stable/$basearch
    enabled=1
    gpgcheck=1
    gpgkey=https://dl-ssl.google.com/linux/linux_signing_key.pub

<br/>

    # yum install google-chrome-stable
