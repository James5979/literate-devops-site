+++
title = "Selectors"
draft = false
weight = 22
+++

Show output matching only the following labels:

```shell
kubectl get pods --selector=$LABEL1,$LABEL2,$LABEL3
```

**Note**: the comma syntax above refers to an **AND** statement.

To count the total number of objects that match a certain label:

```shell
kubectl get all --no-headers --selector=$LABEL | wc --lines
```
