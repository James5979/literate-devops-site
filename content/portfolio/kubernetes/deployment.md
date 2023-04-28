+++
title = "Deployment"
draft = false
weight = 5
+++

A deployment provides declaritive updates for Pods and Replicasets, with zero downtime and rolebacks - whenever necessary.

A deployment is the recommended way to deploy Kubernetes applications.


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
        image: docker.io/library/nginx:latest
        imagePullPolicy: Never
```

Resource output for a deployment:

```shell
kubectl get deployments.apps/$NAME --namespace=default
```

Show all objects created by a deployment:

```shell
kubectl get all --namespace=default --selector=$LABELS
```

Update a container image used within a deployment:

```shell
kubectl set image deployments.apps/$NAME $CONTAINER=$IMAGE:$TAG
```
