+++
title = "Labels"
draft = false
weight = 25
+++

Labels are key-value pairs associated with Kubernetes objects. They allow the classification of objects, e.g. for defining organisational structure, etc.

List all labels associated with an object:

```shell
kubectl label nodes/$OBJECT --list
```

List all labels associated with an object using the `kubectl get` command:

```shell
kubectl get $RESOURCE/$NAME --show-labels
```

Count the total number of labels on an object:

```shell
kubectl label $RESOURCE/$OBJECT --list | wc --lines
```

Add labels to a live Kubernetes object:

```shell
kubectl label $RESOURCE/$NAME $LABEL1 $LABEL2
```

Remove existing labels:

```shell
kubectl label --overwrite $RESOURCE/$NAME ${KEY1}- ${KEY2}-
```
