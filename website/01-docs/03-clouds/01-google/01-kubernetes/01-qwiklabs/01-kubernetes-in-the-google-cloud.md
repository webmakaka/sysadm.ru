---
layout: page
title: Обучение kuberneters в qwiklabs - Kubernetes in the Google Cloud
permalink: /clouds/google/kubernetes/qwiklabs/kubernetes-in-the-google-cloud/
---

# Обучение kuberneters в qwiklabs - Kubernetes in the Google Cloud

<br/>

https://www.qwiklabs.com/quests/29


<br/>

Делаю!  
20.05.2019

<br/>

### [GSP100] Kubernetes Engine: Qwik Start

    $ gcloud auth list
    $ gcloud config list project

    $ gcloud config set compute/zone us-central1-a

    $ gcloud container clusters create marleycluster1

    $ gcloud container clusters get-credentials marleycluster1

    $ kubectl run hello-server --image=gcr.io/google-samples/hello-app:1.0 --port 8080

    $ kubectl expose deployment hello-server --type="LoadBalancer"

    $ kubectl get service hello-server

    $ kubectl get service hello-server
    NAME           TYPE           CLUSTER-IP   EXTERNAL-IP      PORT(S)          AGE
    hello-server   LoadBalancer   10.7.254.2   35.238.204.137   8080:31703/TCP   83s

<br/>

http://35.238.204.137:8080/

<br/>

    $ gcloud container clusters delete marleycluster1


<br/>

### [GSP021] Orchestrating the Cloud with Kubernetes

<a href="https://github.com/kelseyhightower/app">App</a> is hosted on GitHub and provides an example 12-Factor application. During this lab you will be working with the following Docker images:

<a href="https://hub.docker.com/r/kelseyhightower/monolith">kelseyhightower/monolith</a> - Monolith includes auth and hello services.
<a href="https://hub.docker.com/r/kelseyhightower/auth">kelseyhightower/auth</a> - Auth microservice. Generates JWT tokens for authenticated users.
<a href="https://hub.docker.com/r/kelseyhightower/hello">kelseyhightower/hello</a>  - Hello microservice. Greets authenticated users.
ngnix - Frontend to the auth and hello services.

<br/>

    $ gcloud config set compute/zone us-central1-b
    $ gcloud container clusters create io

<br/>

    $ kubectl run nginx --image=nginx:1.10.0
    $ kubectl expose deployment nginx --port 80 --type LoadBalancer

    $ kubectl get services


<br/>

**Creating a Service**

<br/>

    $ git clone https://github.com/googlecodelabs/orchestrate-with-kubernetes.git

    $ cd orchestrate-with-kubernetes/kubernetes

    $ kubectl create -f pods/monolith.yaml
    $ kubectl get pods

    $ kubectl describe pods monolith

<br/>

    $ kubectl port-forward monolith 10080:80

Сессия 2

    $ curl http://127.0.0.1:10080
    {"message":"Hello"}

    $ curl http://127.0.0.1:10080/secure
    authorization failed

    $ curl -u user http://127.0.0.1:10080/login
    Enter host password for user 'user': [password]

    $ TOKEN=$(curl http://127.0.0.1:10080/login -u user|jq -r '.token')
    Enter host password for user 'user':[password]

    $ curl -H "Authorization: Bearer $TOKEN" http://127.0.0.1:10080/secure
    {"message":"Hello"}

    $ kubectl logs -f monolith

Сессия 3

    $ curl http://127.0.0.1:10080

<br/>

    $ kubectl exec monolith --stdin --tty -c monolith /bin/sh
    # ping -c 3 google.com
    # exit


<br>

**Services**

    * ClusterIP (internal) -- the default type means that this Service is only visible inside of the cluster,
    * NodePort gives each node in the cluster an externally accessible IP and
    * LoadBalancer adds a load balancer from the cloud provider which forwards traffic from the service to Nodes within it.

<br>

    $ cd ~/orchestrate-with-kubernetes/kubernetes

    $ kubectl create secret generic tls-certs --from-file tls/
    $ kubectl create configmap nginx-proxy-conf --from-file nginx/proxy.conf
    $ kubectl create -f pods/secure-monolith.yaml

    $ kubectl create -f services/monolith.yaml

    $ gcloud compute firewall-rules create allow-monolith-nodeport \
    --allow=tcp:31000

    NAME                     NETWORK  DIRECTION  PRIORITY  ALLOW      DENY  DISABLED
    allow-monolith-nodeport  default  INGRESS    1000      tcp:31000        False

<br/>

    $ gcloud compute instances list
    NAME                               ZONE           MACHINE_TYPE   PREEMPTIBLE  INTERNAL_IP  EXTERNAL_IP      STATUS
    gke-io-default-pool-f40e6c02-961s  us-central1-b  n1-standard-1               10.128.0.4   34.66.156.29     RUNNING
    gke-io-default-pool-f40e6c02-b6rw  us-central1-b  n1-standard-1               10.128.0.2   104.197.229.212  RUNNING
    gke-io-default-pool-f40e6c02-jw06  us-central1-b  n1-standard-1               10.128.0.3   35.232.234.44    RUNNING

<br/>

    $ curl -k https://<EXTERNAL_IP>:31000

<br/>

**Adding Labels to Pods**

    $ kubectl get pods -l "app=monolith"
    NAME              READY   STATUS    RESTARTS   AGE
    monolith          1/1     Running   0          22m
    secure-monolith   2/2     Running   0          8m29s

<br/>

    $ kubectl get pods -l "app=monolith,secure=enabled"
    No resources found.

<br/>

    $ kubectl label pods secure-monolith 'secure=enabled'

    $ kubectl get pods secure-monolith --show-labels
    NAME              READY   STATUS    RESTARTS   AGE     LABELS
    secure-monolith   2/2     Running   0          9m56s   app=monolith,secure=enabled

<br/>

    $ kubectl describe services monolith | grep Endpoints
    Endpoints:                10.4.1.4:443


<br/>

    $ gcloud compute instances list
    NAME                               ZONE           MACHINE_TYPE   PREEMPTIBLE  INTERNAL_IP  EXTERNAL_IP      STATUS
    gke-io-default-pool-f40e6c02-961s  us-central1-b  n1-standard-1               10.128.0.4   34.66.156.29     RUNNING
    gke-io-default-pool-f40e6c02-b6rw  us-central1-b  n1-standard-1               10.128.0.2   104.197.229.212  RUNNING
    gke-io-default-pool-f40e6c02-jw06  us-central1-b  n1-standard-1               10.128.0.3   35.232.234.44    RUNNING

<br/>

    $ curl -k https://10.4.1.4:31000


<br/>

**Deploying Applications with Kubernetes**

    auth - Generates JWT tokens for authenticated users.
    hello - Greet authenticated users.
    frontend - Routes traffic to the auth and hello services.

<br/>

    $ kubectl create -f deployments/auth.yaml
    $ kubectl create -f services/auth.yaml

    $ kubectl create -f deployments/hello.yaml
    $ kubectl create -f services/hello.yaml

    $ kubectl create configmap nginx-frontend-conf --from-file=nginx/frontend.conf
    $ kubectl create -f deployments/frontend.yaml
    $ kubectl create -f services/frontend.yaml

<br/>

    $ kubectl get services frontend
    NAME       TYPE           CLUSTER-IP     EXTERNAL-IP    PORT(S)         AGE
    frontend   LoadBalancer   10.7.250.249   35.188.84.62   443:32527/TCP   39s

    $ curl -k https://35.188.84.62 
    {"message":"Hello"}

<br/>

# [GSP053] Managing Deployments Using Kubernetes Engine

    $ gcloud config set compute/zone us-central1-a

    $ git clone https://github.com/googlecodelabs/orchestrate-with-kubernetes.git

    $ cd orchestrate-with-kubernetes/kubernetes

    $ gcloud container clusters create bootcamp --num-nodes 5 --scopes "https://www.googleapis.com/auth/projecthosting,storage-rw"


<br/>

    $ vi deployments/auth.yaml

```
containers:
- name: auth
  image: kelseyhightower/auth:1.0.0
```

    $ kubectl create -f deployments/auth.yaml
    $ kubectl create -f services/auth.yaml
    $ kubectl create -f deployments/hello.yaml
    $ kubectl create -f services/hello.yaml

<br/>

    $ kubectl create secret generic tls-certs --from-file tls/
    $ kubectl create configmap nginx-frontend-conf --from-file=nginx/frontend.conf
    $ kubectl create -f deployments/frontend.yaml
    $ kubectl create -f services/frontend.yaml

<br/>

    $ kubectl get services frontend

<br/>

    $ curl -ks https://<EXTERNAL-IP>

Или лучше:

    $ curl -ks https://`kubectl get svc frontend -o=jsonpath="{.status.loadBalancer.ingress[0].ip}"`
    {"message":"Hello"}


<br/>

**Scale a Deployment**

    $ kubectl explain deployment.spec.replicas

    $ kubectl scale deployment hello --replicas=5

<br/>

    $ kubectl get pods | grep hello- | wc -l
    5

<br/>

    $ kubectl scale deployment hello --replicas=3

<br/>

**Rolling update**


    $ kubectl edit deployment hello

```
containers:
- name: hello
  image: kelseyhightower/hello:2.0.0
```

<br/>

    $ kubectl get replicaset
    NAME                  DESIRED   CURRENT   READY   AGE
    auth-6bb8dcd7bd       1         1         1       11m
    frontend-77f46bf858   1         1         1       10m
    hello-5cbf94fc49      0         0         0       11m
    hello-677685c76       3         3         3       20s

<br/>

    $ kubectl rollout history deployment/hello
    deployment.extensions/hello
    REVISION  CHANGE-CAUSE
    1         <none>
    2         <none>

<br/>

**Pause a rolling update**

    $ kubectl rollout pause deployment/hello

    $ kubectl rollout status deployment/hello
    deployment "hello" successfully rolled out

    $ kubectl get pods -o jsonpath --template='{range .items[*]}{.metadata.name}{"\t"}{"\t"}{.spec.containers[0].image}{"\n"}{end}'
    auth-6bb8dcd7bd-fpttf           kelseyhightower/auth:1.0.0
    frontend-77f46bf858-p77hp               nginx:1.9.14
    hello-677685c76-lr92g           kelseyhightower/hello:2.0.0
    hello-677685c76-tj25x           kelseyhightower/hello:2.0.0
    hello-677685c76-vwd2l           kelseyhightower/hello:2.0.0


<br/>

**Resume a rolling update**

    $ kubectl rollout resume deployment/hello
    $ kubectl rollout status deployment/hello

<br/>

**Rollback an update**

    $ kubectl rollout undo deployment/hello
    $ kubectl rollout history deployment/hello

    $ kubectl get pods -o jsonpath --template='{range .items[*]}{.metadata.name}{"\t"}{"\t"}{.spec.containers[0].image}{"\n"}{end}'
    auth-6bb8dcd7bd-fpttf           kelseyhightower/auth:1.0.0
    frontend-77f46bf858-p77hp               nginx:1.9.14
    hello-5cbf94fc49-2j62n          kelseyhightower/hello:1.0.0
    hello-5cbf94fc49-sfzs8          kelseyhightower/hello:1.0.0
    hello-5cbf94fc49-tcm6w          kelseyhightower/hello:1.0.0

<br/>


**Canary deployments**

When you want to test a new deployment in production with a subset of your users, use a canary deployment. Canary deployments allow you to release a change to a small subset of your users to mitigate risk associated with new releases.



    $ vi deployments/hello-canary.yaml


version: 1.0.0

```
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: hello
        track: canary
        # Use ver 1.0.0 so it matches version on service selector
        version: 1.0.0
```

    $ kubectl create -f deployments/hello-canary.yaml

<br/>

    $ kubectl get deployments
    NAME           DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
    auth           1         1         1            1           20m
    frontend       1         1         1            1           19m
    hello          3         3         3            3           20m
    hello-canary   1         1         1            1           18s

<br/>

**Verify the canary deployment**

    $ curl -ks https://`kubectl get svc frontend -o=jsonpath="{.status.loadBalancer.ingress[0].ip}"`/version
    {"version":"1.0.0"}


В общем если повторять, то с вероятностью 25% можно получить версию 2.0.0

<br/>


**Blue-green deployments**

Rolling updates are ideal because they allow you to deploy an application slowly with minimal overhead, minimal performance impact, and minimal downtime. There are instances where it is beneficial to modify the load balancers to point to that new version only after it has been fully deployed. In this case, blue-green deployments are the way to go.

Kubernetes achieves this by creating two separate deployments; one for the old "blue" version and one for the new "green" version. Use your existing hello deployment for the "blue" version. The deployments will be accessed via a Service which will act as the router. Once the new "green" version is up and running, you'll switch over to using that version by updating the Service.

<br/>

    $ kubectl apply -f services/hello-blue.yaml
    $ kubectl create -f deployments/hello-green.yaml

    $ curl -ks https://`kubectl get svc frontend -o=jsonpath="{.status.loadBalancer.ingress[0].ip}"`/version


<br/>

    $ kubectl apply -f services/hello-green.yaml


Теперь всегда будет использоваться версия 2.0.0

    $ curl -ks https://`kubectl get svc frontend -o=jsonpath="{.status.loadBalancer.ingress[0].ip}"`/version

<br/>

**Blue-Green Rollback**

    $ kubectl apply -f services/hello-blue.yaml

А теперь все время 1.0.0

    $ curl -ks https://`kubectl get svc frontend -o=jsonpath="{.status.loadBalancer.ingress[0].ip}"`/version


<br/>

    $ kubectl get pods -o jsonpath --template='{range .items[*]}{.metadata.name}{"\t"}{"\t"}{.spec.containers[0].image}{"\n"}{end}'
    auth-6bb8dcd7bd-fpttf           kelseyhightower/auth:1.0.0
    frontend-77f46bf858-p77hp               nginx:1.9.14
    hello-5cbf94fc49-2j62n          kelseyhightower/hello:1.0.0
    hello-5cbf94fc49-sfzs8          kelseyhightower/hello:1.0.0
    hello-5cbf94fc49-tcm6w          kelseyhightower/hello:1.0.0
    hello-canary-67ddbf5d7c-ldm6h           kelseyhightower/hello:2.0.0
    hello-green-5bf7fc86ff-hdzx8            kelseyhightower/hello:2.0.0
    hello-green-5bf7fc86ff-mpwmn            kelseyhightower/hello:2.0.0
    hello-green-5bf7fc86ff-sww8j            kelseyhightower/hello:2.0.0


<br/>

### [GSP051] Continuous Delivery with Jenkins in Kubernetes Engine



