+++
title = "Apply command"
tags = ["command"]
draft = false
weight = 19
+++

The best way to create and make changes to a Kubernetes object is to use the declarative approach, via the **apply** command.

This approach allows you to create Kubernetes objects that can be updated when changes are made to their manifests without having to fully replace the Kubernetes objects like is with the case when using the imperative approach.

```shell
kubectl apply --filename $FILE
```
