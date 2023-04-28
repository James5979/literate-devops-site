+++
title = "Kubernetes documentation"
tags = ["command", "tip"]
draft = false
weight = 12
+++

Use the `kubectl explain` command to output Kubernetes documentation for a particular resource.

**Examples**

Show the documentation for the `ReplicaSet` resource:

```shell
kubectl explain ReplicaSet | head --lines=10
```

Recursive output shows all nested fields within the resource:

```shell
kubectl explain ReplicaSet.spec --recursive
```
