+++
title = "Replace command"
tags = ["command"]
draft = false
weight = 18
+++

To imperatively replace a Kubernetes object (with a new object):

```shell
kubectl replace --filename $FILE
```

To completely delete and recreate an existing Kubernetes object:

```shell
kubectl replace --filename $FILE --force
```
