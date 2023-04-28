+++
title = "Labels"
tags = ["command"]
draft = false
weight = 25
+++

Labels are key-value pairs associated with Kubernetes objects. They allow the classification of objects, e.g. defining organisational structure.

List all labels associated with an object:

```shell
kubectl label nodes/$OBJECT --list
```

List all labels associated with an object using the `kubectl get` command:

```shell
kubectl get $RESOURCE/$NAME --show-labels
```

Count the total number of labels for an object:

```shell
kubectl label $RESOURCE/$OBJECT --list | wc --lines
```

Add new labels:

```shell
kubectl label $RESOURCE/$OBJECT1 $LABEL1 $LABEL2
kubectl label $RESOURCE/$OBJECT2 $LABEL2 $LABEL3
```

Remove existing labels:

```shell
kubectl label --overwrite $RESOURCE/$OBJECT1 ${KEY1}- ${KEY2}-
kubectl label --overwrite $RESOURCE/$OBJECT2 ${KEY2}- ${KEY3}-
```
