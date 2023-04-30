+++
title = "Deployment"
draft = false
weight = 5
+++

A Kubernetes deployment provides declaritive updates for [Pods](/portfolio/kubernetes/pod/) and [Replicasets](/portfolio/kubernetes/replicaset/), with zero downtime during updates and the ability to rollback when necessary.


## Create a deployment {#create-a-deployment}

Imperative command:

```shell
kubectl create deployment $NAME --image=$IMAGE --namespace=default --replicas=3
```

Example manifest:

```yaml { linenos=inline }
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
  namespace: default
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: docker.io/library/nginx:1.24.0
        imagePullPolicy: Never
```

Resource output for a deployment:

```shell
kubectl get deployments.apps/$NAME --namespace=default
```

Show all objects created by a deployment (using labels):

```shell
kubectl get all --namespace=default --selector=$LABELS
```

Imperatively update a container image within a deployment:

```shell
kubectl set image deployments.apps/$NAME $CONTAINER=$IMAGE:$TAG
```
