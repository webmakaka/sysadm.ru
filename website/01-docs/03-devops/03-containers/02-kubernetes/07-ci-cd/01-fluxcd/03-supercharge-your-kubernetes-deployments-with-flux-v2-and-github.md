---
layout: page
title: Supercharge your Kubernetes deployments with Flux v2 and GitHub
description: Supercharge your Kubernetes deployments with Flux v2 and GitHub
keywords: linux, kubernetes, FluxCD, Kustomize
permalink: /devops/containers/kubernetes/ci-cd/fluxcd/supercharge-your-kubernetes-deployments-with-flux-v2-and-github/
---

# Supercharge your Kubernetes deployments with Flux v2 and GitHub

<br/>

https://www.youtube.com/watch?v=N6UCKF7JD7k&list=PLG9qZAczREKmCq6on_LG8D0uiHMx1h3yn&index=1

# 01-Supercharge your Kubernetes deployments with Flux v2 and GitHub - Introduction

```
$ curl -s https://toolkit.fluxcd.io/install.sh | sudo bash

$ export INGRESS_HOST=$(minikube --profile my-profile ip)

$ echo ${INGRESS_HOST}

$ export GITHUB_USER=<YOUR_GITHUB_USERNAME>

$ export GITHUB_TOKEN=<YOUR_TOKEN>
```

<br/>

```
$ cd ~
```

```
$ flux check --pre
► checking prerequisites
✔ kubectl 1.20.2 >=1.18.0
✔ Kubernetes 1.20.2 >=1.16.0
✔ prerequisites checks passed
```

<br/>

```
$ flux bootstrap github \
    --owner=${GITHUB_USER} \
    --repository=flux-infra \
    --branch=main \
    --path=app-cluster \
    --personal
```

<br/>

# 02-Kubernetes deployments with Flux v2 introduction to kustomize

<br/>

```
$ curl -s "https://raw.githubusercontent.com/\
kubernetes-sigs/kustomize/master/hack/install_kustomize.sh"  | bash && chmod +x kustomize && sudo mv kustomize /usr/local/bin/
```

<br/>

```
$ mkdir -p ~/tmp && cd ~/tmp

$ git clone https://github.com/webmak1/realtimeapp-infra

$ cd ~/tmp/realtimeapp-infra/deploy/overlays/dev
$ kustomize build

// $ kustomize build | kubeclt apply -f -
```

<br/>

# 03-Kubernetes deployments with Flux v2 Deploying Manifests

```
$ mkdir -p ~/project/dev && cd ~/project/dev
$ git clone https://github.com/${GITHUB_USER}/flux-infra
$  cd flux-infra
```

<br/>

```
$ flux create source git realtimeapp-infra \
    --url https://github.com/$GITHUB_USER/realtimeapp-infra \
    --branch main \
    --interval 30s \
    --export > ./app-cluster/realtimeapp-source.yaml
```

<br/>

```
$ cat ./app-cluster/realtimeapp-source.yaml
```

<br/>

```
$ flux create kustomization realtimeapp-dev \
    --source realtimeapp-infra \
    --path "./deploy/overlays/dev" \
    --prune true \
    --validation client \
    --interval 1m \
    --health-check="Deployment/realtime-dev.realtime-dev" \
    --health-check="Deployment/redis-dev.realtime-dev" \
    --health-check-timeout=2m \
    --export > ./app-cluster/realtimeapp-dev.yaml
```

<br/>

```
$ flux create kustomization realtimeapp-prd \
    --source realtimeapp-infra \
    --path "./deploy/overlays/prd" \
    --prune true \
    --validation client \
    --interval 1m \
    --health-check="Deployment/realtime-prd.realtime-prd" \
    --health-check="Deployment/redis-prd.realtime-prd" \
    --health-check-timeout=2m \
    --export > ./app-cluster/realtimeapp-prd.yaml
```

<br/>

```
$ cat ./app-cluster/realtimeapp-source.yaml
```

<br/>

```
$ git add --all

$ git commit -m "Added source and kustomization"

$ git push
```

<br/>

```
$ watch flux get kustomizations
```

<br/>

```
$ kubectl get ns
NAME              STATUS   AGE
default           Active   6d9h
flux-system       Active   18m
kube-node-lease   Active   6d9h
kube-public       Active   6d9h
kube-system       Active   6d9h
realtime-dev      Active   82s
realtime-prd      Active   83s
```

<br/>

```
$ kubectl get pods -n realtime-dev
NAME                          READY   STATUS    RESTARTS   AGE
realtime-dev-869b5674-h6gd5   1/1     Running   0          115s
redis-dev-589977c5c6-7pccx    1/1     Running   0          115s
```

<br/>

```
$ flux reconcile kustomization realtimeapp-dev
```

<br/>

https://github.com/webmak1/realtimeapp

New Release

1.0.2

Publish

В Actions должен пойти билд.

Там срабатывает kustomize edit который меняет версию.

<br/>

# 04-Kubernetes deployments with Flux v2 Monitoring and Alerting

(Пропустил этот шаг)

<br/>

```
$ cd ~/project/dev/flux-infra
```

<br/>

```
$ flux create source git monitoring \
    --url https://github.com/fluxcd/flux2 \
    --branch main \
    --interval 30m \
    --export > ./app-cluster/monitor-source.yaml
```

<br/>

```
$ flux create kustomization monitoring \
    --source monitoring \
    --path "./manifests/monitoring" \
    --prune true \
    --interval 1h \
    --health-check="Deployment/prometheus.flux-system" \
    --health-check="Deployment/grafana.flux-system" \
    --export > ./app-cluster/monitor-kustomization.yaml
```

<br/>

```
$ watch flux get kustomizations
```

<br/>

```
$ kubectl -n flux-system port-forward svc/grafana 3000:3000
```

<br/>

Далее Alerting, какие-то teams и т.д. Наверное, имеет смысл пересмотреть.

<br/>

```
$ flux get alert-providers
```

<br/>

# 05-Kubernetes Deployments with Flux v2 Helm Basics

<br/>

```
$ flux create source helm bitnami \
    --url https://charts.bitnami.com/bitnami \
    --interval 1m0s \
    --export > ./app-cluster/helmrepo-bitnami.yaml
```

<br/>

```
$ flux create helmrelease redis \
    --source=HelmRepository/bitnami \
    --chart redis \
    --release-name redis \
    --target-namespace default \
    --interval 5m0s \
    --export > ./app-cluster/helmrelease-redis.yaml
```

<br/>

```
$ flux get sources helm
```

<br/>

```
$ export HELM_EXPERIMENTAL_OCI=1
$ helm chart save . realtimeapp:1.0.2

// Отправляем в helm registry
$ helm chart save . gebareg.azurecr.io/realtimeapp:1.0.2
$ helm registry login gebareg.azurecr.io
$ helm chart push gebareg.azurecr.io/realtimeapp:1.0.2


// Защищенный паролем репо
$ kubectl create secret generic acr --from-literal username=gebareg --from-literal "password=THEPASSWORD" -n flux-system
```

<br/>

```
$ flux create source helm realtimeapp \
    --url https://gebareg.auzrecr.io/helm/v1/repo/ \
    --interval 1m0s \
    --secret-ref acr \
    --export > ./app-cluster/helmrepo-realtimeapp.yaml
```

<br/>

```
$ flux create helmrelease realtimeapp \
    --source=HelmRepository/realtimeapp \
    --chart realtimeapp \
    --release-name realtimeapp \
    --target-namespace default \
    --interval 5m0s \
    --export > ./app-cluster/helmrelease-realtimeapp.yaml
```

Добавили некоторые изменения в конфиги редиса и realtimeapp.
