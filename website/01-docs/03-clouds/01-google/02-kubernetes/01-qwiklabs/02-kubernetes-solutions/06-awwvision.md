---
layout: page
title: Awwvision Cloud Vision API from a Kubernetes Cluster
permalink: /clouds/google/kubernetes/qwiklabs/kubernetes-solutions/awwvision/
---

# [GSP066] Awwvision: Cloud Vision API from a Kubernetes Cluster

<br/>

Делаю!  
28.05.2019


https://www.qwiklabs.com/focuses/1241?parent=catalog



### Overview


The Awwvision lab uses Kubernetes and Cloud Vision API to demonstrate how to use the Vision API to classify (label) images from Reddit's /r/aww subreddit and display the labelled results in a web app.

Awwvision has three components:

* A simple Redis instance.
* A web app that displays the labels and associated images.
* A worker that handles scraping Reddit for images and classifying them using the Vision API. Cloud Pub/Sub is used to coordinate tasks between multiple worker instances.

<br/>

### Create a Kubernetes Engine cluster


    $ gcloud config set compute/zone us-central1-f

    $ gcloud container clusters create awwvision \
    --num-nodes 2 \
    --scopes cloud-platform

    $ gcloud container clusters get-credentials awwvision


<br/>

### Deploy the sample

    $ git clone https://github.com/GoogleCloudPlatform/cloud-vision
    $ cd cloud-vision/python/awwvision
    $ make all

As part of the process, Docker images will be built and uploaded to the Google Container Registry private container registry. In addition, yaml files will be generated from templates, filled in with information specific to your project, and used to deploy the redis, webapp, and worker Kubernetes resources for the lab.

    $ kubectl get pods
    $ kubectl get deployments -o wide
    $ kubectl get svc awwvision-webapp
    NAME               TYPE           CLUSTER-IP      EXTERNAL-IP    PORT(S)        AGE
    awwvision-webapp   LoadBalancer   10.31.253.146   35.184.74.71   80:31602/TCP   81s


<br/>


http://35.184.74.71