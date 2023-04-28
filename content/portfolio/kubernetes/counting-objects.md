+++
title = "Counting objects"
tags = ["command"]
draft = false
weight = 15
+++

To count the total number of objects in a resource (per namespace),
use the `--no-headers` option and pipe the output to the `wc --lines` command:

```shell
kubectl get $RESOURCE --namespace=$NAMESPACE --no-headers | wc --lines
```
