---
layout: page
title: Kubeflow End to End (ML)
permalink: /clouds/google/kubernetes/qwiklabs/kubernetes-solutions/kubeflow-end-to-end/
---

# [GSP221] Kubeflow End to End (ML)


<br/>

Делаю:  
30.05.2019


https://www.qwiklabs.com/focuses/5169?parent=catalog


<br/>

Kubeflow is a machine learning toolkit for Kubernetes. The project is dedicated to making deployments of machine learning (ML) workflows on Kubernetes simple, portable, and scalable. The goal is to provide a straightforward way to deploy best-of-breed open-source systems for ML to diverse infrastructures.

<br/>

**What you'll build**

In this lab you're going to build a web app that summarizes GitHub issues using a trained model. Upon completion, your infrastructure will contain:

* A Kubernetes Engine cluster with standard Kubeflow and Seldon Core installations.
* A training job that uses Tensorflow to generate a Keras model.
* A serving container that provides predictions.
* A UI that uses the trained model to provide summarizations for GitHub issues.

<br/>

**What you'll learn**

* How to install Kubeflow.
* How to run training using the Tensorflow job server to generate a Keras model.
* How to serve a trained model with Seldon Core.
* How to generate and use predictions from a trained model.

<br/>

### Enable Boost Mode

Cloud Shell window --> Settings --> Enable Boost Mode --> Restart Cloud Shell in Boost Mode". 

This will provision a larger instance for your Cloud Shell session, resulting in speedier Docker builds.


    $ export KUBEFLOW_TAG=0.4.0-rc.2
    $ git clone https://github.com/kubeflow/examples.git
    $ cd examples
    $ git checkout v${KUBEFLOW_TAG}

<br/>

### Set your GitHub token

1. Sign into your GitHub account in a new tab, then navigate to: https://github.com/settings/tokens.

2. Click Generate a new token, select no permissions.

3. Click on the Copy icon.

4. Back in Cloud Shell, set the GITHUB_TOKEN environment variable, replacing <token> with your GitHub token:

    $ export GITHUB_TOKEN=<token>

<br/>

### Install pyyaml

    $ pip install --user pyyaml

<br/>

### Install ksonnet

    $ export KS_VER=0.13.1
    $ export KS_BIN=ks_${KS_VER}_linux_amd64

<br/>

    $ wget -O /tmp/${KS_BIN}.tar.gz https://github.com/ksonnet/ksonnet/releases/download/v${KS_VER}/${KS_BIN}.tar.gz
    
<br/>

    $ mkdir -p ${HOME}/bin
    $ tar -xvf /tmp/${KS_BIN}.tar.gz -C ${HOME}/bin
    $ export PATH=$PATH:${HOME}/bin/${KS_BIN}

<br/>

### Install kfctl

    $ wget -P /tmp https://github.com/kubeflow/kubeflow/archive/v${KUBEFLOW_TAG}.tar.gz

    $ mkdir -p ${HOME}/src

    $ tar -xvf /tmp/v${KUBEFLOW_TAG}.tar.gz -C ${HOME}/src

    $ cd ${HOME}/src/kubeflow-${KUBEFLOW_TAG}/scripts

    $ ln -s kfctl.sh kfctl
    
    $ export PATH=$PATH:${HOME}/src/kubeflow-${KUBEFLOW_TAG}/scripts
    $ cd ${HOME}

<br/>

### Set your GCP project ID

    $ export PROJECT_ID=$(gcloud config get-value project)
    $ export ZONE=us-central1-a

    $ gcloud config set project ${PROJECT_ID}
    $ gcloud config set compute/zone ${ZONE}
    
<!-- $ gcloud config set container/new_scopes_behavior true -->

<br/>

### Authorize Docker

    // Allow Docker access to your project's Container Registry:
    $ gcloud auth configure-docker

<br/>

### Create a service account


    $ export SERVICE_ACCOUNT=user-gcp-sa

    $ export SERVICE_ACCOUNT_EMAIL=${SERVICE_ACCOUNT}@${PROJECT_ID}.iam.gserviceaccount.com

    $ gcloud iam service-accounts create ${SERVICE_ACCOUNT} \
    --display-name "GCP Service Account for use with kubeflow examples"

<br/>

    $ gcloud projects add-iam-policy-binding ${PROJECT_ID} --member \
    serviceAccount:${SERVICE_ACCOUNT_EMAIL} \
    --role=roles/storage.admin

// Generate a credentials file for upload to the cluster:

    $ export KEY_FILE=${HOME}/secrets/${SERVICE_ACCOUNT_EMAIL}.json

    $ gcloud iam service-accounts keys create ${KEY_FILE} \
    --iam-account ${SERVICE_ACCOUNT_EMAIL}

<br/>

### Create a storage bucket

    $ export BUCKET_NAME=kubeflow-${PROJECT_ID}
    $ gsutil mb -c regional -l us-central1 gs://${BUCKET_NAME}

<br/>

### Create a cluster

    $ kfctl init kubeflow-qwiklab --platform gcp --project ${PROJECT_ID}

    $ cd kubeflow-qwiklab

    $ kfctl generate platform

    $ sed -i 's/n1-standard-8/n1-standard-4/g' gcp_config/cluster.jinja

    $ kfctl apply platform


Cluster creation will take a few minutes to complete.

    // To verify the connection, run the following command:
    $ kubectl cluster-info
    Kubernetes master is running at https://104.198.40.210
    GLBCDefaultBackend is running at https://104.198.40.210/api/v1/namespaces/kube-system/services/default-http-backend:http/proxy
    Heapster is running at https://104.198.40.210/api/v1/namespaces/kube-system/services/heapster/proxy
    KubeDNS is running at https://104.198.40.210/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
    Metrics-server is running at https://104.198.40.210/api/v1/namespaces/kube-system/services/https:metrics-server:/proxy


<br/>

Verify that this IP address matches the IP address corresponding to the Endpoint in your Google Cloud Platform Console by comparing the Kubernetes master IP is the same as the Master_IP address in the previous step.

???

<br/>

### Upload service account credentials

    $ kubectl create secret generic user-gcp-sa \
        --from-file=user-gcp-sa.json="${KEY_FILE}"

<br/>

## Install Kubeflow

    $ kfctl generate k8s
    $ kfctl apply k8s

Installing Kubeflow will take several minutes.

ksonnet is a templating framework which allows you to utilize common object definitions and customize them to your environment. You'll begin by referencing Kubeflow templates and apply environment-specific parameters. Once manifests have been generated specifically for your cluster, they can be applied like any other Kubernetes object using kubectl.

<br/>

### Add Seldon to the default installation

    $ cd ${HOME}/kubeflow-qwiklab/ks_app
    $ ks generate seldon seldon
    $ ks apply default -c seldon

Your cluster now contains a Kubernetes installation with Seldon, which has the following components:

* Reverse HTTP proxy (Ambassador)
* Central dashboard
* Jupyterhub
* TF job dashboard
* TF job operator
* Seldon cluster manager
* Seldon cache

    $ kubectl get pods


<br/>

### Train a model

    $ cd ${HOME}/kubeflow-qwiklab/ks_app

    $ ks generate tf-job-simple-v1beta1 tfjob --name tfjob-issue-summarization

    $ cp ${HOME}/examples/github_issue_summarization/ks_app/components/tfjob.jsonnet components/

    $ ks param set tfjob gcpSecretName "user-gcp-sa"

    $ ks param set tfjob gcpSecretFile "user-gcp-sa.json"

    $ ks param set tfjob image "gcr.io/kubeflow-examples/tf-job-issue-summarization:v20180629-v0.1-2-g98ed4b4-dirty-182929"

    $ ks param set tfjob input_data "gs://kubeflow-examples/github-issue-summarization-data/github_issues_sample.csv"

    $ ks param set tfjob input_data_gcs_bucket "kubeflow-examples"
    
    $ ks param set tfjob input_data_gcs_path "github-issue-summarization-data/github-issues.zip"

    $ ks param set tfjob num_epochs "7"

    $ ks param set tfjob output_model "/tmp/model.h5"
    
    $ ks param set tfjob output_model_gcs_bucket "${BUCKET_NAME}"

    $ ks param set tfjob output_model_gcs_path "github-issue-summarization-data"

    $ ks param set tfjob sample_size "100000"

The training component tfjob is now configured to use a pre-built container image.

<br/>

### Launch training

    $ cd ${HOME}/kubeflow-qwiklab/ks_app
    $ ks apply default -c tfjob

    // View the running job
    $ kubectl get pod -l tf_job_name=tfjob-issue-summarization

    $ kubectl logs -f tfjob-issue-summarization-master-0

Дожидаемся:

    Total params: 6,191,702
    Trainable params: 6,189,902
    Non-trainable params: 1,800
    ____________________________

<br/>

    // To verify that training c
    ompleted successfully, check to make sure all three model files were uploaded to your Cloud Storage bucket
    $ gsutil ls gs://${BUCKET_NAME}/github-issue-summarization-data
    
<br/>
    
    gs://kubeflow-qwiklabs-gcp-b45268fd25278901/github-issue-summarization-data/body_pp.dpkl
    gs://kubeflow-qwiklabs-gcp-b45268fd25278901/github-issue-summarization-data/seq2seq_model_tutorial.h5
    gs://kubeflow-qwiklabs-gcp-b45268fd25278901/github-issue-summarization-data/title_pp.dpkl

<br/>

### Serve the trained model

    $ export SERVING_IMAGE=gcr.io/kubeflow-examples/issue-summarization-model:v20180718-g98ed4b4-qwiklab

<br/>

### Create the serving component

    $ cd ${HOME}/kubeflow-qwiklab/ks_app

    $ ks generate seldon-serve-simple-v1alpha2 issue-summarization-model \
    --name=issue-summarization \
    --image=${SERVING_IMAGE} \
    --replicas=2

    // Apply the component manifests to the cluster
    $ ks apply default -c issue-summarization-model

    $ kubectl get pods -l seldon-deployment-id=issue-summarization

<br/>

    $ kubectl logs  $(kubectl get pods \
        -lseldon-deployment-id=issue-summarization \
        -o=jsonpath='{.items[0].metadata.name}') \
    issue-summarization


<br/>

### Add a UI

    $ cd ${HOME}/kubeflow-qwiklab/ks_app
    
    $ ks generate deployed-service ui \
    --name issue-summarization-ui \
    --image gcr.io/kubeflow-examples/issue-summarization-ui:v20180629-v0.1-2-g98ed4b4-dirty-182929

    $ cp ${HOME}/examples/github_issue_summarization/ks_app/components/ui.jsonnet components/

    $ ks param set ui githubToken ${GITHUB_TOKEN}

    $ ks param set ui modelUrl "http://issue-summarization.kubeflow.svc.cluster.local:8000/api/v0.1/predictions"

<br/>

### Create the UI image

    $ cd ${HOME}/examples/github_issue_summarization/docker
    $ docker build -t gcr.io/${PROJECT_ID}/issue-summarization-ui:latest .

    $ docker push gcr.io/${PROJECT_ID}/issue-summarization-ui:latest

    $ cd ${HOME}/kubeflow-qwiklab/ks_app
    $ ks param set ui image gcr.io/${PROJECT_ID}/issue-summarization-ui:latest

<br/>

### Launch the UI

    $ ks apply default -c ui

    $ kubectl get pods -l app=issue-summarization-ui


<br/>

### View the UI

    $ kubectl port-forward svc/ambassador 8080:80

In Cloud Shell, click on the Web Preview button and select "Preview on port 8080."


This will open a new browser tab that shows the Kubeflow Central Dashboard. Add the text "issue-summarization/" to the end of the URL and press Enter (don't forget the trailing slash).

https://8080-dot-7498293-dot-devshell.appspot.com/issue-summarization/

Click the Populate Random Issue button to fill in the large text box with a random issue summary. Then click the Generate Title button to view the machine generated title produced by your trained model. Click the button a couple of times to give yourself some more data to look at in the next step.


<br/>

### View serving container logs

    $ kubectl logs -f $(kubectl get pods \
        -lseldon-deployment-id=issue-summarization \
        -o=jsonpath='{.items[0].metadata.name}') \
    issue-summarization

<br/>

Back in the UI, press the Generate Title button a few times to view the POST request in Cloud Shell. Since there are two serving containers, you might need to try a few times before you see the log entry.

Press Ctrl+C to return to the command prompt.

<br/>

### Clean up

Navigate to https://github.com/settings/tokens and remove the token you generated for this lab.

    $ gcloud container clusters delete kubeflow-qwiklab --zone=us-central1-a


