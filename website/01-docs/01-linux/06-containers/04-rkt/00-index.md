---
layout: page
title: RKT (Roket)
permalink: /linux/containers/krt/
---

# RKT (Roket)


На виртуальной машине с coreos:


    1.	 First, we need to trust the remote site before we download any ACI file from there, as rkt verifies signatures by default:

    $ sudo rkt trust –prefix example.com/nginx

    // $ sudo rkt trust --prefix coreos.com/etcd

    2.	 Then we can fetch (download) an image from there:

    $ sudo rkt fetch example.com/nginx:latest

    // $ rkt fetch coreos.com/etcd:v2.3.7

    3.	 Then running the container with rkt is simple:

    $ sudo rkt run example.com/nginx:v1.8.0

    // $ sudo rkt run coreos.com/etcd:v2.3.7

    As you see, rkt appropriates ETags—as in our case v1.8.0 will be run.
