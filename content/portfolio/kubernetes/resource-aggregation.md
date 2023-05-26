+++
title = "Resource aggregation"
draft = false
weight = 39
+++

Kubernetes allows you to display multiple resources at the same time by separating each resource using a comma.

**Example**

Show **both** pods and services that match a selector:

```shell
kubectl get pods,services --namespace=default --selector=$LABELS
```
