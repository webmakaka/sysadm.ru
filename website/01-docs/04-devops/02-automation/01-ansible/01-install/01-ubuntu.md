---
layout: page
title: Инсталляция Ansible в Ubuntu 18.04
description: Инсталляция Ansible в Ubuntu 18.04
keywords: Инсталляция Ansible в Ubuntu 18.04
permalink: /devops/automation/ansible/install/ubuntu/
---

# Инсталляция Ansible в Ubuntu 18.04

Делаю: 09.03.2020

    $ sudo apt-add-repository -y ppa:ansible/ansible

    $ sudo apt update && sudo apt install -y ansible

    $ ansible --version
    ansible 2.9.6
