+++
title = "ReplicationController"
draft = false
weight = 3
+++

A ReplicationController is responsible for keeping the total number of pod replicas contant.


## Create a replication controller {#create-a-replication-controller}

No imperative command.

Example manifest:

```yaml { linenos=inline }
apiVersion: v1
kind: ReplicationController
metadata:
  name: nginx
  namespace: default
  labels:
    app: nginx
spec:
  replicas: 3
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

Resource output for a replication controller:

```shell
kubectl get replicationcontroller/$NAME --namespace=default
```

Show all objects created by a replication controller:

```shell
kubectl get all --namespace=default --selector=$LABELS
```
