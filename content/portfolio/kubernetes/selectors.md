+++
title = "Selectors"
draft = false
weight = 22
+++

Identify sets of Kubernetes objects using selectors.

Show output for matching labels:

```shell
kubectl get pods --selector=$LABEL1,$LABEL2,$LABEL3
```

**Note**: the comma syntax above is an **AND** statement.

To count the total number of objects that match a certain label:

```shell
kubectl get all --no-headers --selector=$LABEL | wc --lines
```
