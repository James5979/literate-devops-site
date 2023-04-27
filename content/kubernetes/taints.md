+++
title = "Taints"
tags = ["command"]
draft = false
weight = 23
+++

Taint a node and mark it as non-schedulable:

```shell
kubectl taint nodes $NODE $LABEL:NoSchedule
```

Add multiple taints to a node:

```shell
kubectl taint nodes $NODE $LABEL1:NoSchedule $LABEL2:NoSchedule
```

Taint a node and prefer not to schedule any pods (without a toleration):

```shell
kubectl taint nodes $NODE $LABEL:PreferNoSchedule
```

Taint a node and evict any pods (without a toleration):

```shell
kubectl taint nodes $NODE $LABEL:NoExecute
```

View all taints on a particular node:

```shell
kubectl describe nodes/$NODE | grep Taints
```

Remove a taint from a node:

```shell
kubectl taint nodes $NODE $LABEL:NoSchedule-
```
