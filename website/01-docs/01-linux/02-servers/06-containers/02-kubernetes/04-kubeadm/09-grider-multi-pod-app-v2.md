---
layout: page
title: Разворачиваем приложение из видео курса Stephen Grider Docker and Kubernetes The Complete Guide
permalink: /linux/servers/containers/kubernetes/kubeadm/grider-multi-pod-app-v2/
---

# Разворачиваем приложение из видео курса [Stephen Grider] Docker and Kubernetes: The Complete Guide [2018, ENG]

<br/>

Делаю: 18.04.2019

<br/>

**Обращаю внимание, что используется:**
kubernetesinc-ingress-nginx

<br/>

### KubernetesInc Ingress Nginx (Так не работает. Чего-то не хватает)

    $ kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/mandatory.yaml

<!-- $ kubectl get pods -n ingress-nginx

    kubectl get pods --all-namespaces -l app.kubernetes.io/name=ingress-nginx --watch

    $ kubectl logs -n ingress-nginx nginx-ingress-controller-5694ccb578-bfnz2

    $ kubectl exec -it -n ingress-nginx nginx-ingress-controller-5694ccb578-bfnz2 cat /etc/nginx/nginx.conf

    $ kubectl exec -it -n ingress-nginx nginx-ingress-controller-5694ccb578-bfnz2 cat /var/log/nginx/access.log

    $ kubectl exec -it -n ingress-nginx nginx-ingress-controller-5694ccb578-bfnz2 cat /var/log/nginx/error.log

-->

<!-- <br/>

    https://kubernetes.github.io/ingress-nginx/deploy/#bare-metal

    Bare-metal (Using NodePort)

    $ kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/provider/baremetal/service-nodeport.yaml -->

<br/>

```
$ cat <<EOF | kubectl apply -f -
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: kubernetesinc-ingress-nginx
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/rewrite-target: /
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
spec:
  rules:
    - host: nginx.example.com
      http:
        paths:
          - path: /
            backend:
              serviceName: client-cluster-ip-service
              servicePort: 3000
          - path: /api/
            backend:
              serviceName: server-cluster-ip-service
              servicePort: 5000
EOF
```

<br/>

    $ kubectl describe ing kubernetesinc-ingress-nginx

<br/>

    $ curl http://192.168.0.5 -H 'Host:nginx.example.com'
