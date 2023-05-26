+++
title = "Deployment"
draft = false
weight = 5
+++

A Kubernetes `Deployment` provides declaritive updates to [Pods](/portfolio/kubernetes/pod/) (using [Replicasets](/portfolio/kubernetes/replicaset/)), with the ability to rollback when necessary, and zero downtime when using a rolling update strategy.


## Create a deployment {#create-a-deployment}

Imperative command:

```shell
kubectl create deployment $NAME --image=$IMAGE --namespace=default --replicas=3
```

Example manifest:

```yaml { linenos=inline, hl_lines=["2","13-17"] }
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
  strategy:
    rollingUpdate:
      maxSurge: 0
      maxUnavailable: 1
    type: RollingUpdate
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: docker.io/library/nginx:1.24.0
        imagePullPolicy: IfNotPresent
```

Resource output for a deployment:

```shell
kubectl get deployments.apps/$NAME --namespace=default
```

Show all objects created by a deployment (via selectors):

```shell
kubectl get all --namespace=default --selector=$LABELS
```

Imperative command to update a container image for a deployment:

```shell
kubectl set image deployments.apps/$NAME $CONTAINER=$IMAGE:$TAG
```
