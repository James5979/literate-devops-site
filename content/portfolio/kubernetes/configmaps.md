+++
title = "ConfigMaps"
draft = false
weight = 42
+++

A `ConfigMap` is an API object used to store non-confidential data as key-value pairs. --- [1]


## Create a configmap and specify multiple key-value pairs {#create-a-configmap-and-specify-multiple-key-value-pairs}

Imperative command:

```shell
kubectl create configmap $NAME --dry-run=client --from-literal=$ENV1 --from-literal=$ENV2 --output=yaml
```

Example manifest:

```yaml { linenos=inline }
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-configmap
data:
  APP: busybox
  TEST: success
```

Example manifest (using a multi-line value):

```yaml { linenos=inline }
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-configmap
data:
  key1: Hello, world!
  key2: |
    Test
    multiple lines
    more lines
```

Resource output for a configmap:

```shell
kubectl get configmaps/$NAME --namespace=default
```

Describe a configmap:

```shell
kubectl describe configmaps/$NAME --namespace=default
```


## References {#references}

1.  [kubernetes.io](https://kubernetes.io/docs/concepts/configuration/configmap/)
