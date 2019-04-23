---
layout: page
title: Services - ClusterIP, NodePort, LoadBalancer, Ingress
permalink: /linux/servers/containers/kubernetes/basics/services/
---

# Services: ClusterIP, NodePort, LoadBalancer, Ingress

<br/>

### Kubernetes - Services Explained

<div align="center">

    <iframe width="853" height="480" src="https://www.youtube.com/embed/5lzUpDtmWgM" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

</div>

![kubernetes Services](/img/linux/servers/containers/kubernetes/basics/services/services.png "kubernetes Services"){: .center-image }

<br/>

### ClusterIP

![kubernetes ClusterIP](/img/linux/servers/containers/kubernetes/basics/services/clusterIP.png "kubernetes ClusterIP"){: .center-image }

<br/>

```
apiVersion: v1
kind: Service
metadata:
  name: my-internal-service
spec:
  selector:
    app: my-app
  type: ClusterIP
  ports:
  - name: http
    port: 80
    targetPort: 80
    protocol: TCP
```

<br/>

### NodePort

![kubernetes NodePort](/img/linux/servers/containers/kubernetes/basics/services/NodePort.png "kubernetes NodePort"){: .center-image }

<br/>

```
apiVersion: v1
kind: Service
metadata:
  name: my-nodeport-service
spec:
  selector:
    app: my-app
  type: NodePort
  ports:
  - name: http
    port: 80
    targetPort: 80
    nodePort: 30036
    protocol: TCP
```

<br/>

### LoadBalancer

![kubernetes LoadBalancer](/img/linux/servers/containers/kubernetes/basics/services/LoadBalancer.png "kubernetes LoadBalancer"){: .center-image }

<br/>

```
kind: Service
apiVersion: v1
metadata:
    name: loadbalancer-service
spec:
    selector:
        app: app-lb
    ports:
    - protocol: TCP
      port: 80
      targetPort: 9376
    clusterIP: <internalIP>
    loadBalancerIP: <externalIP>
    type: LoadBalancer
```

<br/>

### Ingress

![kubernetes Ingress](/img/linux/servers/containers/kubernetes/basics/services/Ingress.png "kubernetes Ingress"){: .center-image }

<br/>

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: my-ingress
spec:
  backend:
    serviceName: other
    servicePort: 8080
  rules:
  - host: foo.mydomain.com
    http:
      paths:
      - backend:
          serviceName: foo
          servicePort: 8080
  - host: mydomain.com
    http:
      paths:
      - path: /bar/*
        backend:
          serviceName: bar
          servicePort: 8080
```

<br/>

**Скапитализжено:**

https://medium.com/google-cloud/kubernetes-nodeport-vs-loadbalancer-vs-ingress-when-should-i-use-what-922f010849e0
