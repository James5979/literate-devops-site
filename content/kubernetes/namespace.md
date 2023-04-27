+++
title = "Namespace"
draft = false
weight = 13
+++

A namespace is a mechanism to isolate groups of resources in a cluster.


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

To change to a certain namespace, permanently:

```shell
kubectl config set-context --current --namespace=$NAME
```

Show contexts to see what the current namespace is set to:

```shell
kubectl config get-contexts
```
