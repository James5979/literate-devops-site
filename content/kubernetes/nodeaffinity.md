+++
title = "Node affinity"
tags = ["tip"]
draft = false
weight = 27
+++

Node affinity affects what nodes a pod can be scheduled on, using a selection of labels and operator logic.

Example manifest:

```yaml { linenos=inline, hl_lines=["12-29"] }
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  namespace: default
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: compute
            operator: In
            values:
            - large
            - medium
          - key: compute
            operator: NotIn
            values:
            - small
          - key: gpu
            operator: Exists
          - key: quantum
            operator: DoesNotExist
```

**CKA Exam Tip**: node affinity requires that you reference the [kubernetes.io](https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/) documentation, since no imperative command exists. You can always use the `kubectl explain` command to infer what fields you should use.

**Tip**: use a combination of taints and tolerations with node affinity to ensure that specific workloads can only be scheduled on certain nodes.
