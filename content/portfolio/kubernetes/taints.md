+++
title = "Taints"
draft = false
weight = 23
+++

Taints are used to repel pods during scheduling.

Taint a node and mark it as non-schedulable:

```shell
kubectl taint nodes $NODE $LABEL:NoSchedule
```

Add multiple taints to a node:

```shell
kubectl taint nodes $NODE $LABEL1:NoSchedule $LABEL2:NoSchedule
```

Taint a node and prefer not to schedule pods:

```shell
kubectl taint nodes $NODE $LABEL:PreferNoSchedule
```

Taint a node and evict any pods that do **not** have a toleration:

```shell
kubectl taint nodes $NODE $LABEL:NoExecute
```

View all taints that exist on a node:

```shell
kubectl describe nodes/$NODE | grep Taints
```

Remove a taint from a node:

```shell
kubectl taint nodes $NODE $LABEL:NoSchedule-
```
