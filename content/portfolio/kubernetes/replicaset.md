+++
title = "ReplicaSet"
draft = false
weight = 4
+++

A `ReplicaSet` ensures that there is always a stable number of [pods](/portfolio/kubernetes/pod/) based on the number of replicas.

**Note**: ReplicaSets are very similar to [ReplicationControllers](/portfolio/kubernetes/replicationcontroller/), with the exception being that a replicaset uses a selector to keep track of pod replicas.


## Create a replicaset {#create-a-replicaset}

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
        imagePullPolicy: IfNotPresent
```

Resource output for a replicaset:

```shell
kubectl get replicasets.apps/$NAME --namespace=default
```

Show all objects created by a replicaset (via selectors):

```shell
kubectl get all --namespace=default --selector=$LABELS
```
