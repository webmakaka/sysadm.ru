---
layout: page
title: Инсталляция Ansible в Ubuntu 18.04
permalink: /devops/automation/ansible/install/ubuntu/
---

# Инсталляция Ansible в Ubuntu 18.04

Делаю: 13.03.2019

    $ sudo apt-add-repository -y ppa:ansible/ansible

    $ sudo apt update && sudo apt install -y ansible

    $ ansible --version
    ansible 2.7.8
