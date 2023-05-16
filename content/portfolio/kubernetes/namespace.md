+++
title = "Namespace"
draft = false
weight = 13
+++

A namespace is a mechanism to isolate groups of resources within a cluster.


## Create a namespace {#create-a-namespace}

Imperative command:

```shell
kubectl create namespace $NAME
```

Example manifest:

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: dev
```

Resource output for a namespace:

```shell
kubectl get namespaces/$NAME
```

To permanently change to a particular namespace:

```shell
kubectl config set-context --current --namespace=$NAME
```

View the active context (to identify the current namespace):

```shell
kubectl config get-contexts
```
