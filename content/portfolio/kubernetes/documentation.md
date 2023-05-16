+++
title = "Kubernetes documentation"
tags = ["command", "tip"]
draft = false
weight = 12
+++

Use the `kubectl explain` command to view the documentation for a resource.

**Examples**

Show the documentation for a `ReplicaSet`:

```shell
kubectl explain ReplicaSet
```

Show the documentation for the spec of a `ReplicaSet`:

```shell
kubectl explain ReplicaSet.spec
```

You can optionally choose to display the output with nested fields:

```shell
kubectl explain ReplicaSet.spec --recursive
```
