---
layout: page
title: Запуск базы postgresql в minikube
permalink: /linux/containers/kubernetes/minikube/postgresql/
---

# Запуск базы postgresql в minikube

Делаю:  
30.11.2019

kubernetes v1.16.2

```
$ eval $(minikube docker-env)

$ docker pull postgres:10.5

$ kubectl get pv --all-namespaces
No resources found

$ kubectl get pvc --all-namespaces
No resources found


```

<br/>

**postgres-pv.yml**

```
$ cat <<EOF | kubectl create -f -
apiVersion: v1
kind: PersistentVolume
metadata:
  name: postgres-pv
  labels:
    type: local
spec:
  storageClassName: mystorage
  accessModes:
    - ReadWriteOnce
  capacity:
    storage: 2Gi
  hostPath:
    path: "/data/mypostgresdata/"
EOF

```

<br/>

**postgres-pvc.yml**

```
$ cat <<EOF | kubectl create -f -
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: postgres-pvc
  labels:
   app: postgres
spec:
  storageClassName: mystorage
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
EOF
```

<br/>

```
$ kubectl get pv/postgres-pv
NAME          CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                  STORAGECLASS   REASON   AGE
postgres-pv   2Gi        RWO            Retain           Bound    default/postgres-pvc   mystorage               4m8s
```

<br/>

**postgres-deployment.yml**

```
$ cat <<EOF | kubectl create -f -
apiVersion: apps/v1
kind: Deployment
metadata:
  name: postgres
spec:
  replicas: 1
  selector:
    matchLabels:
      app: postgres
  template:
    metadata:
      labels:
        app: postgres
    spec:
      containers:
      - name: postgres
        image: postgres:10.5
        imagePullPolicy: "IfNotPresent"
        env:
        - name: POSTGRES_DB
          value: postgresdb
        - name: POSTGRES_USER
          value: admin
        - name: POSTGRES_PASSWORD
          value: adminS3cret
        ports:
        - containerPort: 5432
          name: postgres
        volumeMounts:
          # mountPath within the container
        - name: postgres-pvc
          mountPath: "/var/lib/postgresql/data/:Z"
      volumes:
          # mapped to the PVC
        - name: postgres-pvc
          persistentVolumeClaim:
            claimName: postgres-pvc
EOF
```

<br/>

```
$ kubectl get pods
NAME                        READY   STATUS    RESTARTS   AGE
postgres-6df5c4bc5d-vmj2z   1/1     Running   0          73s
```

<br/>

```
$ kubectl port-forward <posgtres-pod> 5433:5432

```

<br/>

Запускаю в виртуалке minikube

```
$ docker run -e PGADMIN_DEFAULT_EMAIL='username' -e PGADMIN_DEFAULT_PASSWORD='password' -p 5555:80 --name pgadmin dpage/pgadmin4
```

<br/>

http://192.168.99.159:5555/

<br/>

https://github.com/burrsutter/9stepsawesome/blob/master/9_databases.adoc
