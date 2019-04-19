---
layout: page
title: KubernetesInc Ingress Nginx
permalink: /linux/servers/containers/kubernetes/kubeadm/ingress/kubernetesinc-ingress-nginx/
---

# KubernetesInc Ingress Nginx

Делаю  
19.04.2019

<br/>

    $ kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/mandatory.yaml

<br/>

    $ kubectl get pods --all-namespaces | grep ingress-nginx
    ingress-nginx   nginx-ingress-controller-5694ccb578-2jqw2   1/1     Running   0          97s

<br/>

    $ mkdir -p ~/tmp/ingress && cd ~/tmp/ingress

<br/>

    # vi web-01.yaml

```
apiVersion: v1
kind: Pod
metadata:
  name: web-01
  labels:
    app: nginx

spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80
```

<br/>

    $ kubectl create -f web-01.yaml

<br/>

    $ kubectl expose pod web-01 --type="ClusterIP" --port 80

<br/>

    $ kubectl get services
    NAME         TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)   AGE
    kubernetes   ClusterIP   10.96.0.1        <none>        443/TCP   3h10m
    web-01       ClusterIP   10.108.124.242   <none>        80/TCP    12s

<br/>

    # vi ingress.yaml

```
apiVersion: v1
kind: Service
metadata:
  name: ingress-nginx
  namespace: ingress-nginx
  labels:
    app.kubernetes.io/name: ingress-nginx
    app.kubernetes.io/part-of: ingress-nginx
spec:
  type: NodePort
  ports:
  - name: http-new
    port: 80
    targetPort: 80
    protocol: TCP
    nodePort: 30090
  selector:
    app.kubernetes.io/name: ingress-nginx
    app.kubernetes.io/part-of: ingress-nginx
```

<br/>

    $ kubectl create -f ingress.yaml

<br/>

    $ kubectl get service --all-namespaces
    NAMESPACE       NAME            TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                  AGE
    default         kubernetes      ClusterIP   10.96.0.1        <none>        443/TCP                  3h15m
    default         web-01          ClusterIP   10.108.124.242   <none>        80/TCP                   4m52s
    ingress-nginx   ingress-nginx   NodePort    10.96.176.174    <none>        80:30090/TCP             21s
    kube-system     kube-dns        ClusterIP   10.96.0.10       <none>        53/UDP,53/TCP,9153/TCP   3h15m

<br/>

### Now we will define the rules for ingress

<br/>

    # vi ingress-rules.yaml

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: ingress-test
  annotations:
    kubernetes.io/ingress.class: nginx
    ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - host: kube-master.labs.local
    http:
      paths:
        - path: /
          backend:
            serviceName: web-01
            servicePort: 80
```

<br/>

    $ kubectl create -f ingress-rules.yaml

<br/>

    $ kubectl get ingress
    NAME           HOSTS                    ADDRESS   PORTS   AGE
    ingress-test   kube-master.labs.local             80      91s

<br/>

    $ sudo su -
    # vi /etc/hosts

    192.168.0.11 kube-master.labs.local

<br/>

192.168.0.11 - одна из нод кластера.

<br/>

    $ curl http://192.168.0.11:30090 -H 'Host:kube-master.labs.local'
