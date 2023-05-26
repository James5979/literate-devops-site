+++
title = "ReplicationController"
draft = false
weight = 3
+++

A `ReplicationController` is responsible for keeping the total number of replicas constant.


## Create a replication controller {#create-a-replication-controller}

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
        image: docker.io/library/nginx:1.24.0
        imagePullPolicy: IfNotPresent
```

Resource output for a replication controller:

```shell
kubectl get replicationcontroller/$NAME --namespace=default
```

Show all objects created by a replication controller (via selectors):

```shell
kubectl get all --namespace=default --selector=$LABELS
```
