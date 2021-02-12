---
layout: page
title: Kubernetes ArgoCD
description: Kubernetes ArgoCD
keywords: linux, kubernetes, ArgoCD
permalink: /devops/containers/kubernetes/ci-cd/argocd/applying-gitops-principles-to-manage-production-environment-in-kubernetes/
---

# Kubernetes ArgoCD

### Argo CD - Applying GitOps Principles To Manage Production Environment In Kubernetes

<br/>

Делаю:  
12.02.2021

<br/>

### Пока не работает!

<br/>

https://www.youtube.com/watch?v=vpWQeoaiRM4

<br/>

Поставил Istio, как в <a href="/devops/containers/kubernetes/service-mesh/istio/minikube/setup/">доке</a>

<br/>

```
$ export INGRESS_HOST=$(kubectl \
 --namespace istio-system \
 get service istio-ingressgateway \
 --output jsonpath="{.status.loadBalancer.ingress[0].ip}")

$ echo ${INGRESS_HOST}
```

<br/>

Получаю

192.168.49.20

<br/>

Заменяю в скриптах.

<br/>

```
$ kubectl create namespace production
```

<br/>

### Создаю repo на гитхаб "argocd-15-min"

```
$ cd ~/tmp
$ mkdir argocd-15-min && cd argocd-15-min
```

<br/>

```
$ vi Chart.yaml
```

<br/>

```yaml
apiVersion: v1
description: Production environment
name: production
version: '1.0.0'
```

<br/>

```
$ mkdir -p templates && cd templates
```

<br/>

```
$ vi devops-toolkit.yaml
```

<br/>

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
    name: devops-toolkit
    namespace: argocd
    finalizers:
        - resources-finalizer.argocd.argoproj.io
spec:
    project: production
    source:
        path: helm
        repoURL: https://github.com/vfarcic/devops-toolkit.git
        targetRevision: HEAD
        helm:
            values: |
                image:
                  tag: latest
                ingress:
                  host: devops-toolkit.192.168.49.20.xip.io
            version: v3
    destination:
        namespace: production
        server: https://kubernetes.default.svc
    syncPolicy:
        automated:
            selfHeal: true
            prune: true
```

<br/>

```
$ vi devops-paradox.yaml
```

<br/>

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
    name: devops-paradox
    namespace: argocd
    finalizers:
        - resources-finalizer.argocd.argoproj.io
spec:
    project: production
    source:
        path: helm
        repoURL: https://github.com/vfarcic/devops-paradox.git
        targetRevision: HEAD
        helm:
            values: |
                image:
                  tag: latest
                ingress:
                  host: devops-paradox.192.168.49.20.xip.io
            version: v3
    destination:
        namespace: production
        server: https://kubernetes.default.svc
    syncPolicy:
        automated:
            selfHeal: true
            prune: true
```

<br/>

Отправить репо на гитхаб.

<br/>

```
$ vi apps.yaml
```

<br/>

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
    name: devops-toolkit
    namespace: argocd
    finalizers:
        - resources-finalizer.argocd.argoproj.io
spec:
    project: production
    source:
        path: helm
        repoURL: https://github.com/webmak1/argocd-15-min.git
        targetRevision: HEAD
    destination:
        namespace: production
        server: https://kubernetes.default.svc
    syncPolicy:
        automated:
            selfHeal: true
            prune: true
```

<br/>

```
$ kubectl apply -n argocd -f ./apps.yaml
```

<br/>

http://argocd.$INGRESS_HOST.xip.io

http://devops-toolkit.$INGRESS_HOST.xip.io

<br/>

Меняем версию с latest на 2.9.17 в devops devops-toolkit.yaml

<br/>

```
$ watch 'kubectl --namespace production sget deployment devops-toolkit-devops-toolkit \
--output jsonpath="{.spec.temlate.spec.containers[0].image}"'
```
