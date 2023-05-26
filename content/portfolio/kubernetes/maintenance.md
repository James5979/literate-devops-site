+++
title = "Maintenance"
draft = false
weight = 51
+++

Preparing a node for maintenance.

Drain a node of all pods and then cordon it for maintenance:

```shell
kubectl drain nodes/$NAME --force --ignore-daemonsets
```

Resource output for a node:

```shell
kubectl get nodes/$NAME
```

To uncordon a node:

```shell
kubectl uncordon nodes/$NAME
```

**Tip**: you can always delete pods to attempt a rebalance of the cluster.

To disable scheduling on a node (without draining pods):

```shell
kubectl cordon nodes/$NAME
```

Resource output to display what nodes are unschedulable:

```shell
kubectl get nodes,pods --output=custom-columns=NAME:metadata.name,NODE:spec.nodeName,Unschedulable:spec.unschedulable
```
