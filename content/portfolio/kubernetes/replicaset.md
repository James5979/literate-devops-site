+++
title = "ReplicaSet"
draft = false
weight = 4
+++

A ReplicaSet ensures that there is always a stable number of pods based on the number of replicas.

**Note**: replicasets are very similar to [replicationcontrollers](/portfolio/kubernetes/replicationcontroller/), with the exception that a replicaset can adopt pre-existing pods (using a selector).


## Create a replicaset {#create-a-replicaset}

No imperative command.

Example manifest:

```yaml { linenos=inline }
apiVersion: apps/v1
kind: ReplicaSet
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

Resource output for a replicaset:

```shell
kubectl get replicasets.apps/$NAME --namespace=default
```

Show all objects created by a replicaset (using labels):

```shell
kubectl get all --namespace=default --selector=$LABELS
```
