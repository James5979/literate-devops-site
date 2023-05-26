+++
title = "Pod"
description = "Kubernetes pods."
draft = false
weight = 1
+++

A `Pod` is the smallest unit of compute in a Kubernetes cluster.

Each pod encapsulates one or more containers, and is accompanied by both metadata and a specification, containing all the necessary information to fully define an application.


## Create a pod {#create-a-pod}

Imperative command:

```shell
kubectl run $NAME --image=$IMAGE --labels=$LABELS --namespace=default
```

Example manifest:

```yaml { linenos=inline }
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  namespace: default
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: docker.io/library/nginx:1.24.0
```

Resource output for a pod:

```shell
kubectl get pods/$NAME --namespace=default
```


## References {#references}

-   [Unsplash](https://unsplash.com/photos/RDIa_qFpWHc)
