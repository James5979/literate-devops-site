+++
title = "Documentation"
draft = false
weight = 12
+++

Use the `kubectl explain` command to view the documentation for a resource.

**Examples**

Show the documentation for a `ReplicaSet`:

```shell
kubectl explain ReplicaSet
```

Show the documentation for the `spec` dictionary within a `ReplicaSet`:

```shell
kubectl explain ReplicaSet.spec
```

You can optionally choose to display this output as nested fields instead:

```shell
kubectl explain ReplicaSet.spec --recursive
```
