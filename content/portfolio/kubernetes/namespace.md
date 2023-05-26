+++
title = "Namespace"
draft = false
weight = 13
+++

A `Namespace` is a mechanism used to isolate groups of resources within a single cluster. --- [1]


## Create a namespace {#create-a-namespace}

Imperative command:

```shell
kubectl create namespace $NAME
```

Example manifest:

```yaml { linenos=inline }
apiVersion: v1
kind: Namespace
metadata:
  name: dev
```

Resource output for a namespace:

```shell
kubectl get namespaces/$NAME
```

Change the current namespace:

```shell
kubectl config set-context --current --namespace=$NAME
```

View contexts (and identify the current namespace):

```shell
kubectl config get-contexts
```


## References {#references}

1.  [kubernetes.io](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/)
