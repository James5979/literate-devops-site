+++
title = "Apply"
draft = false
weight = 19
+++

It is recommended to use a declarative approach when working with most Kubernetes objects, using the `kubectl apply` command:

```shell
kubectl apply --filename $FILE
```

Unlike the `kubectl create` command, the `kubectl apply` command will retain the previous writes made to a (live) Kubernetes object.
