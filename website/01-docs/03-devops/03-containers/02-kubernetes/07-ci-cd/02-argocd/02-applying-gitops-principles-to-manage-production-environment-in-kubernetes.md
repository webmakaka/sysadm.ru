---
layout: page
title: Kubernetes ArgoCD
description: Kubernetes ArgoCD
keywords: linux, kubernetes, ArgoCD
permalink: /devops/containers/kubernetes/ci-cd/argocd/applying-gitops-principles-to-manage-production-environment-in-kubernetes/
---

# Kubernetes ArgoCD

### Argo CD - Applying GitOps Principles To Manage Production Environment In Kubernetes

https://www.youtube.com/watch?v=vpWQeoaiRM4

<br/>

```
$ kubectl create namespace production
```

<br/>

```
$ mkdir argo
$ cd argo
```

<br/>

```
$ vi Chart.yaml
```

<br/>

```
apiVersion: v1
description: Production environment
name: production
version: "1.0.0"
```

<br/>

```
$ cd ~/tmp
$ mkdir argocd-15-min && cd argocd-15-min
$ mkdir helm/templates && cd helm/templates
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
#                  repository: vfarcic/devops-toolkit
                  tag: latest
                ingress:
                  host: devopst-toolkit.192.168.64.14.xip.io
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
#                  repository: vfarcic/devops-toolkit
                  tag: latest
                ingress:
                  host: devopst-paradox.192.168.64.14.xip.io
            version: v3
    destination:
        namespace: production
        server: https://kubernetes.default.svc
    syncPolicy:
        automated:
            selfHeal: true
            prune: true
```

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
        repoURL: https://github.com/vfarcic/argocd-15-min.git
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

Отправить репо на гитхаб.

<br/>

```
$ kubectl apply --filename appx.yaml
```

http://argocd.$INGRESS_HOST.xip.io

http://devops-toolkit.$INGRESS_HOST.xip.io

<br/>

Меняем версию с latest на 2.9.17 в devops devops-toolkit.yaml

<br/>

```
$ watch 'kubectl --namespace production sget deployment devops-toolkit-devops-toolkit \
--output jsonpath="{.spec.temlate.spec.containers[0].image}"'
```
